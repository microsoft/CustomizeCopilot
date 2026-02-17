# CustomizeCopilot - Template Add-ons

<div align="center">

**Downloadable add-on pages and customizations for Microsoft's Copilot adoption and impact analytics templates**

**Plug these add-ons into the base templates below:**

[![Super User Adoption](https://img.shields.io/badge/Template-Super%20User%20Adoption-blue)](https://aka.ms/decodingsuperusage)
[![Super User Impact](https://img.shields.io/badge/Template-Super%20User%20Impact-green)](https://aka.ms/superuserimpact)

⭐ **Star this repository** to receive notifications about new template versions
👀 **Watch** for updates and announcements

</div>

---

<details open>
<summary><strong>📖 Introduction</strong></summary>

<br>

This repository provides ready-to-use add-on pages that extend Microsoft's official Copilot analytics templates. Each add-on is designed to be imported into your existing reports, providing additional insights and visualizations without requiring you to start from scratch.

### Main Templates

These add-ons are compatible with:

- **[Super User Adoption Report](https://aka.ms/decodingsuperusage)** - Track Copilot adoption patterns, identify super users, and measure engagement across Microsoft 365 applications
- **[Super User Impact Report](https://aka.ms/superuserimpact)** - Measure the impact of Copilot on work behaviors, productivity, and collaboration patterns using Viva Insights data

### Why Use Add-ons?

- ✅ **Plug-and-play** - Import pages directly into your existing reports
- ✅ **Pre-configured** - All measures and visuals are ready to use
- ✅ **Maintained** - Regular updates aligned with the main templates
- ✅ **Modular** - Only add what you need
- ✅ **Compatible** - Works with both Adoption and Impact templates

</details>

---

<details>
<summary><strong>🔄 How to Merge Templates</strong></summary>

<br>

### Prerequisites

- **Power BI Desktop** (latest version recommended)
- **Source template** (.pbit file) - The add-on you want to import FROM
- **Target template** (.pbit or .pbix file) - Your main report you want to import TO
- **Compatible data source** - Both templates must use the same or compatible data schemas

---

### Method 1: Using Power BI Desktop (Recommended)

#### Step 1: Open Your Target Report

Open your main report (Super User Adoption or Super User Impact) in Power BI Desktop. Ensure the report is connected to your data and loading correctly.

#### Step 2: Open the Add-on in a New Window

1. Launch a **second instance** of Power BI Desktop
2. Open the add-on template file (e.g., `Champion ID Pages.pbit`)
3. Connect it to your data source when prompted

#### Step 3: Copy Pages

1. In the **add-on window**, find the pages you want to copy in the **Pages pane** (left sidebar)
2. **Right-click** on a page → Select **"Copy"** or **"Duplicate Page"**
3. Switch to your **target report window**
4. **Right-click** in the Pages pane → Select **"Paste"**
5. Repeat for each page you want to import

#### Step 4: Verify

1. Check that all visuals on imported pages display correctly
2. Verify that measures and fields resolve properly
3. Test interactions and filters
4. If you see errors, consult the add-on's specific README for dependencies

#### Step 5: Save

- **File** → **Save As**
- Save as `.pbix` (working file) or `.pbit` (template)

---

### Method 2: Using Power BI Project Files (.pbip)

For users working with the Power BI Project format:

#### Step 1: Locate Page Folders

1. Navigate to the source `.pbip` folder: `[AddOnReport].Report\definition\pages\`
2. Each page has its own folder with a unique GUID name (e.g., `883530f35a08e1eb760e`)
3. Identify which page folders you want to copy

#### Step 2: Copy Page Folders

1. Copy the entire page folder(s) from the add-on
2. Paste into your target report's pages directory: `[YourReport].Report\definition\pages\`

#### Step 3: Update pages.json

1. Open `[YourReport].Report\definition\pages\pages.json` in a text editor
2. Add the page GUID(s) to the `pageOrder` array:

```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/fabric/item/report/definition/pagesMetadata/1.0.0/schema.json",
  "pageOrder": [
    "your-existing-page-id-1",
    "your-existing-page-id-2",
    "NEW-COPIED-PAGE-ID-HERE"
  ],
  "activePageName": "your-existing-page-id-1"
}
```

#### Step 4: Verify Dependencies

Check the add-on's README for required measures. If any are missing, add them to:
`[YourReport].SemanticModel\definition\tables\Table.tmdl`

#### Step 5: Test

Open the modified `.pbip` in Power BI Desktop and verify everything loads correctly.

---

### Troubleshooting

| Problem | Solution |
|---------|----------|
| **Visuals show errors** | Check that all required measures exist in your target report (see add-on README for dependencies) |
| **"Cannot find field" errors** | The add-on references measures/columns not in your target report. Add missing measures or modify visuals to use existing fields |
| **Pages are blank** | Ensure target report is connected to data with the same schema. Refresh your data model |
| **Duplicate page names** | Rename imported pages: Right-click page tab → Rename |

---

### Best Practices

- ✅ **Always backup** your target report before importing pages
- ✅ **Test with data** before sharing the modified report
- ✅ **Review dependencies** listed in the add-on README
- ✅ **Keep Power BI Desktop updated** to the latest version
- ✅ **Document your customizations** for future reference

</details>

---

<details>
<summary><strong>🏆 Champion ID Pages</strong></summary>

<br>

### Overview

The **Champion ID Pages** add-on provides detailed person-level analytics for identifying and analyzing your top Copilot champions. These pages show individual usage patterns, application-specific action breakdowns, and network influence metrics.

**📦 Download:** [`Champion-ID/Champion ID Pages.pbit`](./Champion-ID/Champion%20ID%20Pages.pbit)

---

### What's Included

This add-on contains **2 report pages**:

#### Champion ID Page 1: Individual Rankings & Actions
- **Usage Rankings** - See how each person ranks across total Copilot usage and by application (Word, Excel, Outlook, PowerPoint, Teams)
- **Action Totals** - Detailed breakdown of Copilot actions by person across all M365 apps
- **Network Metrics** - Strong ties, diverse ties, and internal network size per person
- **Activity Summary** - Average apps used and Copilot active days per person

#### Champion ID Page 2: Extended Analytics
- Additional person-level insights and trends
- Comparative views across team members
- Temporal usage patterns

---

### Use Cases

1. **Champion Identification** - Pinpoint your most active Copilot users for peer training programs
2. **Personalized Coaching** - Understand individual usage patterns to provide targeted support
3. **Recognition Programs** - Identify top performers for awards or incentives
4. **Adoption Analysis** - Track which applications specific users are engaging with
5. **Network Analysis** - Identify influential employees who can drive adoption through their networks

---

### Prerequisites

These pages require the following data to function properly:

#### Required Columns
- `PersonId`, `DisplayName`, `TimeZone`, `Date`
- `Organization` (or aggregated organization field)
- Copilot action columns (e.g., `Copilot actions taken in Word`, `Copilot actions taken in Excel`, etc.)
- Network metrics (if available): `Strong ties`, `Diverse ties`, `Internal network size`

#### Required Measures

The Champion ID pages depend on **25 measures** that must exist in your target report. **Good news:** If you're using the latest version (v5+) of the Super User Adoption or Super User Impact templates, all required measures are already included!

<details>
<summary>View complete list of required measures (click to expand)</summary>

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

</details>

---

### Compatibility

| Template | Version | Status |
|----------|---------|--------|
| Super User Adoption | v5+ | ✅ Fully Compatible |
| Super User Adoption | v4 and below | ⚠️ Requires measure updates |
| Super User Impact | v5+ | ✅ Fully Compatible |
| Super User Impact | v4 and below | ⚠️ Requires measure updates |

**Note:** If you're using an older template version, the required measures need to be added manually. Contact support or update to the latest template version.

---

### Data Requirements

- **Minimum data:** 1 week of Copilot usage data from Viva Insights
- **Recommended data:** 4+ weeks for meaningful trend analysis
- **Network metrics:** Optional but recommended for full functionality (requires Viva Insights with network analysis enabled)

---

### Known Limitations

- Network metrics (Strong Ties, Diverse Ties, Internal Network Size) require Viva Insights data with network analysis enabled
- Person-level data may be subject to organizational privacy policies
- Rankings are relative to your data set; ensure consistent data timeframes for accurate comparisons

---

### Support

For questions or issues specific to the Champion ID Pages:
- 📄 [View detailed documentation](./Champion-ID/README.md)
- 🐛 [Report an issue](https://github.com/microsoft/CustomizeCopilot/issues)
- 💬 [Join discussions](https://github.com/microsoft/CustomizeCopilot/discussions)

</details>

---

<details open>
<summary><strong>🚀 Getting Started</strong></summary>

<br>

1. **Download the latest version** of your base template ([Adoption](https://aka.ms/decodingsuperusage) or [Impact](https://aka.ms/superuserimpact))
2. **Browse add-ons** in this repository and download the ones you need
3. **Follow the merge instructions** above to import pages into your report
4. **Verify dependencies** listed in each add-on's README
5. **Test with your data** before deploying to production

</details>

---

## 📄 License

These add-ons are provided as-is and require the base templates (Super User Adoption or Super User Impact) to function properly.

---

## 🔔 Stay Updated

- ⭐ **Star this repository** to receive notifications about new add-ons
- 👀 **Watch** for updates and announcements
- 🔄 Check back regularly for new add-ons and template updates

---

<div align="center">

**Made with ❤️ for the Copilot adoption community**

Questions? Visit our [Discussions](https://github.com/microsoft/CustomizeCopilot/discussions) page

</div>
