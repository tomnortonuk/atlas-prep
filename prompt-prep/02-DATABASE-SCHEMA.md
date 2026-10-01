# ATLAS Database Schema

Complete SQLite database schema design for ATLAS platform.

**Database:** SQLite (local Docker) / Cloudflare D1 (production)

---

## Schema Overview

The ATLAS database is organized into six logical domains:

1. **Data Source Management** - Catalog and availability tracking
2. **Organizational Hierarchy** - NHS/care structure and relationships  
3. **Geographic Mapping** - LSOA to ICB to Region to LA lookups
4. **Insight Categorization** - Novel multi-dimensional tagging
5. **User Customization** - Saved views, folders, published reports
6. **Data Tables** - Actual health and care data (dataset-specific)

---

## 1. DATA SOURCE MANAGEMENT

### 1.1 `data_sources`

Master catalog of all data sources integrated into ATLAS.

```sql
CREATE TABLE data_sources (
    source_id TEXT PRIMARY KEY,           -- e.g., 'HES_APC', 'MHSDS', 'RTT'
    source_name TEXT NOT NULL,            -- Display name
    source_description TEXT,
    source_category TEXT,                 -- 'Acute', 'Mental Health', 'Social Care', etc.
    publisher TEXT,                       -- 'NHS England', 'UKHSA', 'CQC'
    update_frequency TEXT,                -- 'Monthly', 'Quarterly', 'Annual'
    typical_lag_days INTEGER,             -- Average lag from period end to publication
    endpoint_url TEXT,                    -- API or download URL
    granularity TEXT,                     -- 'Episode', 'Person', 'Trust Aggregate', etc.
    active BOOLEAN DEFAULT TRUE,
    notes TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Indexes
CREATE INDEX idx_sources_category ON data_sources(source_category);
CREATE INDEX idx_sources_active ON data_sources(active);
```

### 1.2 `data_availability`

Tracks which data periods have been published and loaded for each source.

```sql
CREATE TABLE data_availability (
    availability_id INTEGER PRIMARY KEY AUTOINCREMENT,
    source_id TEXT NOT NULL,
    reporting_period_start DATE NOT NULL,  -- Period covered by data
    reporting_period_end DATE NOT NULL,
    publication_date DATE,                  -- When published by source
    loaded_date DATE,                       -- When loaded into ATLAS
    load_status TEXT DEFAULT 'pending',     -- 'pending', 'loading', 'complete', 'failed'
    checksum TEXT,                          -- MD5/SHA of source file for change detection
    record_count INTEGER,
    error_message TEXT,
    FOREIGN KEY (source_id) REFERENCES data_sources(source_id),
    UNIQUE(source_id, reporting_period_start, reporting_period_end)
);

-- Indexes
CREATE INDEX idx_avail_source ON data_availability(source_id);
CREATE INDEX idx_avail_period ON data_availability(reporting_period_start, reporting_period_end);
CREATE INDEX idx_avail_status ON data_availability(load_status);
```

---

## 2. ORGANIZATIONAL HIERARCHY

### 2.1 `organizations`

Master registry of all NHS and care organizations (based on ODS).

```sql
CREATE TABLE organizations (
    ods_code TEXT PRIMARY KEY,              -- e.g., 'RVJ' (King's College), 'QWE' (ICB)
    organization_name TEXT NOT NULL,
    organization_type TEXT NOT NULL,        -- 'NHS Trust', 'ICB', 'Care Home', 'LA', etc.
    sub_type TEXT,                          -- 'Acute', 'Mental Health', 'Ambulance', 'Community'
    status TEXT DEFAULT 'Active',           -- 'Active', 'Inactive', 'Proposed', 'Closed'
    open_date DATE,
    close_date DATE,
    address_line1 TEXT,
    address_line2 TEXT,
    city TEXT,
    postcode TEXT,
    region_code TEXT,                       -- NHS England region (e.g., 'Y56' = South West)
    region_name TEXT,
    icb_code TEXT,                          -- Parent ICB if applicable
    parent_ods_code TEXT,                   -- Immediate parent in hierarchy
    predecessor_code TEXT,                  -- Organization this one succeeded (for mergers)
    successor_code TEXT,                    -- Organization that succeeded this one
    national_grouping TEXT,                 -- 'NHS', 'Independent', 'LA', 'Voluntary'
    latitude REAL,
    longitude REAL,
    contact_telephone TEXT,
    contact_email TEXT,
    website TEXT,
    last_updated DATE,
    FOREIGN KEY (parent_ods_code) REFERENCES organizations(ods_code),
    FOREIGN KEY (predecessor_code) REFERENCES organizations(ods_code),
    FOREIGN KEY (successor_code) REFERENCES organizations(ods_code),
    FOREIGN KEY (icb_code) REFERENCES organizations(ods_code)
);

-- Indexes
CREATE INDEX idx_org_type ON organizations(organization_type);
CREATE INDEX idx_org_status ON organizations(status);
CREATE INDEX idx_org_icb ON organizations(icb_code);
CREATE INDEX idx_org_region ON organizations(region_code);
CREATE INDEX idx_org_parent ON organizations(parent_ods_code);
CREATE INDEX idx_org_name ON organizations(organization_name);
```

### 2.2 `organization_relationships`

Explicit parent-child relationships for hierarchical reporting.

```sql
CREATE TABLE organization_relationships (
    relationship_id INTEGER PRIMARY KEY AUTOINCREMENT,
    child_ods_code TEXT NOT NULL,
    parent_ods_code TEXT NOT NULL,
    relationship_type TEXT NOT NULL,        -- 'ICB_Member', 'Site_Of', 'Commissioned_By', etc.
    start_date DATE NOT NULL,
    end_date DATE,
    FOREIGN KEY (child_ods_code) REFERENCES organizations(ods_code),
    FOREIGN KEY (parent_ods_code) REFERENCES organizations(ods_code),
    UNIQUE(child_ods_code, parent_ods_code, relationship_type, start_date)
);

-- Indexes
CREATE INDEX idx_rel_child ON organization_relationships(child_ods_code);
CREATE INDEX idx_rel_parent ON organization_relationships(parent_ods_code);
CREATE INDEX idx_rel_type ON organization_relationships(relationship_type);
CREATE INDEX idx_rel_dates ON organization_relationships(start_date, end_date);
```

### 2.3 `organization_succession`

Tracks mergers, acquisitions, splits, and reorganizations.

```sql
CREATE TABLE organization_succession (
    succession_id INTEGER PRIMARY KEY AUTOINCREMENT,
    predecessor_ods_code TEXT NOT NULL,
    successor_ods_code TEXT NOT NULL,
    succession_type TEXT NOT NULL,          -- 'Merger', 'Split', 'Acquisition', 'Reorganization'
    succession_date DATE NOT NULL,
    notes TEXT,
    FOREIGN KEY (predecessor_ods_code) REFERENCES organizations(ods_code),
    FOREIGN KEY (successor_ods_code) REFERENCES organizations(ods_code)
);

-- Indexes
CREATE INDEX idx_succ_pred ON organization_succession(predecessor_ods_code);
CREATE INDEX idx_succ_succ ON organization_succession(successor_ods_code);
CREATE INDEX idx_succ_date ON organization_succession(succession_date);
```

### 2.4 `icb_hierarchy`

Explicit ICB → Sub-ICB → Region mapping.

```sql
CREATE TABLE icb_hierarchy (
    hierarchy_id INTEGER PRIMARY KEY AUTOINCREMENT,
    sicbl_code TEXT,                        -- Sub-ICB Location code
    sicbl_name TEXT,
    icb_code TEXT NOT NULL,
    icb_name TEXT NOT NULL,
    region_code TEXT NOT NULL,
    region_name TEXT NOT NULL,
    start_date DATE DEFAULT '2022-07-01',   -- ICBs created July 2022
    end_date DATE,
    FOREIGN KEY (icb_code) REFERENCES organizations(ods_code)
);

-- Indexes
CREATE INDEX idx_icbh_sicbl ON icb_hierarchy(sicbl_code);
CREATE INDEX idx_icbh_icb ON icb_hierarchy(icb_code);
CREATE INDEX idx_icbh_region ON icb_hierarchy(region_code);
```

### 2.5 `provider_systems`

Linked/merged trust groupings (e.g., Bristol hospitals partnership).

```sql
CREATE TABLE provider_systems (
    system_id INTEGER PRIMARY KEY AUTOINCREMENT,
    system_name TEXT NOT NULL,
    system_description TEXT,
    lead_provider_ods TEXT,                 -- Lead organization in system
    start_date DATE,
    end_date DATE,
    FOREIGN KEY (lead_provider_ods) REFERENCES organizations(ods_code)
);

CREATE TABLE provider_system_members (
    membership_id INTEGER PRIMARY KEY AUTOINCREMENT,
    system_id INTEGER NOT NULL,
    member_ods_code TEXT NOT NULL,
    start_date DATE,
    end_date DATE,
    FOREIGN KEY (system_id) REFERENCES provider_systems(system_id),
    FOREIGN KEY (member_ods_code) REFERENCES organizations(ods_code)
);

-- Indexes
CREATE INDEX idx_psm_system ON provider_system_members(system_id);
CREATE INDEX idx_psm_member ON provider_system_members(member_ods_code);
```

---

## 3. GEOGRAPHIC MAPPING

### 3.1 `geographic_lookup`

LSOA to higher geographies (ONS-based).

```sql
CREATE TABLE geographic_lookup (
    lsoa_code TEXT PRIMARY KEY,             -- Lower Super Output Area
    lsoa_name TEXT,
    msoa_code TEXT,                         -- Middle Super Output Area
    msoa_name TEXT,
    la_code TEXT,                           -- Local Authority
    la_name TEXT,
    sicbl_code TEXT,                        -- Sub-ICB Location
    sicbl_name TEXT,
    icb_code TEXT,                          -- Integrated Care Board
    icb_name TEXT,
    region_code TEXT,                       -- NHS England region
    region_name TEXT,
    country_code TEXT DEFAULT 'E92000001',  -- England
    imd_decile INTEGER,                     -- Index of Multiple Deprivation
    rural_urban_classification TEXT,
    FOREIGN KEY (icb_code) REFERENCES organizations(ods_code)
);

-- Indexes
CREATE INDEX idx_geo_msoa ON geographic_lookup(msoa_code);
CREATE INDEX idx_geo_la ON geographic_lookup(la_code);
CREATE INDEX idx_geo_sicbl ON geographic_lookup(sicbl_code);
CREATE INDEX idx_geo_icb ON geographic_lookup(icb_code);
CREATE INDEX idx_geo_region ON geographic_lookup(region_code);
```

### 3.2 `local_authority_mapping`

Local authority to NHS geography crosswalk.

```sql
CREATE TABLE local_authority_mapping (
    la_code TEXT PRIMARY KEY,
    la_name TEXT NOT NULL,
    la_type TEXT,                           -- 'County', 'Unitary', 'District', 'London Borough'
    region_code TEXT,
    region_name TEXT,
    population INTEGER,
    area_sq_km REAL
);

-- Indexes
CREATE INDEX idx_la_region ON local_authority_mapping(region_code);
```

---

## 4. INSIGHT CATEGORIZATION

### 4.1 `insight_categories`

Novel multi-dimensional categorization beyond traditional service types.

```sql
CREATE TABLE insight_categories (
    category_id INTEGER PRIMARY KEY AUTOINCREMENT,
    category_name TEXT UNIQUE NOT NULL,
    category_description TEXT,
    category_type TEXT NOT NULL,            -- 'Patient Journey', 'System Pressure', 'Population Health', etc.
    display_order INTEGER,
    icon TEXT                                -- Icon identifier for UI
);

-- Example categories:
INSERT INTO insight_categories (category_name, category_type, display_order) VALUES
('Prevention & Early Intervention', 'Patient Journey', 1),
('First Contact & Access', 'Patient Journey', 2),
('Diagnosis & Assessment', 'Patient Journey', 3),
('Treatment & Care', 'Patient Journey', 4),
('Recovery & Rehabilitation', 'Patient Journey', 5),
('Ongoing Support', 'Patient Journey', 6),
('End of Life Care', 'Patient Journey', 7),

('A&E Crowding', 'System Pressure', 10),
('Elective Backlogs', 'System Pressure', 11),
('Bed Capacity', 'System Pressure', 12),
('Workforce Shortages', 'System Pressure', 13),
('Social Care Delays', 'System Pressure', 14),

('Deprivation & Inequalities', 'Population Health', 20),
('Healthy Life Expectancy', 'Population Health', 21),
('Risk Factors', 'Population Health', 22),
('Mental Wellbeing', 'Population Health', 23),

('Health & Social Care Interface', 'Cross-Agency', 30),
('Police & Health Partnerships', 'Cross-Agency', 31),
('Fire & Health Prevention', 'Cross-Agency', 32),
('Housing & Health', 'Cross-Agency', 33),

('Patient Safety', 'Quality & Safety', 40),
('Infection Prevention', 'Quality & Safety', 41),
('Clinical Effectiveness', 'Quality & Safety', 42),

('Productivity & Efficiency', 'Productivity', 50),
('Length of Stay', 'Productivity', 51),
('Did Not Attend', 'Productivity', 52),
('Theatre Utilization', 'Productivity', 53);
```

### 4.2 `source_category_mapping`

Maps data sources to insight categories (many-to-many).

```sql
CREATE TABLE source_category_mapping (
    mapping_id INTEGER PRIMARY KEY AUTOINCREMENT,
    source_id TEXT NOT NULL,
    category_id INTEGER NOT NULL,
    relevance_score INTEGER DEFAULT 5,      -- 1-10 scale, for sorting/filtering
    FOREIGN KEY (source_id) REFERENCES data_sources(source_id),
    FOREIGN KEY (category_id) REFERENCES insight_categories(category_id),
    UNIQUE(source_id, category_id)
);

-- Indexes
CREATE INDEX idx_scm_source ON source_category_mapping(source_id);
CREATE INDEX idx_scm_category ON source_category_mapping(category_id);
```

---

## 5. USER CUSTOMIZATION

### 5.1 `user_saved_views`

User-created custom reports and filter combinations.

```sql
CREATE TABLE user_saved_views (
    view_id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id TEXT NOT NULL,                  -- User identifier (from auth system)
    view_name TEXT NOT NULL,
    view_description TEXT,
    folder_id INTEGER,                      -- Optional folder organization
    data_source_ids TEXT NOT NULL,          -- JSON array of source_ids
    filters_json TEXT,                      -- JSON object with filter config
    chart_config_json TEXT,                 -- JSON object with visualization config
    is_public BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (folder_id) REFERENCES user_folders(folder_id)
);

-- Indexes
CREATE INDEX idx_views_user ON user_saved_views(user_id);
CREATE INDEX idx_views_folder ON user_saved_views(folder_id);
CREATE INDEX idx_views_public ON user_saved_views(is_public);
```

### 5.2 `user_folders`

Hierarchical folder structure for organizing saved views.

```sql
CREATE TABLE user_folders (
    folder_id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id TEXT NOT NULL,
    folder_name TEXT NOT NULL,
    parent_folder_id INTEGER,               -- For nested folders
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (parent_folder_id) REFERENCES user_folders(folder_id)
);

-- Indexes
CREATE INDEX idx_folders_user ON user_folders(user_id);
CREATE INDEX idx_folders_parent ON user_folders(parent_folder_id);
```

### 5.3 `published_reports`

Static published reports for sharing.

```sql
CREATE TABLE published_reports (
    report_id INTEGER PRIMARY KEY AUTOINCREMENT,
    report_name TEXT NOT NULL,
    report_description TEXT,
    created_by_user_id TEXT NOT NULL,
    view_id INTEGER,                        -- Based on a saved view
    static_snapshot_date DATE,              -- Data snapshot date
    pdf_url TEXT,                           -- Link to generated PDF
    html_url TEXT,                          -- Link to static HTML
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (view_id) REFERENCES user_saved_views(view_id)
);

-- Indexes
CREATE INDEX idx_reports_user ON published_reports(created_by_user_id);
CREATE INDEX idx_reports_view ON published_reports(view_id);
```

### 5.4 `report_access`

Access control for published reports.

```sql
CREATE TABLE report_access (
    access_id INTEGER PRIMARY KEY AUTOINCREMENT,
    report_id INTEGER NOT NULL,
    user_id TEXT,                           -- NULL = public access
    organization_ods TEXT,                  -- Share with specific org
    access_level TEXT DEFAULT 'view',       -- 'view', 'download'
    granted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (report_id) REFERENCES published_reports(report_id),
    FOREIGN KEY (organization_ods) REFERENCES organizations(ods_code)
);

-- Indexes
CREATE INDEX idx_access_report ON report_access(report_id);
CREATE INDEX idx_access_user ON report_access(user_id);
CREATE INDEX idx_access_org ON report_access(organization_ods);
```

### 5.5 `user_chart_templates`

Reusable chart configurations.

```sql
CREATE TABLE user_chart_templates (
    template_id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id TEXT NOT NULL,
    template_name TEXT NOT NULL,
    chart_type TEXT NOT NULL,               -- 'line', 'bar', 'spc', 'heatmap', etc.
    config_json TEXT NOT NULL,              -- Full chart configuration
    is_shared BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Indexes
CREATE INDEX idx_templates_user ON user_chart_templates(user_id);
CREATE INDEX idx_templates_type ON user_chart_templates(chart_type);
```

---

## 6. EXAMPLE DATA TABLES

Below are example schemas for specific datasets. Each data source will have its own table(s).

### 6.1 `rtt_waiting_list` (Referral to Treatment)

```sql
CREATE TABLE rtt_waiting_list (
    record_id INTEGER PRIMARY KEY AUTOINCREMENT,
    reporting_period DATE NOT NULL,
    provider_ods_code TEXT NOT NULL,
    specialty_code TEXT NOT NULL,
    specialty_name TEXT,
    treatment_function_code TEXT,
    pathway_type TEXT,                      -- 'Admitted', 'Non-Admitted', 'Incomplete'
    weeks_waiting_band TEXT,                -- '0-1', '1-2', ..., '52+'
    patient_count INTEGER NOT NULL,
    FOREIGN KEY (provider_ods_code) REFERENCES organizations(ods_code)
);

-- Indexes
CREATE INDEX idx_rtt_period ON rtt_waiting_list(reporting_period);
CREATE INDEX idx_rtt_provider ON rtt_waiting_list(provider_ods_code);
CREATE INDEX idx_rtt_specialty ON rtt_waiting_list(specialty_code);
```

### 6.2 `ae_attendances` (A&E via ECDS summary)

```sql
CREATE TABLE ae_attendances (
    record_id INTEGER PRIMARY KEY AUTOINCREMENT,
    reporting_period DATE NOT NULL,
    provider_ods_code TEXT NOT NULL,
    department_type TEXT,                   -- 'Type 1', 'Type 2', 'Type 3'
    total_attendances INTEGER,
    attendances_4hr_breaches INTEGER,
    attendances_12hr_trolley_waits INTEGER,
    ambulance_handover_30_60min INTEGER,
    ambulance_handover_over_60min INTEGER,
    FOREIGN KEY (provider_ods_code) REFERENCES organizations(ods_code)
);

-- Indexes
CREATE INDEX idx_ae_period ON ae_attendances(reporting_period);
CREATE INDEX idx_ae_provider ON ae_attendances(provider_ods_code);
```

### 6.3 `workforce_monthly` (ESR Workforce)

```sql
CREATE TABLE workforce_monthly (
    record_id INTEGER PRIMARY KEY AUTOINCREMENT,
    reporting_period DATE NOT NULL,
    org_ods_code TEXT NOT NULL,
    staff_group TEXT NOT NULL,              -- 'Nursing', 'Medical', 'AHP', etc.
    occupation_code TEXT,
    headcount INTEGER,
    fte REAL,
    vacancies_fte REAL,
    sickness_absence_rate REAL,
    turnover_rate REAL,
    bank_staff_fte REAL,
    agency_staff_fte REAL,
    FOREIGN KEY (org_ods_code) REFERENCES organizations(ods_code)
);

-- Indexes
CREATE INDEX idx_wf_period ON workforce_monthly(reporting_period);
CREATE INDEX idx_wf_org ON workforce_monthly(org_ods_code);
CREATE INDEX idx_wf_group ON workforce_monthly(staff_group);
```

### 6.4 `shmi_mortality` (Summary Hospital Mortality Indicator)

```sql
CREATE TABLE shmi_mortality (
    record_id INTEGER PRIMARY KEY AUTOINCREMENT,
    reporting_period_start DATE NOT NULL,
    reporting_period_end DATE NOT NULL,
    provider_ods_code TEXT NOT NULL,
    diagnosis_group TEXT,                   -- 'All', or specific condition
    observed_deaths INTEGER,
    expected_deaths REAL,
    shmi_ratio REAL,
    shmi_banding TEXT,                      -- 'Lower', 'As expected', 'Higher'
    FOREIGN KEY (provider_ods_code) REFERENCES organizations(ods_code)
);

-- Indexes
CREATE INDEX idx_shmi_period ON shmi_mortality(reporting_period_start, reporting_period_end);
CREATE INDEX idx_shmi_provider ON shmi_mortality(provider_ods_code);
```

---

## Recursive Queries for Hierarchies

### Find All Child Organizations

```sql
WITH RECURSIVE org_tree AS (
    -- Base case: start with parent org
    SELECT ods_code, organization_name, parent_ods_code, 0 as level
    FROM organizations
    WHERE ods_code = 'RVJ'  -- Example: King's College Hospital
    
    UNION ALL
    
    -- Recursive case: find children
    SELECT o.ods_code, o.organization_name, o.parent_ods_code, ot.level + 1
    FROM organizations o
    INNER JOIN org_tree ot ON o.parent_ods_code = ot.ods_code
)
SELECT * FROM org_tree;
```

### Trace Succession Chain

```sql
WITH RECURSIVE succession_chain AS (
    -- Base case: start with current org
    SELECT ods_code, organization_name, predecessor_code, 0 as chain_position
    FROM organizations
    WHERE ods_code = 'R0A'  -- Example: current Bristol NHS FT
    
    UNION ALL
    
    -- Recursive case: follow predecessors
    SELECT o.ods_code, o.organization_name, o.predecessor_code, sc.chain_position + 1
    FROM organizations o
    INNER JOIN succession_chain sc ON o.ods_code = sc.predecessor_code
)
SELECT * FROM succession_chain
ORDER BY chain_position DESC;  -- Oldest first
```

---

## Data Quality & Integrity

### Constraints

1. **Referential Integrity:** All foreign keys enforced
2. **Date Validity:** `end_date >= start_date` where applicable
3. **Unique Constraints:** Prevent duplicate records
4. **Not Null:** Key fields cannot be null

### Triggers (Examples)

```sql
-- Auto-update updated_at timestamp
CREATE TRIGGER update_datasources_timestamp 
AFTER UPDATE ON data_sources
FOR EACH ROW
BEGIN
    UPDATE data_sources 
    SET updated_at = CURRENT_TIMESTAMP 
    WHERE source_id = NEW.source_id;
END;

-- Validate date ranges
CREATE TRIGGER validate_org_dates
BEFORE INSERT ON organizations
FOR EACH ROW
WHEN NEW.close_date IS NOT NULL AND NEW.close_date < NEW.open_date
BEGIN
    SELECT RAISE(ABORT, 'close_date must be >= open_date');
END;
```

---

## Performance Optimization

### Indexing Strategy
- All foreign keys indexed
- Date fields used in queries indexed
- Composite indexes for common filter combinations
- Full-text search indexes for name fields (if needed)

### Materialized Views / Aggregation Tables

For common expensive queries, pre-compute:

```sql
-- Example: ICB-level monthly waiting list summary
CREATE TABLE agg_rtt_by_icb_monthly AS
SELECT 
    r.reporting_period,
    o.icb_code,
    o.region_code,
    r.specialty_code,
    SUM(r.patient_count) as total_waiting
FROM rtt_waiting_list r
INNER JOIN organizations o ON r.provider_ods_code = o.ods_code
GROUP BY r.reporting_period, o.icb_code, o.region_code, r.specialty_code;

CREATE INDEX idx_agg_rtt_icb_period ON agg_rtt_by_icb_monthly(icb_code, reporting_period);
```

---

## Database Initialization Script

```sql
-- Step 1: Create core tables
-- (Run all CREATE TABLE statements above)

-- Step 2: Load reference data
-- Load ODS organizations
-- Load geographic lookups
-- Load ICB hierarchy
-- Load insight categories

-- Step 3: Create indexes
-- (All CREATE INDEX statements above)

-- Step 4: Create views
CREATE VIEW v_active_trusts AS
SELECT * FROM organizations 
WHERE organization_type = 'NHS Trust' 
AND status = 'Active';

CREATE VIEW v_latest_data_availability AS
SELECT 
    source_id,
    MAX(reporting_period_end) as latest_period
FROM data_availability
WHERE load_status = 'complete'
GROUP BY source_id;

-- Step 5: Initial data source catalog
INSERT INTO data_sources (source_id, source_name, source_category, publisher, update_frequency) VALUES
('RTT', 'Referral to Treatment', 'Operational Performance', 'NHS England', 'Monthly'),
('ECDS', 'Emergency Care Data Set', 'Emergency Care', 'NHS England', 'Monthly'),
('MHSDS', 'Mental Health Services Data Set', 'Mental Health', 'NHS England', 'Monthly'),
('ESR_WF', 'ESR Workforce Statistics', 'Workforce', 'NHS England', 'Monthly'),
('SHMI', 'Summary Hospital Mortality Indicator', 'Mortality & Outcomes', 'NHS England', 'Quarterly');
```

---

*This schema provides the foundation for ATLAS. Implementation should start with core tables (organizations, data_sources, data_availability) and expand incrementally as data sources are added.*
