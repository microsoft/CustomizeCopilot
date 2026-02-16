# Champion ID Pages

**Person-level analytics for identifying and analyzing your top Copilot champions**

---

## Overview

The **Champion ID Pages** add-on provides detailed individual-level analytics for the Super User Adoption and Impact templates. These pages enable you to identify your organization's top Copilot users, understand their usage patterns across applications, and analyze their network influence.

**Perfect for:**
- Champion identification programs
- Personalized coaching and training
- Recognition and incentive programs
- Network-based adoption strategies

---

## What's Included

This add-on contains **2 report pages**:

### Champion ID Page 1: Individual Rankings & Actions
- **Usage Rankings** - Rankings by total Copilot usage and by individual application (Word, Excel, Outlook, PowerPoint, Teams, Copilot Chat)
- **Action Totals** - Complete breakdown of Copilot actions per person:
  - Chat (web) and Chat (work) prompts
  - Teams: Intelligent Recap, Summarize Chat, Summarize Meeting
  - Outlook: Generate Email, Email Coaching, Summarize Email
  - Word: Draft Doc, Rewrite Text, Summarize Doc, Visualize Table
  - Excel: Create Formula, Analysis, Formatting
  - PowerPoint: Create Presentation, Organize, Summarize, Add Content
- **Network Metrics** - Strong ties, diverse ties, and internal network size per person (requires Viva Insights data)
- **Activity Summary** - Average apps used and Copilot active days per person

### Champion ID Page 2: Extended Analytics
- Additional person-level insights and comparative views
- Temporal usage patterns across champions
- Team-level comparisons

---

## Use Cases

1. **Champion Identification**
   - Quickly identify your most active Copilot users across all M365 apps
   - Spot application-specific experts (e.g., "Excel Copilot Champions")
   - Build targeted champion programs based on usage data

2. **Personalized Coaching**
   - Understand each person's usage patterns to provide targeted support
   - Identify skill gaps (e.g., users strong in Word but not using Copilot in Excel)
   - Track individual progress over time

3. **Recognition Programs**
   - Identify top performers for awards, incentives, or public recognition
   - Create leaderboards and gamification programs
   - Highlight diverse usage patterns (not just volume)

4. **Network-Based Adoption**
   - Identify influential employees with large, diverse networks
   - Select champions who can drive adoption through their connections
   - Target users with high strong ties for peer-to-peer training

5. **Adoption Analysis**
   - Track which applications specific users are engaging with
   - Identify early adopters vs. laggards
   - Monitor adoption velocity across your organization

---

## Prerequisites

### Required Data Columns

These columns must exist in your data source:

**Core Columns:**
- `PersonId` (unique identifier for each person)
- `DisplayName` (person's name for display)
- `Date` or `MetricDate` (date of the metric)
- `TimeZone` (person's timezone)
- `Organization` (organizational unit or department)

**Copilot Action Columns:**
- Individual action columns for each Copilot feature (e.g., `Copilot actions taken in Word`, `Summarize email thread actions taken using Copilot in Outlook`, etc.)
- Total action columns (e.g., `Total Copilot actions taken`)
- Active days columns (e.g., `Total Copilot active days`)

**Network Columns (optional but recommended):**
- `Strong ties` - Number of strong network connections
- `Diverse ties` - Number of diverse network connections
- `Internal network size` - Total internal network connections

### Required Measures

The Champion ID pages depend on **25 DAX measures** that must exist in your target report:

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

**Good news:** If you're using **v4 or later** of the Super User Adoption or Impact templates, all required measures are already included!

---

## Compatibility

| Template | Version | Status |
|----------|---------|--------|
| **Super User Adoption** | v4+ | ✅ Fully Compatible |
| Super User Adoption | v3 and below | ⚠️ Requires measure updates |
| **Super User Impact** | v4+ | ✅ Fully Compatible |
| Super User Impact | v3 and below | ⚠️ Requires measure updates |

**Note:** If using an older template version:
1. Update to the latest template version (recommended), OR
2. Manually add the required measures listed above

---

## Installation

### Method 1: Import Pages Directly (Recommended)

1. **Open your main report** (Super User Adoption or Impact) in Power BI Desktop
2. **Open the add-on template**:
   - Launch a second instance of Power BI Desktop
   - Open `Champion ID Pages.pbit`
   - Connect to your data source when prompted
3. **Copy pages**:
   - In the add-on window, right-click on "Champion ID Page 1" → Select "Copy"
   - Switch to your main report window
   - Right-click in the Pages pane → Select "Paste"
   - Repeat for "Champion ID Page 2"
4. **Verify**:
   - Check that all visuals display correctly
   - Test filters and interactions
   - Verify network metrics (if using Viva Insights data)
5. **Save** your updated report

### Method 2: Using Power BI Project Files (.pbip)

If working with Power BI Project format:

1. Locate page folders in the add-on's `.Report\definition\pages\` directory
2. Copy the page folders to your main report's pages directory
3. Update your report's `pages.json` file to include the new page GUIDs
4. Open the modified report in Power BI Desktop

**Detailed instructions:** See the main [CustomizeCopilot README](../README.md#how-to-merge-templates)

---

## Data Requirements

**Minimum data:**
- 1 week of Copilot usage data from Microsoft Viva Insights or equivalent

**Recommended data:**
- 4+ weeks for meaningful trend analysis
- Person-level Copilot usage metrics from Viva Insights
- Network metrics (requires Viva Insights with network analysis enabled)

**Data Privacy:**
- Person-level data may be subject to organizational privacy policies
- Ensure you have appropriate permissions to view individual-level analytics
- Consider using aggregated views for broader audiences

---

## Configuration

### Parameters

The Champion ID Pages use the same parameters as the main templates:

- **SourceType**: Choose between `CSV` or `DirectQuery` (Viva Insights)
- **CSV**: File path to your CSV data (if using CSV mode)
- **PartitionID**: Viva Insights partition ID (if using DirectQuery mode)
- **QueryID**: Viva Insights query ID (if using DirectQuery mode)

### Filters

The pages include pre-configured filters:
- **Date Slider**: Control the time period for analysis
- **Organization**: Filter by organizational unit
- **Person Filters**: Filter to specific individuals

### Customization

**To customize rankings:**
- Edit the visual filters to change ranking criteria
- Modify the DAX measures to adjust scoring logic

**To add custom metrics:**
- Add new measures to your data model
- Create new visuals referencing those measures

---

## Known Limitations

- **Network metrics** (Strong Ties, Diverse Ties, Internal Network Size) require Viva Insights data with network analysis enabled
  - If network data is unavailable, those visuals will show blank/null
- **Person-level data** may be subject to organizational privacy policies and minimum aggregation thresholds
- **Rankings** are relative to your dataset; ensure consistent date ranges for accurate comparisons
- **DisplayName** column is required; if your data doesn't include names, you can add a calculated column or use PersonId
- **Performance**: Large datasets (50k+ rows) may require performance optimization
  - Consider using aggregations or Import mode instead of DirectQuery

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| **Visuals show errors** | Verify all required measures exist (see Prerequisites section) |
| **"Cannot find field" errors** | Missing measures in your target report - add them or update to template v4+ |
| **Network metrics show blank** | Network data not available - either data source doesn't include it, or below privacy threshold |
| **Rankings look incorrect** | Check date filter - rankings are calculated over the selected date range |
| **Performance is slow** | Large dataset - consider using Import mode or adding aggregations |
| **DisplayName shows IDs instead of names** | DisplayName column missing - add names to your data or create a mapping table |

---

## Feedback & Support

Have questions or issues with the Champion ID Pages?

- 📄 [View main documentation](../README.md)
- 🐛 [Report an issue](https://github.com/microsoft/CustomizeCopilot/issues)
- 💬 [Join discussions](https://github.com/microsoft/CustomizeCopilot/discussions)
- 📧 Contact your Copilot adoption team

---

## Version History

### v1.0 (February 2026)
- Initial release
- 2 pages with person-level analytics
- 25 measures for action totals and rankings
- Network metrics support (Viva Insights)
- Compatible with Super User Adoption v4+ and Impact v4+

---

<div align="center">

**Made with ❤️ for the Copilot adoption community**

[⬅️ Back to CustomizeCopilot](../README.md)

</div>
