#!/usr/bin/env python3
"""
flatten_purview_export.py
=========================

Convert a MANUAL Microsoft Purview audit-log export into the exact CSV format
that the "AI in One" Power BI report expects.

WHY THIS IS NEEDED
------------------
The report's Power Query was built for the Graph API exporter (Export-CopilotData.ps1),
whose records use the columns: createdDateTime / auditLogRecordType / operation / auditData.

A *manual* export from the Purview / Compliance portal Audit search produces an .xlsx
(or CSV) with DIFFERENT column names -- e.g. `Operations` (plural), `UserIds`, and often
NO `RecordType` column at all. The report's load step does:

    Table.SelectColumns(t, {"CreationDate", "AuditData", "RecordType", "Operation"})

with NO "MissingField.Ignore", so any renamed/missing column throws:

    "The column 'XXX' of the table wasn't found."

This script reads the manual export (.xlsx / .xls / .csv / .json), pulls the canonical
fields (falling back to the AuditData JSON blob when a column is missing), re-serializes
the AuditData as clean single-line JSON, and writes a UTF-8 CSV with EXACTLY these headers:

    CreationDate, AuditData, RecordType, Operation

That file loads cleanly into the report with no field-recognition errors.

USAGE
-----
    python flatten_purview_export.py "AuditExport.xlsx"
    python flatten_purview_export.py "AuditExport.xlsx" -o "CopilotData-clean.csv"
    python flatten_purview_export.py "AuditExport.xlsx" --sheet "Audit"
    python flatten_purview_export.py "records.json"

Then point the report's "Copilot Interactions File" parameter at the output CSV.

REQUIREMENTS
------------
    pip install openpyxl        # only needed for .xlsx / .xls inputs
"""

from __future__ import annotations

import argparse
import csv
import json
import sys
from datetime import datetime
from pathlib import Path

# Canonical output headers required by the report's Power Query, in order.
CANONICAL_HEADERS = ["CreationDate", "AuditData", "RecordType", "Operation"]

# Accepted source-column name variants (compared case-insensitively, trimmed).
CREATION_KEYS = {"creationdate", "creationtime", "createddatetime", "creationdatetime"}
AUDIT_KEYS = {"auditdata", "audit_data", "auditdatajson", "audit data", "auditdata (json)"}
RECORDTYPE_KEYS = {"recordtype", "auditlogrecordtype", "record type"}
OPERATION_KEYS = {"operation", "operations"}

# Excel hard-truncates any cell at 32,767 characters. Large Copilot AuditData
# blobs exceed this and get silently corrupted -- we detect and report it.
EXCEL_CELL_LIMIT = 32767


def _norm(name: object) -> str:
    return str(name).strip().lower() if name is not None else ""


# --------------------------------------------------------------------------- #
# Input readers -- each yields dict rows {header: value}
# --------------------------------------------------------------------------- #
def read_xlsx(path: Path, sheet: str | None):
    try:
        from openpyxl import load_workbook
    except ImportError:
        sys.exit(
            "ERROR: reading .xlsx requires the 'openpyxl' package.\n"
            "       Install it with:  pip install openpyxl"
        )

    wb = load_workbook(path, read_only=True, data_only=True)
    ws = wb[sheet] if sheet else wb.active
    rows = ws.iter_rows(values_only=True)

    try:
        header_cells = next(rows)
    except StopIteration:
        return
    headers = [str(c).strip() if c is not None else "" for c in header_cells]

    for r in rows:
        if r is None:
            continue
        if all(c is None or str(c).strip() == "" for c in r):
            continue  # skip blank rows
        yield dict(zip(headers, r))


def read_csv(path: Path):
    # utf-8-sig transparently strips a BOM if present.
    with path.open("r", encoding="utf-8-sig", newline="") as f:
        reader = csv.DictReader(f)
        for row in reader:
            yield row


def read_json(path: Path):
    with path.open("r", encoding="utf-8-sig") as f:
        text = f.read()
    try:
        data = json.loads(text)
    except json.JSONDecodeError:
        # Maybe NDJSON (one JSON record per line)
        for line in text.splitlines():
            line = line.strip()
            if line:
                yield json.loads(line)
        return

    if isinstance(data, dict) and "value" in data and isinstance(data["value"], list):
        data = data["value"]  # Graph-style { "value": [ ... ] }
    if isinstance(data, dict):
        data = [data]
    for rec in data:
        if isinstance(rec, dict):
            yield rec


def read_rows(path: Path, sheet: str | None):
    ext = path.suffix.lower()
    if ext in (".xlsx", ".xlsm", ".xls"):
        yield from read_xlsx(path, sheet)
    elif ext == ".json":
        yield from read_json(path)
    else:
        yield from read_csv(path)


# --------------------------------------------------------------------------- #
# Field resolution helpers
# --------------------------------------------------------------------------- #
def build_column_map(sample_row: dict) -> dict:
    """Map each canonical concept to the actual header found in the input."""
    found = {"creation": None, "audit": None, "recordtype": None, "operation": None}
    for key in sample_row.keys():
        n = _norm(key)
        if found["creation"] is None and n in CREATION_KEYS:
            found["creation"] = key
        elif found["audit"] is None and n in AUDIT_KEYS:
            found["audit"] = key
        elif found["recordtype"] is None and n in RECORDTYPE_KEYS:
            found["recordtype"] = key
        elif found["operation"] is None and n in OPERATION_KEYS:
            found["operation"] = key
    return found


def get_cell(row: dict, col: str | None):
    if col is None:
        return None
    v = row.get(col)
    if v is None:
        return None
    s = str(v).strip()
    return s if s != "" else None


def parse_audit_json(raw):
    """Return (obj, was_json_object). Accepts a dict (from .json input) or a string."""
    if isinstance(raw, dict):
        return raw, True
    if raw is None:
        return None, False
    s = str(raw).strip()
    if len(s) < 10:
        return None, False
    try:
        obj = json.loads(s)
        return (obj, True) if isinstance(obj, dict) else (None, False)
    except (json.JSONDecodeError, ValueError):
        return None, False


def resolve_record_type(col_value, operation, json_value):
    """Prefer an existing *text* RecordType; else use the Operation enum string.

    The report expects friendly strings like 'CopilotInteraction' / 'AIAppInteraction'.
    The numeric RecordType inside AuditData is not what the report wants, so when the
    only available RecordType is numeric we fall back to the Operation string, which
    for these events IS the friendly enum name.
    """
    if col_value:
        s = str(col_value).strip()
        if s and not s.isdigit():
            return s
    if operation:
        return str(operation).strip()
    if json_value is not None and not str(json_value).strip().isdigit():
        return str(json_value).strip()
    return str(json_value).strip() if json_value is not None else ""


_DATE_FORMATS = (
    "%Y-%m-%dT%H:%M:%S.%fZ",
    "%Y-%m-%dT%H:%M:%SZ",
    "%Y-%m-%dT%H:%M:%S",
    "%m/%d/%Y %H:%M:%S",
    "%m/%d/%Y %I:%M:%S %p",
    "%Y-%m-%d %H:%M:%S",
    "%m/%d/%Y",
    "%Y-%m-%d",
)


def to_iso(v) -> str:
    """Normalize a date/datetime to ISO 8601 (round-trip safe for Power Query)."""
    if v is None or v == "":
        return ""
    if isinstance(v, datetime):
        return v.strftime("%Y-%m-%dT%H:%M:%SZ")
    s = str(v).strip()
    for fmt in _DATE_FORMATS:
        try:
            return datetime.strptime(s, fmt).strftime("%Y-%m-%dT%H:%M:%SZ")
        except ValueError:
            continue
    try:
        return datetime.fromisoformat(s.replace("Z", "+00:00")).strftime("%Y-%m-%dT%H:%M:%SZ")
    except ValueError:
        return s  # leave as-is; Power Query may still parse it


# --------------------------------------------------------------------------- #
# Main
# --------------------------------------------------------------------------- #
def main(argv=None):
    parser = argparse.ArgumentParser(
        description="Flatten a manual Purview audit export into the AI-in-One report CSV format."
    )
    parser.add_argument("input", help="Path to the manual export (.xlsx / .xls / .csv / .json)")
    parser.add_argument("-o", "--output", help="Output CSV path (default: <input>-clean.csv)")
    parser.add_argument("--sheet", help="Worksheet name to read (xlsx only; default: first sheet)")
    args = parser.parse_args(argv)

    in_path = Path(args.input)
    if not in_path.exists():
        sys.exit(f"ERROR: input file not found: {in_path}")

    out_path = Path(args.output) if args.output else in_path.with_name(in_path.stem + "-clean.csv")

    rows = read_rows(in_path, args.sheet)
    try:
        first = next(rows)
    except StopIteration:
        sys.exit("ERROR: the input file has no data rows.")

    colmap = build_column_map(first)
    if colmap["audit"] is None:
        sys.exit(
            "ERROR: could not find an 'AuditData' column in the input.\n"
            f"       Columns seen: {list(first.keys())}\n"
            "       The AuditData JSON blob is required to build the report dataset."
        )

    print("Detected source columns:")
    print(f"  CreationDate <- {colmap['creation'] or '(from AuditData JSON)'}")
    print(f"  AuditData    <- {colmap['audit']}")
    print(f"  RecordType   <- {colmap['recordtype'] or '(derived from Operation)'}")
    print(f"  Operation    <- {colmap['operation'] or '(from AuditData JSON)'}")
    print()

    total = written = skipped_empty = skipped_bad_json = truncated = 0

    # utf-8-sig + BOM matches the canonical PowerShell exporter output; the report
    # reads it with Encoding=65001 and ignores the BOM.
    with out_path.open("w", encoding="utf-8-sig", newline="") as f:
        writer = csv.writer(f, quoting=csv.QUOTE_MINIMAL)
        writer.writerow(CANONICAL_HEADERS)

        for row in _chain(first, rows):
            total += 1
            raw_audit = row.get(colmap["audit"])

            if raw_audit is not None and not isinstance(raw_audit, dict):
                if len(str(raw_audit)) >= EXCEL_CELL_LIMIT:
                    truncated += 1  # Excel chopped the cell -> JSON is corrupt

            obj, ok = parse_audit_json(raw_audit)
            if not ok:
                if raw_audit is None or str(raw_audit).strip() == "":
                    skipped_empty += 1
                else:
                    skipped_bad_json += 1
                continue

            creation = get_cell(row, colmap["creation"]) or obj.get("CreationTime") or obj.get("CreationDate")
            operation = get_cell(row, colmap["operation"]) or obj.get("Operation")
            recordtype = resolve_record_type(
                get_cell(row, colmap["recordtype"]), operation, obj.get("RecordType")
            )

            audit_clean = json.dumps(obj, separators=(",", ":"), ensure_ascii=False)

            writer.writerow([
                to_iso(creation),
                audit_clean,
                recordtype or "",
                operation or "",
            ])
            written += 1

    print("Done.")
    print(f"  Rows read        : {total}")
    print(f"  Rows written     : {written}")
    print(f"  Skipped (empty)  : {skipped_empty}")
    print(f"  Skipped (bad JSON): {skipped_bad_json}")
    if truncated:
        print()
        print(f"  WARNING: {truncated} AuditData cell(s) hit Excel's 32,767-char limit and were")
        print("           truncated BEFORE this script ran -- their JSON is corrupt and was dropped.")
        print("           Re-export from Purview directly to CSV (not Excel) to recover those rows.")
    print()
    print(f"Output written to: {out_path}")
    print('Point the report\'s "Copilot Interactions File" parameter at this file.')


def _chain(first, rest):
    yield first
    yield from rest


if __name__ == "__main__":
    main()
