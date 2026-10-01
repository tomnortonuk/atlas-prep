# ATLAS Data Sources Catalog

Complete reference of all NHS and care system datasets for integration into ATLAS platform.

**Last Updated:** October 2026

---

## Overview

ATLAS integrates **28+ distinct data domains** across the health and care system, covering:
- Acute hospital care
- Mental health services
- Maternity services
- Emergency care
- Community services
- Social care
- Ambulance services
- Primary care
- Workforce
- Quality & safety
- Operational performance
- Finance

---

## 1. ACUTE CARE DATASETS

### 1.1 Hospital Episode Statistics (HES)

**HES Admitted Patient Care (APC)**
- **Source:** NHS England
- **Content:** All admissions to NHS hospitals in England
- **Granularity:** Episode-level (individual periods of care under one consultant)
- **Update Frequency:** Monthly provisional, annual final
- **Key Fields:** ICD-10 diagnoses, OPCS procedures, admission method, discharge destination
- **Use Cases:** Activity analysis, casemix, LOS, readmissions, specialties

**HES Outpatient (OP)**
- **Source:** NHS England
- **Content:** Outpatient appointments and attendances
- **Granularity:** Appointment-level
- **Update Frequency:** Monthly provisional, annual final
- **Key Fields:** Specialty, attendance status, DNA rates, procedure codes
- **Use Cases:** Outpatient activity, did not attend (DNA) analysis, specialty demand

**HES Critical Care**
- **Source:** NHS England (via CCMDS)
- **Content:** Adult critical care (ICU, HDU)
- **Granularity:** Critical care period
- **Update Frequency:** Monthly provisional, annual final
- **Key Fields:** APACHE scores, organ support, outcomes
- **Use Cases:** Critical care capacity, acuity, outcomes

### 1.2 Secondary Uses Service (SUS / SUS+)

- **Source:** NHS England
- **Content:** Near real-time hospital activity (inpatient, outpatient, A&E)
- **Granularity:** Event-level
- **Update Frequency:** Daily/weekly feeds
- **Relationship to HES:** SUS is the source data for HES (SUS → processing → HES)
- **Use Cases:** Operational monitoring, commissioning, payment

### 1.3 Commissioning Data Set (CDS)

**CDS v6.3**
- **Source:** Submitted by providers to commissioners
- **Content:** Detailed care activity for commissioning and payment
- **Granularity:** Episode, outpatient, A&E
- **Update Frequency:** Monthly
- **Use Cases:** Local commissioning, contract monitoring

---

## 2. EMERGENCY CARE DATASETS

### 2.1 Emergency Care Data Set (ECDS)

**ECDS v4.0**
- **Source:** NHS England (all Type 1, 2, 3 A&E departments)
- **Content:** All A&E attendances, ambulance handovers, diagnoses, investigations
- **Granularity:** Attendance-level
- **Update Frequency:** Monthly
- **Key Fields:** SNOMED diagnoses, investigations, treatments, 4-hour performance
- **Use Cases:** A&E performance, demand patterns, ambulance handovers, diagnostics

### 2.2 MSitAE (Monthly Situation Reports - A&E)

- **Source:** NHS England operational returns
- **Content:** Trust-level A&E performance metrics
- **Granularity:** Trust aggregate, daily snapshots
- **Update Frequency:** Monthly with daily submissions
- **Key Fields:** 4-hour waits, 12-hour trolley waits, attendances by type
- **Use Cases:** National A&E performance tracking, system pressure monitoring

### 2.3 Ambulance Data

**Ambulance Quality Indicators (AQI)**
- **Source:** NHS England (via ambulance trusts)
- **Content:** Response times, incident types, clinical outcomes
- **Granularity:** Trust-level aggregates
- **Update Frequency:** Monthly
- **Key Fields:** Category 1/2/3/4 response times, conveyance rates, hear and treat, see and treat
- **Use Cases:** Ambulance performance, system integration, conveyance patterns

---

## 3. MENTAL HEALTH DATASETS

### 3.1 Mental Health Services Data Set (MHSDS)

**MHSDS v6.0.6.4** (current version)
- **Source:** NHS England (all NHS-funded mental health services)
- **Content:** Community and inpatient mental health care, IAPT, CYP services
- **Granularity:** Person-level, event-level
- **Update Frequency:** Monthly
- **Key Fields:** Referrals, assessments, contacts, discharges, outcomes (HoNOS, IAPT measures), restrictive interventions
- **Coverage:** Adults, children & young people, crisis care, eating disorders, perinatal
- **Use Cases:** Mental health activity, waiting times, outcomes, out-of-area placements

### 3.2 NHS Talking Therapies (formerly IAPT)

- **Source:** Subset of MHSDS
- **Content:** Psychological therapy services for anxiety and depression
- **Granularity:** Person-level treatment journey
- **Update Frequency:** Monthly
- **Key Fields:** Referrals, waiting times, treatment modality, recovery rates, reliable improvement
- **Use Cases:** IAPT access, outcomes, recovery rates vs national targets

---

## 4. MATERNITY DATASETS

### 4.1 Maternity Services Dataset (MSDS)

- **Source:** NHS England (all NHS maternity services)
- **Content:** Booking, antenatal care, birth, postnatal care
- **Granularity:** Mother and baby-level
- **Update Frequency:** Monthly
- **Key Fields:** Booking characteristics, smoking, BMI, birth outcomes, neonatal outcomes, interventions
- **Use Cases:** Continuity of carer, smoking cessation, outcomes by demographic, stillbirth rates

---

## 5. COMMUNITY CARE DATASETS

### 5.1 Community Services Data Set (CSDS)

**CSDS v1.6**
- **Source:** NHS England (community health services)
- **Content:** District nursing, health visiting, community therapy, rehabilitation
- **Granularity:** Contact-level
- **Update Frequency:** Monthly
- **Key Fields:** Service type, contact type, care activity, safeguarding flags
- **Use Cases:** Community activity, caseloads, skill mix, integration with acute/social care

---

## 6. SOCIAL CARE DATASETS

### 6.1 Adult Social Care Finance Return (ASC-FR)

- **Source:** NHS England (local authorities)
- **Content:** Expenditure on adult social care services
- **Granularity:** Local authority-level
- **Update Frequency:** Annual
- **Key Fields:** Gross expenditure, income, service user contributions, care home spend
- **Use Cases:** Social care funding, cost pressures, integration with health spend

### 6.2 Client Level Data (CLD)

*Replaced SALT (Short and Long Term support) in 2024*

- **Source:** NHS England (local authorities)
- **Content:** Individual service users receiving adult social care
- **Granularity:** Person-level
- **Update Frequency:** Annual collection
- **Key Fields:** Support reason (primary need), service setting, service type, demographics
- **Use Cases:** Social care demand, needs assessment, integration with health data

### 6.3 CQC Care Directory and Ratings

**Care Quality Commission (CQC) Care Directory**
- **Source:** CQC
- **Content:** All registered care providers (care homes, domiciliary care, supported living)
- **Granularity:** Service/location-level
- **Update Frequency:** Continuous updates
- **Key Fields:** Service type, beds, ratings (Outstanding/Good/Requires Improvement/Inadequate), inspection dates
- **Use Cases:** Care market intelligence, capacity planning, quality oversight

---

## 7. SPECIALIST & LONG-TERM CONDITIONS

### 7.1 Waiting Lists & RTT

**Referral to Treatment (RTT)**
- **Source:** NHS England monthly return
- **Content:** Incomplete pathways (patients still waiting for treatment)
- **Granularity:** Trust, specialty, aggregate counts
- **Update Frequency:** Monthly
- **Key Fields:** Weeks waiting, admitted/non-admitted/incomplete pathways
- **Use Cases:** Elective recovery, waiting time analysis, specialty backlogs

**Waiting List Minimum Data Set (WLMDS)**
- **Source:** Trust submissions to NHS England
- **Content:** Patient-level waiting list records (replacing aggregate RTT over time)
- **Granularity:** Patient-pathway level
- **Update Frequency:** Monthly
- **Key Fields:** Individual pathway clock starts, priority, specialty, intended management
- **Use Cases:** Granular waiting list management, longest waiters, priority cohorts

---

## 8. MORTALITY & OUTCOMES

### 8.1 Summary Hospital-level Mortality Indicator (SHMI)

*Replaced HSMR following 2011 review*

- **Source:** NHS England (calculated from HES APC and ONS death registrations)
- **Content:** Hospital-level mortality rates (deaths in hospital + 30 days post-discharge)
- **Granularity:** Trust-level, diagnosis groups
- **Update Frequency:** Quarterly with 6-month lag
- **Key Fields:** Observed deaths, expected deaths, SHMI ratio, banding
- **Use Cases:** Mortality surveillance, outlier detection, quality assurance

---

## 9. PALLIATIVE & END OF LIFE CARE

### 9.1 National Audit of Care at End of Life (NACEL)

- **Source:** Royal College of Physicians / HQIP
- **Content:** Quality of end-of-life care in acute hospitals
- **Granularity:** Trust-level audit data
- **Update Frequency:** Periodic audit rounds
- **Use Cases:** EOL quality improvement, advance care planning

### 9.2 Hospice & Palliative Care Data

**Source:** Hospice UK, Public Health England Fingertips
- **Content:** Hospice capacity, admissions, community palliative care
- **Granularity:** Provider-level, some national aggregates
- **Update Frequency:** Annual surveys
- **Use Cases:** Hospice capacity, gaps in palliative provision

---

## 10. CONTINUING HEALTHCARE

### 10.1 Continuing Healthcare (CHC) and Funded Nursing Care (FNC)

- **Source:** NHS England (ICB submissions)
- **Content:** Number of people receiving NHS continuing healthcare, fast track, FNC
- **Granularity:** ICB-level
- **Update Frequency:** Quarterly
- **Key Fields:** Assessments, successful CHC, fast track, FNC placements, spend
- **Use Cases:** CHC demand, funding flows between health and social care

---

## 11. SUBSTANCE MISUSE

### 11.1 NDTMS Core Dataset R

**National Drug Treatment Monitoring System**
- **Source:** Public Health England / OHID (via treatment providers)
- **Content:** People in drug and alcohol treatment services
- **Granularity:** Person-level treatment episode
- **Update Frequency:** Monthly/quarterly
- **Key Fields:** Substance, treatment modality, interventions, completion, demographics
- **Use Cases:** Substance misuse treatment access, outcomes, unmet need

---

## 12. CRIMINAL JUSTICE & HEALTH

### 12.1 Police Mental Health Act (Section 136) Detentions

- **Source:** Police forces (via Home Office / local partnerships)
- **Content:** Use of S136 MHA powers (public place mental health detention)
- **Granularity:** Force-level, some trust-level for health-based Place of Safety
- **Update Frequency:** Annual returns, some quarterly local data
- **Use Cases:** Mental health crisis pathways, police/NHS partnership, use of Place of Safety vs A&E

### 12.2 Prison Health Data

- **Source:** NHS England Health and Justice team
- **Content:** Healthcare provision in prisons (commissioned by NHS England)
- **Granularity:** Prison-level aggregates
- **Update Frequency:** Periodic reports
- **Use Cases:** Prison health needs, integration with community services on release

---

## 13. FIRE & RESCUE HEALTH PARTNERSHIPS

### 13.1 Home Fire Safety Visits (Safe & Well)

- **Source:** Fire and rescue services (via National Fire Chiefs Council / local data sharing)
- **Content:** Fire safety visits to vulnerable people, health referrals
- **Granularity:** Fire service area
- **Update Frequency:** Annual / ad-hoc local partnership data
- **Use Cases:** Prevention, falls, safeguarding referrals, multi-agency working

---

## 14. PATIENT SAFETY & LEARNING

### 14.1 Learn from Patient Safety Events (LFPSE)

*Replaced NRLS (decommissioned June 2024) and StEIS*

- **Source:** NHS England (replacing National Reporting and Learning System)
- **Content:** Patient safety incidents, serious incidents, learning
- **Granularity:** Incident-level (anonymised for national analysis)
- **Update Frequency:** Continuous submission, periodic analysis
- **Key Fields:** Incident type, severity, specialty, contributory factors, learning actions
- **Use Cases:** Patient safety surveillance, thematic analysis, improvement priorities

---

## 15. HEALTHCARE ASSOCIATED INFECTIONS (HCAI)

### 15.1 UKHSA HCAI Data Capture System (DCS)

**UK Health Security Agency HCAI surveillance**
- **Source:** Acute trusts mandatory reporting to UKHSA
- **Content:** MRSA bacteraemia, MSSA bacteraemia, C. difficile, E. coli, Klebsiella, Pseudomonas BSI
- **Granularity:** Trust-level, weekly/monthly aggregates
- **Update Frequency:** Weekly (some organisms), monthly publications
- **Use Cases:** Infection control, AMR surveillance, trust performance, IPC interventions

---

## 16. CLINICAL AUDITS

### 16.1 National Clinical Audit and Patient Outcomes Programme (NCAPOP)

**Managed by Healthcare Quality Improvement Partnership (HQIP)**

Key national audits include:
- National Hip Fracture Database
- Myocardial Ischaemia National Audit Project (MINAP)
- National Cardiac Arrest Audit
- National Emergency Laparotomy Audit (NELA)
- National Lung Cancer Audit
- National Diabetes Audit
- Stroke Audit (SSNAP)
- National Pregnancy in Diabetes Audit

- **Source:** HQIP / individual audit programmes
- **Content:** Clinical outcomes, process measures, case ascertainment
- **Granularity:** Trust-level, anonymised patient-level for research
- **Update Frequency:** Annual reports, quarterly dashboards (varies by audit)
- **Use Cases:** Clinical quality benchmarking, outcome tracking, improvement planning

---

## 17. OPERATIONAL PERFORMANCE

### 17.1 National Outpatient Follow-up (NOF)

- **Source:** NHS England return
- **Content:** Outpatient follow-up activity and OP transformation (PIFU, telephone, advice and guidance)
- **Granularity:** Trust-level, specialty-level
- **Update Frequency:** Monthly
- **Use Cases:** Outpatient transformation, reduction in face-to-face follow-ups, PIFU adoption

### 17.2 Diagnostic Waiting Times (DM01)

- **Source:** NHS England monthly return
- **Content:** Waiting times for 15 key diagnostics (MRI, CT, colonoscopy, echo, etc.)
- **Granularity:** Trust-level, diagnostic test type
- **Update Frequency:** Monthly
- **Key Fields:** 6-week waits, number waiting, activity
- **Use Cases:** Diagnostic capacity, backlogs, elective recovery dependencies

### 17.3 Cancer Waiting Times

- **Source:** NHS England (CWT return)
- **Content:** Cancer pathways from referral to treatment
- **Granularity:** Trust-level, cancer type
- **Update Frequency:** Monthly
- **Key Fields:** 2-week wait, 31-day, 62-day performance, 28-day faster diagnosis
- **Use Cases:** Cancer performance, pathway analysis, early diagnosis rates

---

## 18. WORKFORCE DATASETS

### 18.1 Electronic Staff Record (ESR) Workforce Statistics

- **Source:** NHS England (from ESR system used by most NHS orgs)
- **Content:** Workforce headcount, FTE, joiners, leavers, sickness, bank and agency
- **Granularity:** Trust-level, staff group, occupation code
- **Update Frequency:** Monthly
- **Key Fields:** Headcount, FTE, vacancies, sickness absence, turnover, bank/agency spend
- **Use Cases:** Workforce planning, retention, productivity, temporary staffing reliance

### 18.2 NHS Staff Survey

- **Source:** NHS England (annual staff survey)
- **Content:** Staff experience, engagement, morale, safety culture
- **Granularity:** Trust-level aggregate scores
- **Update Frequency:** Annual
- **Use Cases:** Staff wellbeing, culture, engagement benchmarking

---

## 19. FINANCE & PRODUCTIVITY

### 19.1 Model Health System / Model Hospital

- **Source:** NHS England
- **Content:** Benchmarking data on productivity, efficiency, cost
- **Granularity:** Trust-level metrics
- **Update Frequency:** Quarterly updates
- **Key Fields:** Cost per WAU (weighted activity unit), length of stay, theatre utilization, outpatient DNA, prescribing cost
- **Use Cases:** Productivity benchmarking, efficiency opportunities, peer comparison

### 19.2 PLICS (Patient-Level Information and Costing System)

- **Source:** Trust finance teams
- **Content:** Patient-level costs for activity
- **Granularity:** Episode/pathway-level costs
- **Update Frequency:** Annual submission to NHS England
- **Use Cases:** Cost analysis, service line reporting, efficiency, tariff development

### 19.3 Getting It Right First Time (GIRFT)

- **Source:** NHS England GIRFT programme
- **Content:** Specialty-specific benchmarking (clinical + operational + finance)
- **Granularity:** Trust-level packs
- **Update Frequency:** Periodic (specialty deep dives every 1-3 years)
- **Use Cases:** Unwarranted variation, best practice identification, efficiency

---

## 20. PRIMARY CARE DATASETS

### 20.1 GP Patient Survey

- **Source:** NHS England (Ipsos survey)
- **Content:** Patient experience of GP and primary care services
- **Granularity:** Practice-level, ICB-level
- **Update Frequency:** Annual (with monthly tracker)
- **Key Fields:** Access, experience, appointment availability
- **Use Cases:** Primary care quality, patient experience, access benchmarking

### 20.2 QOF (Quality and Outcomes Framework)

- **Source:** NHS Digital / NHS England (via GP systems)
- **Content:** Practice achievement against quality indicators
- **Granularity:** Practice-level
- **Update Frequency:** Annual
- **Key Fields:** Disease prevalence, screening/treatment achievement, exception rates
- **Use Cases:** Primary care quality, disease registers, prevention

---

## 21. POPULATION HEALTH & PUBLIC HEALTH

### 21.1 Public Health England Fingertips / OHID Profiles

**Office for Health Improvement and Disparities**
- **Source:** OHID (aggregates from multiple national datasets)
- **Content:** 100+ indicators covering health outcomes, wider determinants, risk factors
- **Granularity:** Local authority, ICB, region, national
- **Update Frequency:** Varies (annual to quarterly by indicator)
- **Key Indicators:** Life expectancy, healthy life expectancy, deprivation, smoking, obesity, cancer screening, immunisation
- **Use Cases:** Population health needs assessment, health inequalities, prevention planning

---

## Data Integration Challenges

### 1. **Time Misalignment**
Different datasets publish on different schedules:
- Some monthly (MHSDS, ECDS, workforce)
- Some quarterly (SHMI, CHC)
- Some annual (social care, staff survey, audits)
- Lag varies (HES is ~6 months behind, SUS is near real-time)

**Solution:** `data_availability` table tracks each source's publication schedule and loaded periods

### 2. **Granularity Mismatch**
- HES is episode-level (millions of rows)
- RTT is aggregate trust/specialty level
- CQC is provider location-level
- Fingertips is local authority-level

**Solution:** Multi-level aggregation tables and geographic crosswalks (LSOA → SICBL → ICB → LA)

### 3. **Coding Standards**
- ICD-10 for diagnoses
- OPCS-4 for procedures
- SNOMED CT for emergency care and community
- Read codes legacy in primary care
- Local codes in social care

**Solution:** Code mapping tables and standardized grouping logic

### 4. **Organization Codes**
- ODS codes for NHS providers
- CQC location IDs for care providers
- LA codes for local authorities
- ICB codes (changed in April 2022 and April 2026)

**Solution:** Master ODS registry with succession tracking and geographic crosswalks

---

## Data Sources Prioritization for MVP

### Phase 1: Core Acute & Operational (MVP)
1. RTT / WLMDS - waiting lists
2. A&E (ECDS / MSitAE) - emergency performance
3. Workforce (ESR) - staffing
4. ODS - organizations

### Phase 2: Clinical Quality & Outcomes
5. SHMI - mortality
6. HCAI - infections
7. HES APC - activity and casemix

### Phase 3: Mental Health & Community
8. MHSDS - mental health
9. CSDS - community services
10. IAPT outcomes

### Phase 4: Integration & Population Health
11. Social care (ASC-FR, CLD)
12. CQC ratings
13. Fingertips population health
14. Ambulance data

### Phase 5: Specialty & Audit
15. Clinical audits (NCAPOP)
16. Cancer waiting times
17. Maternity (MSDS)

---

## Data Access & APIs

### NHS England Data Access
Most datasets available via:
- **NHS England publications** - CSV/Excel downloads
- **NHS Digital Data Dissemination Tool** - bulk extracts
- **NHS England Open Data Portal** - API access (some datasets)

### External Sources
- **CQC API** - care provider ratings and registrations
- **OHID Fingertips API** - population health indicators
- **ONS Geography Portal** - LSOA/LA/ICB boundaries and lookups

---

*This catalog represents the full scope of data sources identified for ATLAS integration. Implementation should be phased, starting with MVP datasets (Phase 1).*
