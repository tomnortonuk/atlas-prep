# ATLAS Organizational Hierarchy

Complete guide to NHS and care system organizational structures.

---

## NHS England Structure (2026)

### National Level

**NHS England** (NHSE)
- National commissioning and oversight
- Policy implementation
- Performance management
- Data collection and publication

### Regional Level (7 Regions)

1. **North East and Yorkshire** (Y63)
2. **North West** (Y62)
3. **Midlands** (Y60)
4. **East of England** (Y61)
5. **London** (Y56)
6. **South East** (Y59)
7. **South West** (Y58)

### Integrated Care Board (ICB) Level

**36 ICBs (as of April 2026)**

*Reduced from 42 ICBs following April 2026 reorganization*

#### North East and Yorkshire
- NHS Humber and North Yorkshire ICB
- NHS South Yorkshire ICB
- NHS West Yorkshire ICB
- NHS North East and North Cumbria ICB

#### North West
- NHS Cheshire and Merseyside ICB
- NHS Greater Manchester ICB
- NHS Lancashire and South Cumbria ICB

#### Midlands
- NHS Birmingham and Solihull ICB
- NHS Black Country ICB
- NHS Coventry and Warwickshire ICB
- NHS Derby and Derbyshire ICB
- NHS Herefordshire and Worcestershire ICB
- NHS Leicester, Leicestershire and Rutland ICB
- NHS Northamptonshire ICB
- NHS Nottingham and Nottinghamshire ICB
- NHS Shropshire, Telford and Wrekin ICB
- NHS Staffordshire and Stoke-on-Trent ICB

#### East of England
- NHS Bedfordshire, Luton and Milton Keynes ICB
- NHS Cambridgeshire and Peterborough ICB
- NHS Hertfordshire and West Essex ICB
- NHS Mid and South Essex ICB
- NHS Norfolk and Waveney ICB
- NHS Suffolk and North East Essex ICB

#### London
- NHS North Central London ICB
- NHS North East London ICB
- NHS North West London ICB
- NHS South East London ICB
- NHS South West London ICB

#### South East
- NHS Buckinghamshire, Oxfordshire and Berkshire West ICB
- NHS Frimley ICB
- NHS Hampshire and Isle of Wight ICB
- NHS Kent and Medway ICB
- NHS Surrey Heartlands ICB
- NHS Sussex ICB

#### South West
- NHS Bath and North East Somerset, Swindon and Wiltshire ICB
- NHS Bristol, North Somerset and South Gloucestershire ICB
- NHS Cornwall and the Isles of Scilly ICB
- NHS Devon ICB
- NHS Dorset ICB
- NHS Gloucestershire ICB
- NHS Somerset ICB

---

## Sub-ICB Locations (Place-Based Partnerships)

Sub-ICB locations represent place-based planning areas (typically aligned with local authorities or groups of LAs).

**Example: Somerset ICB**
- Mendip
- Sedgemoor
- South Somerset
- Somerset West and Taunton
- (Reorganized into unified Somerset Council 2023)

**Function:**
- Local health and care planning
- Integration with local authorities
- Community-based services
- Prevention and population health

---

## Provider Organizations

### NHS Trusts

**Acute Trusts**
- Provide hospital-based services
- Emergency departments
- Inpatient and outpatient care
- Examples: King's College Hospital NHS FT (RVJ), Guy's and St Thomas' NHS FT (RJ7)

**Foundation Trusts (NHS FT)**
- Greater autonomy
- Accountable to NHS England and local governors
- Can retain surpluses for reinvestment

**Mental Health Trusts**
- Specialist mental health services
- Community and inpatient MH care
- Examples: South London and Maudsley NHS FT, Mersey Care NHS FT

**Community Trusts**
- Community health services
- District nursing, health visiting
- Community therapies and rehabilitation

**Ambulance Trusts**
- Emergency and urgent care transport
- 111 services (some trusts)
- 10 ambulance trusts covering England

**Specialist Trusts**
- Tertiary and quaternary care
- Examples: Moorfields Eye Hospital, Great Ormond Street Hospital, Royal Marsden

### Trust Sites

Many trusts operate across multiple hospital sites.

**Example: University Hospitals Bristol and Weston NHS FT**
- Bristol Royal Infirmary
- Bristol Royal Hospital for Children
- Bristol Haematology and Oncology Centre
- Bristol Heart Institute
- St Michael's Hospital
- South Bristol Community Hospital
- Weston General Hospital

---

## Provider Systems & Partnerships

**Provider Collaboratives**
- Groups of trusts working together
- Shared services and specialties
- Joint planning

**Acute Provider Collaboratives** (examples):
- North Central London Provider Collaborative
- South Yorkshire Provider Collaborative

**Provider System Partnerships** (non-statutory):
- Voluntary arrangements
- Lead provider model
- Examples: East of England Community Health and Care NHS Trust (formed from multiple community providers)

---

## Primary Care Organizations

### General Practices

- Independent contractors (GMS, PMS, APMS contracts)
- Organized into ~6,500 practices in England
- Registered patient lists

### Primary Care Networks (PCNs)

- Groups of practices covering 30,000-50,000 population
- Additional roles reimbursement scheme (ARRS)
- Enhanced access and extended hours
- ~1,250 PCNs in England

---

## Social Care Organizations

### Local Authorities (Upper Tier)

**Responsibilities:**
- Adult social care
- Public health (from 2013)
- Children's social care

**Types:**
- County councils (e.g., Somerset County Council)
- Unitary authorities (e.g., Bristol City Council)
- London boroughs
- Metropolitan districts

**152 upper-tier local authorities** in England (responsible for social care)

### Care Providers

**Care Quality Commission (CQC) Regulated**

**Care Homes:**
- Nursing homes
- Residential care homes
- ~11,000 care home providers with ~400,000 beds

**Home Care:**
- Domiciliary care agencies
- ~9,000 domiciliary care providers

**Supported Living:**
- Independent living with support
- Learning disabilities, mental health, older people

---

## Commissioning & Planning

### Integrated Care Systems (ICS)

**Each ICS comprises:**
1. **Integrated Care Board (ICB)** - Statutory commissioning body
2. **Integrated Care Partnership (ICP)** - Broader partnership (health, LA, VCSE)

**Functions:**
- Strategic planning
- Population health management
- Delegated commissioning (primary care, specialized services)
- System oversight

### NHS England Direct Commissioning

**Nationally commissioned services:**
- Primary care (delegated to ICBs in many areas)
- Specialized services (e.g., organ transplant, specialised cancer)
- Armed forces health
- Health and justice (prisons)
- Public health functions (screening, immunisation)

---

## Organizational Data Service (ODS)

### ODS API

**Source:** NHS Digital (now NHS England)
**Purpose:** Master registry of all NHS organizations

**Access:**
- ORD API (Organisation Reference Data)
- FHIR R4 API
- DSE predefined reports (Data Service for Evaluation)

**Organization Types in ODS:**
- NHS Trust
- Clinical Commissioning Group (legacy, dissolved July 2022)
- Integrated Care Board
- GP Practice
- Care Home
- Pharmacy
- And 80+ other organization types

**Key ODS Fields:**
- ODS Code (unique identifier, e.g., RVJ)
- Organization name
- Status (Active, Inactive, Proposed)
- Open/close dates
- Address and postcode
- Parent organization
- Successor organization (for mergers)

---

## Trust Mergers & Succession

### Recent Major Mergers (2020-2026)

**Bristol NHS FT (2020)**
- Predecessor: University Hospitals Bristol NHS FT (RVJ)
- Predecessor: Weston Area Health NHS Trust (RK9)
- Successor: University Hospitals Bristol and Weston NHS FT (RVJ - retained code)

**East of England Community Health and Care NHS Trust (2024)**
- Merger of multiple community providers
- Covers Norfolk, Suffolk, Bedfordshire, Hertfordshire

**Liverpool University Hospitals NHS FT (2019)**
- Aintree University Hospital NHS FT
- Royal Liverpool and Broadgreen University Hospitals NHS Trust

### Tracking Succession

**Historical Data Mapping:**
When querying pre-merger data:
```sql
-- Find all predecessors of current organization
WITH RECURSIVE predecessors AS (
  SELECT ods_code, organization_name, predecessor_code
  FROM organizations
  WHERE ods_code = 'RVJ'  -- Current Bristol NHS FT
  UNION ALL
  SELECT o.ods_code, o.organization_name, o.predecessor_code
  FROM organizations o
  INNER JOIN predecessors p ON o.ods_code = p.predecessor_code
)
SELECT * FROM predecessors;

-- Aggregate historical data for merged trusts
SELECT 
    SUM(activity) as total_activity
FROM historical_data
WHERE provider_ods IN (
    SELECT ods_code FROM predecessors
);
```

---

## Geographic Hierarchies

### ONS Statistical Geographies

**Lower Super Output Area (LSOA)**
- ~35,000 LSOAs in England
- Avg population: 1,500
- Used for deprivation indices (IMD)

**Middle Super Output Area (MSOA)**
- ~7,000 MSOAs
- Avg population: 7,200
- Used for health profiles

**Local Authority Districts (LAD)**
- 309 districts in England (including London boroughs)
- Lower-tier and upper-tier authorities

**NHS Geographies**
- Sub-ICB Location
- ICB (36 in England)
- Region (7 in England)

### Geographic Crosswalks

**LSOA → SICBL → ICB → Region**

Available from ONS Geography Portal and NHS Digital.

**Example:**
```
LSOA: E01015187 (postcode area TA1)
MSOA: E02004126
LA: Somerset (E10000027)
SICBL: Somerset (to be assigned)
ICB: NHS Somerset ICB (15M)
Region: South West (Y58)
```

---

## Data Reporting Levels

ATLAS supports reporting at multiple organizational levels:

### 1. National
- All-England aggregates
- National trends
- Benchmark reference

### 2. Regional
- 7 NHS England regions
- Regional variation analysis
- Regional performance comparison

### 3. Integrated Care Board (ICB)
- 36 ICBs
- System-level planning
- Provider comparison within system

### 4. Sub-ICB Location (SICBL)
- Place-based reporting
- Local authority alignment
- Integration with social care

### 5. Trust / Provider
- Individual NHS trust
- Independent sector provider
- Care provider organization

### 6. Site / Hospital
- Individual hospital within a trust
- Community service location
- Care home location

### 7. Provider System
- Collaborative arrangements
- Linked trusts
- Shared service models

### 8. Local Authority
- Social care responsibility area
- Public health planning
- Integration with NHS

---

## Organization Lookup Implementation

### API Integration

**ODS ORD API:**
```bash
GET https://directory.spineservices.nhs.uk/ORD/2-0-0/organisations
?Status=Active&OrgRecordClass=RC1  # NHS Trusts
```

**Response:**
```xml
<Organisation>
  <OrgId>
    <extension>RVJ</extension>
  </OrgId>
  <Name>University Hospitals Bristol and Weston NHS Foundation Trust</Name>
  <Status>Active</Status>
  <OrgRecordClass>RC1</OrgRecordClass>
  <PostCode>BS1 3NU</PostCode>
  <Rels>
    <Rel>
      <Target>QF7</Target> <!-- Parent ICB -->
      <Status>Active</Status>
    </Rel>
  </Rels>
</Organisation>
```

### Database Population

**Initial Load:**
1. Fetch all active organizations from ODS
2. Parse organization details and relationships
3. Insert into `organizations` table
4. Build hierarchy in `organization_relationships`
5. Record succession chains in `organization_succession`

**Incremental Updates:**
1. Daily sync via ODS API
2. Track changes (new, updated, closed orgs)
3. Update succession when mergers occur
4. Maintain historical records (soft delete)

---

## Reporting Scenarios

### Scenario 1: Trust Board Report

**User:** Somerset NHS FT executive
**Requirement:** Trust-level performance metrics

**Implementation:**
```sql
SELECT * FROM performance_metrics
WHERE provider_ods_code = 'RH5'  -- Somerset NHS FT
AND reporting_period >= '2024-01-01';
```

### Scenario 2: ICB System Dashboard

**User:** Somerset ICB commissioner
**Requirement:** All providers within ICB

**Implementation:**
```sql
SELECT 
    o.organization_name,
    p.metric_value
FROM performance_metrics p
INNER JOIN organizations o ON p.provider_ods_code = o.ods_code
WHERE o.icb_code = '15M'  -- Somerset ICB
AND p.reporting_period = '2024-09';
```

### Scenario 3: Regional Comparison

**User:** NHS England regional team
**Requirement:** Compare all ICBs in South West

**Implementation:**
```sql
SELECT 
    o.icb_code,
    MAX(o.organization_name) as icb_name,
    AVG(p.metric_value) as avg_metric
FROM performance_metrics p
INNER JOIN organizations o ON p.provider_ods_code = o.ods_code
WHERE o.region_code = 'Y58'  -- South West
GROUP BY o.icb_code;
```

### Scenario 4: Historical Analysis (Pre-Merger)

**User:** Bristol NHS FT analyst
**Requirement:** Compare current trust to historical predecessors

**Implementation:**
```sql
-- Get all predecessor codes
WITH predecessors AS (
  SELECT ods_code FROM organizations
  WHERE ods_code = 'RVJ'  -- Current
  OR successor_code = 'RVJ'  -- Predecessors
)
SELECT 
    CASE 
        WHEN provider_ods_code = 'RVJ' THEN 'Current (Merged)'
        ELSE 'Pre-merger'
    END as period,
    SUM(activity) as total_activity
FROM historical_data
WHERE provider_ods_code IN (SELECT ods_code FROM predecessors)
GROUP BY period;
```

---

## Key Considerations for ATLAS

### 1. Dynamic Hierarchy
- Organizations open, close, merge frequently
- Must handle historical changes
- Cannot assume static structure

### 2. Multiple Parent Relationships
- Trust may be in one ICB but have sites in another
- Cross-boundary services
- Solution: Allow multiple relationship types

### 3. Aggregation Flexibility
- User should choose aggregation level dynamically
- Drill-down from national → regional → ICB → trust → site
- Drill-up supported

### 4. Labeling Clarity
- Always show organization type with name
- Example: "Somerset NHS FT (Acute Trust)" vs "Somerset ICB (Commissioner)"
- Avoid confusion between "Somerset" entities

### 5. Postcode-Based Search
- Users may not know ODS codes
- Allow search by postcode → find nearest providers
- Requires geographic lookup

---

*This organizational hierarchy guide provides the foundation for ATLAS's multi-level reporting and navigation capabilities.*
