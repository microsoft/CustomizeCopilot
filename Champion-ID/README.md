<div align="center">

<br>

# 🏆 Champion ID Pages

### Person-level analytics for identifying and analyzing your top Copilot champions.

<br>

[![Built by Microsoft](https://img.shields.io/badge/Built%20by-Microsoft-0078d4?style=for-the-badge&logo=microsoft&logoColor=white)](https://microsoft.github.io/Analytics-Hub/team/)
[![Analytics Hub](https://img.shields.io/badge/Analytics%20Hub-Origin%20Site-8661c5?style=for-the-badge&logo=github&logoColor=white)](https://microsoft.github.io/Analytics-Hub/)

<br>

**🌐 Origin site:** [microsoft.github.io/Analytics-Hub](https://microsoft.github.io/Analytics-Hub/)

<br>

**[Overview ↓](#overview)** &nbsp;·&nbsp; **[Installation ↓](#installation)** &nbsp;·&nbsp; **[Prerequisites ↓](#prerequisites)** &nbsp;·&nbsp; **[Troubleshooting ↓](#troubleshooting)**

<br>

</div>

---

<a id="overview"></a>

<details open>
<summary><strong>📖 Overview</strong></summary>

<br>

The **Champion ID Pages** add-on provides detailed individual-level analytics for the Super User Adoption and Impact templates. These pages help you identify your organization's top Copilot users, understand their usage patterns across applications, and analyze their network influence.

**Perfect for:**
- Champion identification programs
- Personalized coaching and training
- Recognition and incentive programs
- Network-based adoption strategies

### What's Included

This add-on contains **2 report pages**:

#### Champion ID Page 1: Individual Rankings & Actions
- **Usage Rankings** — Rankings by total Copilot usage and by individual application (Word, Excel, Outlook, PowerPoint, Teams, Copilot Chat)
- **Action Totals** — Complete breakdown of Copilot actions per person across every M365 app
- **Network Metrics** — Strong ties, diverse ties, and internal network size per person (requires Viva Insights data)
- **Activity Summary** — Average apps used and Copilot active days per person

#### Champion ID Page 2: Extended Analytics
- Additional person-level insights and comparative views
- Temporal usage patterns across champions
- Team-level comparisons

### Use Cases

1. **Champion Identification** — Pinpoint your most active Copilot users for peer training programs
2. **Personalized Coaching** — Understand individual usage patterns to provide targeted support
3. **Recognition Programs** — Identify top performers for awards or incentives
4. **Network-Based Adoption** — Surface influential employees who can drive adoption through their networks
5. **Adoption Analysis** — Track which applications specific users are engaging with

</details>

---

<a id="installation"></a>

<details open>
<summary><strong>🚀 Installation — Open, Select All, Paste</strong></summary>

<br>

### Download

Grab **[`Merge Champion ID.pbix`](./Merge%20Champion%20ID.pbix)** from this folder. It's a ready-to-open Power BI report containing both Champion ID pages and sample data — no parameters or data connection required to preview.

> The earlier template format (`Champion ID Pages.pbit`) has been moved to [`archive/`](./archive/) for reference. The `.pbix` is the recommended download going forward because it opens instantly and the copy/paste flow below works without any prompts.

### Steps

#### 1. Open both reports side by side
- Open your main report (Super User **Adoption** or **Impact**) in Power BI Desktop
- Launch a **second** Power BI Desktop window and open `Merge Champion ID.pbix`

#### 2. Add a blank page in your target report
- In your Adoption/Impact report, click the **+** at the bottom of the page tabs to create a new blank page
- Name it `Champion ID` (or whatever you prefer)

#### 3. Select all visuals in `Merge Champion ID.pbix`
- Switch to the `Merge Champion ID.pbix` window and open a Champion ID page (start with Page 1)
- Click anywhere on the report canvas (not on a visual), then press **`Ctrl + A`** to select every visual on the page

#### 4. Copy and paste into the new page
- Press **`Ctrl + C`**
- Switch to your Adoption/Impact report → click into the blank `Champion ID` page
- Press **`Ctrl + V`** — every visual lands in its original position with all formatting intact

#### 5. Repeat for Page 2
- Add a second blank page (`Champion ID — Extended`)
- Back in `Merge Champion ID.pbix`, switch to Page 2 → `Ctrl + A` → `Ctrl + C`
- In your target report → `Ctrl + V` into the new blank page

#### 6. Verify and save
- Check that visuals resolve against your data (measures should bind automatically on Adoption/Impact v4+)
- Test filters and slicers
- **File → Save**

> **Why `Ctrl + A` + paste instead of "Copy page"?** Pasting visuals into a *blank* page in your existing report keeps your report's theme, page size, and global filters in control. This is the pattern we're standardizing on across all CustomizeCopilot add-ons going forward.

</details>

---

<a id="prerequisites"></a>

<details>
<summary><strong>🧩 Prerequisites & Compatibility</strong></summary>

<br>

### Required Data Columns

**Core columns:**
- `PersonId` (unique identifier for each person)
- `DisplayName` (person's name for display)
- `Date` or `MetricDate` (date of the metric)
- `TimeZone`
- `Organization` (organizational unit or department)

**Copilot action columns:**
- Individual action columns for each Copilot feature (e.g., `Copilot actions taken in Word`, `Summarize email thread actions taken using Copilot in Outlook`, etc.)
- Total action columns (e.g., `Total Copilot actions taken`)
- Active days columns (e.g., `Total Copilot active days`)

**Network columns (optional but recommended):**
- `Strong ties` — Number of strong network connections
- `Diverse ties` — Number of diverse network connections
- `Internal network size` — Total internal network connections

### Required Measures

The Champion ID pages depend on **25 DAX measures** that must exist in your target report.

<details>
<summary><strong>View complete list of required measures (click to expand)</strong></summary>

**Network Measures (3):**
- `Total Strong Ties`
- `Total Diverse Ties`
- `Total Internal Network Size`

**Copilot Action Total Measures (19):**
- `(M) Chat Web Total`
- `(M) Chat Work Total`
- `(M) Teams Intelligent Recap Total`
- `(M) Summarize Teams Chat Total`
- `(M) Summarize Teams Meeting Total`
- `(M) Generate Outlook Email Total`
- `(M) Outlook Email Coaching Total`
- `(M) Summarize Outlook Email Total`
- `(M) Draft Word Doc Total`
- `(M) Rewrite Word Text Total`
- `(M) Summarize Word Doc Total`
- `(M) Visualize Word Table Total`
- `(M) Create Excel formula Total`
- `(M) Excel analysis Total`
- `(M) Excel formatting Total`
- `(M) Create PPT Presentation Total`
- `(M) Organize PPT Presentation Total`
- `(M) Summarize PPT Presentation Total`
- `(M) Add PPT Presentation Content Total`

**General Measures (3):**
- `Avg Apps Used`
- `Average Copilot Active Days`
- `Total Copilot Actions (30 Days)` or equivalent

</details>

> **Good news:** If you're using **v4 or later** of the Super User Adoption or Impact templates, all required measures are already included.

### Compatibility

| Template | Version | Status |
|----------|---------|--------|
| **Super User Adoption** | v4+ | ✅ Fully Compatible |
| Super User Adoption | v3 and below | ⚠️ Requires measure updates |
| **Super User Impact** | v4+ | ✅ Fully Compatible |
| Super User Impact | v3 and below | ⚠️ Requires measure updates |

### Data Requirements

- **Minimum:** 1 week of Copilot usage data from Microsoft Viva Insights or equivalent
- **Recommended:** 4+ weeks for meaningful trend analysis
- **Network metrics:** Require Viva Insights with network analysis enabled

### Data Privacy

- Person-level data may be subject to organizational privacy policies
- Ensure you have appropriate permissions to view individual-level analytics
- Consider using aggregated views for broader audiences

</details>

---

<a id="troubleshooting"></a>

<details>
<summary><strong>🛠️ Troubleshooting</strong></summary>

<br>

| Problem | Solution |
|---------|----------|
| **Visuals show errors after paste** | Verify all required measures exist in your target report (see Prerequisites). If you're on Adoption/Impact v4+, they should already be there. |
| **"Cannot find field" errors** | A measure or column referenced by the visual doesn't exist in your target model. Add the missing measure or update your template to v4+. |
| **Network metrics show blank** | Network data isn't available — either the data source doesn't include it, or values are below the privacy aggregation threshold. |
| **Rankings look incorrect** | Check your date filter — rankings are calculated over the selected date range. |
| **`Ctrl + A` selects nothing** | Click once on the report canvas (not on a visual or pane) first, then press `Ctrl + A`. |
| **Pasted visuals overlap existing visuals** | Always paste into a **new blank page** rather than an existing one. |
| **DisplayName shows IDs instead of names** | `DisplayName` column is missing — add names to your data source or create a mapping table. |
| **Performance is slow** | Large dataset (50k+ rows) — consider using Import mode or adding aggregations. |

</details>

---

<details>
<summary><strong>🗓️ Version History</strong></summary>

<br>

### v1.1 (May 2026)
- Switched primary distribution to `Merge Champion ID.pbix` for one-click open and copy/paste
- Standardized on the **`Ctrl + A` → new blank page → paste** workflow
- Archived the original `Champion ID Pages.pbit` under `archive/` for reference
- Refreshed sample data
- Restyled README to match the CustomizeCopilot hub layout

### v1.0 (February 2026)
- Initial release
- 2 pages with person-level analytics
- 25 measures for action totals and rankings
- Network metrics support (Viva Insights)
- Compatible with Super User Adoption v4+ and Impact v4+

</details>

---

<div align="center">

**Made with ❤️ for the Copilot adoption community**

[⬅ Back to CustomizeCopilot](../README.md) &nbsp;·&nbsp; [🌐 Analytics Hub](https://microsoft.github.io/Analytics-Hub/) &nbsp;·&nbsp; [🐛 Report an issue](https://github.com/microsoft/CustomizeCopilot/issues) &nbsp;·&nbsp; [💬 Discussions](https://github.com/microsoft/CustomizeCopilot/discussions)

</div>
