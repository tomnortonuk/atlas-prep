# ATLAS Platform Functionality

Complete feature specifications and user requirements for ATLAS.

---

## User Personas

### 1. Trust Executive / Board Member
**Needs:**
- High-level performance dashboards
- Benchmarking against peers
- Board-ready PDF reports
- National context for local performance

**Use Cases:**
- Monthly board pack generation
- Trust-wide performance overview
- Peer comparison across ICB/region
- Long-term trend analysis

### 2. Operational Manager / Service Lead
**Needs:**
- Specialty/service-specific data
- Drill-down capability
- Real-time operational metrics
- Custom alert thresholds

**Use Cases:**
- Daily operational briefing
- Waiting list management
- Workforce planning
- Capacity monitoring

### 3. Business Intelligence / Analytics Team
**Needs:**
- Flexible query building
- Multiple data source integration
- Export capabilities (CSV, Excel)
- API access for tool integration

**Use Cases:**
- Ad-hoc analysis
- Multi-source data integration
- Custom reporting for stakeholders
- Data validation and quality checks

### 4. ICB Commissioner / System Leader
**Needs:**
- System-wide view across providers
- Population health insights
- Integration between health and care
- Resource allocation planning

**Use Cases:**
- ICB system reporting
- Provider comparison
- Integration dashboard
- Inequalities analysis

### 5. National Policy Maker / Researcher
**Needs:**
- National aggregates
- Trend analysis over time
- Geographic variation
- Cross-sector integration

**Use Cases:**
- Policy development
- National performance monitoring
- Research and evaluation
- Regional variation analysis

---

## Core Features

### 1. Data Explorer

**Dashboard Home**
- Welcome screen with quick stats
- Recently viewed datasets
- Saved views shortcuts
- Data freshness indicators

**Multi-Source Selection**
```
┌─────────────────────────────────────┐
│ Select Data Sources                 │
├─────────────────────────────────────┤
│ ☑ RTT Waiting Lists                 │
│ ☑ A&E Performance                   │
│ ☐ Workforce (ESR)                   │
│ ☐ Mental Health (MHSDS)             │
│ ☐ Mortality (SHMI)                  │
│ + Add More Sources                  │
└─────────────────────────────────────┘
```

**Dynamic Filtering**
- Organization selector (autocomplete)
  - By trust name
  - By ODS code
  - By postcode proximity
- Time period selector
  - Single month/quarter/year
  - Date range
  - Comparison periods (vs. last year)
- Geographic filters
  - Region
  - ICB
  - Sub-ICB
  - Local Authority
- Specialty/service filters (context-dependent)
- Custom metric thresholds

**Filter Persistence**
- Save filter combinations
- Quick apply saved filters
- Share filters with colleagues
- Set default filters per user

### 2. Organizational Hierarchy Navigator

**Multi-Level View**
```
National
  └─ Region: South West
       └─ ICB: Somerset ICB
            └─ Sub-ICB: Taunton & Bridgwater
                 └─ Trust: Somerset NHS FT
                      └─ Site: Musgrove Park Hospital
```

**Features:**
- Expand/collapse hierarchy
- Click any level to filter data
- Aggregate up to any level
- Historical view (track reorganizations)
- Provider systems view (linked trusts)

**Succession Tracking**
- View historical organizations
- Trace merger/acquisition chains
- Compare pre/post-merger performance
- Link historical data to current structures

### 3. Insight Categories (Novel Categorization)

**Multi-Dimensional Navigation**

Instead of traditional "by service type," offer categorization by:

**A. Patient Journey Stages**
1. Prevention & Early Intervention
2. First Contact & Access
3. Diagnosis & Assessment
4. Treatment & Care
5. Recovery & Rehabilitation
6. Ongoing Support
7. End of Life Care

**B. System Pressure Points**
1. A&E Crowding
2. Elective Backlogs
3. Bed Capacity
4. Workforce Shortages
5. Social Care Delays
6. Diagnostic Capacity

**C. Population Health Determinants**
1. Deprivation & Inequalities
2. Healthy Life Expectancy
3. Risk Factors (smoking, obesity, alcohol)
4. Mental Wellbeing
5. Social Determinants

**D. Cross-Agency Integration**
1. Health & Social Care Interface
2. Police & Health Partnerships
3. Fire & Health Prevention
4. Housing & Health
5. Justice & Health

**E. Quality & Safety Themes**
1. Patient Safety Incidents
2. Infection Prevention
3. Clinical Effectiveness
4. Mortality & Harm

**F. Productivity & Efficiency**
1. Length of Stay
2. Did Not Attend (DNA)
3. Theatre Utilization
4. Outpatient Optimization
5. Workforce Productivity

**Implementation:**
```
┌─────────────────────────────────────┐
│ Explore By:                         │
├─────────────────────────────────────┤
│ ○ Service Type (Traditional)        │
│ ● Patient Journey                   │
│ ○ System Pressure                   │
│ ○ Cross-Agency Working              │
└─────────────────────────────────────┘

Selected: Patient Journey > Diagnosis & Assessment

Related Data Sources:
• ECDS (A&E diagnostics)
• Diagnostic Waiting Times (DM01)
• Cancer 28-day Faster Diagnosis
• Mental Health Assessments (MHSDS)
• Community Diagnostics
```

---

## 4. Visualization Suite

### Chart Types

**Line Charts** (Time Trends)
- Single or multi-series
- Confidence intervals
- Annotations for policy changes
- Seasonal adjustment option

**Bar Charts** (Comparisons)
- Horizontal for long org names
- Grouped or stacked
- Threshold reference lines
- Peer benchmarking bands

**Statistical Process Control (SPC)**
- XmR charts
- Control limits (±3 sigma)
- Special cause rules (NHS-R Making Data Count)
- Process change annotations
- Trends, shifts, runs detection

**Heatmaps**
- Geographic (by ICB/LA/trust)
- Time-series heatmap (rows=orgs, cols=months)
- Correlation matrices

**Tables**
- Sortable columns
- Sparklines in cells
- Conditional formatting
- Totals/subtotals
- Export to Excel

**Maps**
- Choropleth by ICB/LA/region
- Point maps for provider locations
- Interactive tooltips
- Zoom/pan

**Sankey Diagrams**
- Patient flows (e.g., admission to discharge)
- Funding flows (health to social care)
- Pathway visualization

### Interactivity

- **Hover:** Tooltips with detail
- **Click:** Drill-down to detail
- **Brush:** Zoom to time range
- **Legend toggle:** Show/hide series
- **Cross-filtering:** Chart interaction updates others

### Customization

- Color schemes (ATLAS brand, colorblind-safe, high-contrast)
- Chart title and labels
- Axis scaling (auto, manual, log)
- Reference lines and annotations
- Font sizes (presentation mode)

---

## 5. Custom View Builder

**Step 1: Select Data Sources**
```
[RTT Waiting Lists] [A&E Performance] [Workforce]
```

**Step 2: Choose Aggregation Level**
```
○ National  ○ Regional  ○ ICB  ● Trust  ○ Site
```

**Step 3: Apply Filters**
```
Time Period: [Jan 2024] to [Sep 2024]
Organization: [Somerset NHS FT]
Specialty: [All]
```

**Step 4: Select Metrics**
```
☑ Total waiting list size
☑ Patients waiting 18+ weeks
☑ Patients waiting 52+ weeks
☐ Median wait time
```

**Step 5: Choose Visualization**
```
● Line chart  ○ Bar chart  ○ Table  ○ SPC chart
```

**Step 6: Save View**
```
View Name: [Somerset RTT Monthly Trend]
Folder: [Elective Recovery]
☑ Add to Dashboard
[Save] [Save & Publish]
```

---

## 6. Report Generation

### Quick Reports (Pre-built Templates)

**Trust Performance Pack**
- Trust summary page
- Waiting lists (RTT)
- A&E performance
- Workforce snapshot
- Benchmarking vs. peers

**ICB System Report**
- System-wide metrics
- Provider comparison table
- Geographic variation map
- Integration dashboard

**National Overview**
- National trends
- Regional comparison
- Top/bottom performers
- Long-term trends

### Custom Report Builder

**Select Views to Include**
```
☑ Trust Summary Dashboard
☑ RTT Waiting List Trend (last 12 months)
☑ A&E Performance vs. ICB Average
☑ Workforce FTE by Staff Group
☐ Add More...
```

**Report Settings**
```
Title: [Q2 2024 Trust Board Report]
Cover Page: ☑ Include ATLAS branding
Page Numbers: ☑ Include
Table of Contents: ☑ Auto-generate
Header/Footer: [Somerset NHS FT | September 2024]
```

**Output Options**
```
Format: ● PDF  ○ PowerPoint  ○ Word
Page Size: [A4]
Orientation: ● Portrait  ○ Landscape
[Generate Report]
```

**Generated Report Features**
- Clickable table of contents
- Page numbers
- Chart legends
- Data source references
- Publication date/time
- Static snapshot (data as of date X)

### Report Publishing & Sharing

**Publish Report**
```
Report Name: [Q2 2024 Board Report]
Description: [Quarterly performance review]
Access: ○ Private  ● Organization  ○ Public
Notify: [board-members@trust.nhs.uk]
[Publish]
```

**Sharing Options**
- Direct link (time-limited or permanent)
- Email distribution list
- Organization-wide (all trust users)
- Public (external stakeholders)

---

## 7. User Workspace Management

### Saved Views

**My Views**
```
┌─ Elective Recovery (Folder)
│  ├─ Somerset RTT Trend
│  ├─ Longest Waiters by Specialty
│  └─ RTT vs. Regional Average
├─ Workforce Planning (Folder)
│  ├─ Nursing Vacancies
│  └─ Bank & Agency Spend
└─ A&E Dashboard
```

**View Metadata**
- Last modified date
- Number of times viewed
- Data sources used
- Share status

**View Actions**
- Rename/duplicate/delete
- Move to folder
- Share with colleagues
- Export configuration (JSON)
- Schedule email updates

### Folder Organization

**Create Folder Structure**
```
Root
├─ Executive Dashboards
├─ Operational Monitoring
├─ Service Line Reports
│  ├─ Surgery
│  ├─ Medicine
│  └─ Urgent Care
└─ Board Papers
```

**Folder Permissions**
- Private (only me)
- Shared with team
- Organization-wide

### Dashboard Builder

**Create Personal Dashboard**
```
┌────────────────────┬────────────────────┐
│                    │                    │
│  [RTT Summary]     │  [A&E Performance] │
│                    │                    │
├────────────────────┴────────────────────┤
│                                         │
│  [Workforce Snapshot]                   │
│                                         │
├──────────────────────────────┬──────────┤
│                              │          │
│  [Bed Occupancy]             │ [Alerts] │
│                              │          │
└──────────────────────────────┴──────────┘
```

**Features:**
- Drag-and-drop layout
- Resize panels
- Add saved views to dashboard
- Auto-refresh (live mode)
- Export dashboard as image

---

## 8. Data Export & Integration

### Export Formats

**CSV Export**
- Current filtered view
- Raw data (flat table)
- UTF-8 encoding
- Configurable delimiters

**Excel Export**
- Multiple sheets (one per data source)
- Formatted headers
- Conditional formatting
- Charts embedded
- Data validation

**JSON Export**
- Structured data
- Includes metadata
- API-compatible format

**PDF Export**
- Charts as images
- Tables formatted
- Page breaks optimized
- Branding applied

### API Access (Future)

**RESTful API**
```bash
GET /api/v1/data/rtt?period=2024-09&org=RVJ&format=json

Authorization: Bearer <token>
```

**Use Cases:**
- Integration with BI tools (Power BI, Tableau)
- Automated report generation
- Data pipeline integration
- Mobile app development

---

## 9. Alerts & Notifications

### Custom Alerts

**Create Alert**
```
Alert Name: [RTT 52-week waiters spike]
Data Source: [RTT Waiting Lists]
Condition: [Patients waiting 52+ weeks] [increases by] [>20%]
Comparison: [vs. last month]
Notify: ● Email  ☐ In-app  ☐ SMS
Recipients: [performance.team@trust.nhs.uk]
Frequency: [When triggered]
```

**Alert Types**
- Threshold breach (metric exceeds value)
- Percentage change (vs. previous period)
- Rank change (dropped in peer comparison)
- Data freshness (new data available)
- Anomaly detection (statistical outlier)

---

## 10. Data Quality & Metadata

### Data Freshness Indicators

```
Data Source         | Latest Period | Published   | Loaded     | Status
--------------------|---------------|-------------|------------|--------
RTT Waiting Lists   | Sep 2024      | 2024-10-12  | 2024-10-12 | ✓ Current
A&E Performance     | Sep 2024      | 2024-10-14  | 2024-10-14 | ✓ Current
MHSDS               | Aug 2024      | 2024-10-08  | 2024-10-08 | ⚠ Lag
Workforce (ESR)     | Jul 2024      | 2024-09-15  | 2024-09-16 | ⚠ Lag
```

### Metadata Display

For each dataset/chart:
- Source publication
- Reporting period covered
- Geographic coverage
- Known quality issues
- Link to technical guidance
- Last updated timestamp

---

## 11. Help & Documentation

### In-App Help

- Tooltips on hover (explain metrics)
- Contextual help panel
- Video tutorials
- Guided tours (for new users)

### Documentation Site

- User guide
- Data dictionary
- FAQ
- API documentation
- Release notes

---

## User Journey Examples

### Example 1: Trust Executive - Board Paper Preparation

1. Log in to ATLAS
2. Navigate to "Quick Reports"
3. Select "Trust Performance Pack"
4. Apply filters:
   - Organization: My Trust
   - Period: Last quarter
5. Review auto-generated report
6. Add custom commentary text boxes
7. Generate PDF
8. Email to board members

**Time to complete:** 5 minutes

---

### Example 2: Analyst - Custom Elective Recovery Analysis

1. Navigate to "Data Explorer"
2. Select data sources:
   - RTT Waiting Lists
   - Diagnostic Waiting Times (DM01)
   - Theatre Utilization
3. Filter:
   - ICB: My ICB
   - Period: Last 12 months
   - Specialty: Orthopaedics
4. Create visualizations:
   - Line chart: RTT waiting list size over time
   - Bar chart: Provider comparison within ICB
   - SPC chart: 52-week waiters
5. Save view: "Ortho Elective Recovery - ICB"
6. Share with elective recovery team
7. Schedule monthly email update

**Time to complete:** 15 minutes

---

### Example 3: ICB Commissioner - System Dashboard

1. Use "Dashboard Builder"
2. Add widgets:
   - ICB waiting list summary (all providers)
   - A&E performance heatmap (by trust)
   - Workforce vacancy map
   - Integration metrics (CHC, delayed discharges)
3. Set auto-refresh (daily at 9am)
4. Share dashboard with system partners
5. Present in weekly system calls

**Time to complete:** 10 minutes setup, then daily 2-minute review

---

## Future Enhancements (Post-MVP)

### Advanced Analytics

- Predictive modeling (forecast waiting lists)
- Scenario planning (what-if analysis)
- Anomaly detection (auto-flag outliers)
- Segmentation analysis (cohort identification)

### Collaboration Features

- Comments on charts
- Shared workspaces for teams
- Version control for views
- Audit log for changes

### Mobile App

- iOS and Android apps
- Push notifications for alerts
- Offline mode
- Quick dashboards

### AI Assistant

- Natural language queries ("Show me Somerset's RTT performance")
- Auto-generate insights ("What's changed this month?")
- Suggested comparisons ("Similar trusts to mine")

---

*This functionality specification provides the complete feature set for ATLAS. Implementation should be phased, starting with core features (Data Explorer, Basic Charts, Export) and expanding incrementally.*
