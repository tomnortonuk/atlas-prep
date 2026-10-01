# ATLAS Project - Conversation Overview

**Project Goal:** Build a national health and care data intelligence platform enabling any hospital/trust/organization to view its own data, explore peer data, and develop insights into national datasets.

**Platform Name:** ATLAS (chosen after extensive naming research)

**Date:** October 2026

---

## Conversation Journey

### Phase 1: Data Source Discovery
**Initial Request:** Build a hospital reporting solution for NHS trust data exploration and national reporting.

**Evolution:** Expanded from NHS-only to full health and care system coverage:
- Acute care (HES, SUS)
- Mental health (MHSDS)
- Community services
- Social care
- Emergency services (ambulance, police, fire)
- Maternity services
- Workforce data
- Quality & safety
- Operational performance

### Phase 2: Technical Architecture Planning
**Key Decisions:**
- SQLite database (local prototyping in Docker, production on Cloudflare D1)
- Automated data loading with scheduled checks
- Multi-level organizational reporting (national → regional → ICB → trust → site)
- Time-aligned data management (handling different publication schedules)

**Hard Constraints:**
- Free hosting via Cloudflare Pages/Workers
- SQLite/Cloudflare D1 for data storage
- Docker for local development
- Innovative categorization beyond traditional NHS reporting

### Phase 3: Organizational Complexity
**Challenge:** NHS and care system structure is complex
- 36 ICBs (post-April 2026)
- Sub-ICB locations
- Trust mergers and succession chains
- Provider systems (linked trusts)
- Local authority integration
- Geographic hierarchies (LSOA → ICB → Region)

**Solution:** ODS (Organisation Data Service) master registry with succession tracking

### Phase 4: Platform Functionality Design
**Core Features:**
- Multi-source data integration
- Flexible reporting levels (org → national)
- Custom view saving and folder management
- Report publishing and sharing
- Static PDF export generation
- Novel insight categorization (patient journeys, system pressure, population health)

**Visualization Requirements:**
- Chart.js, Apache ECharts, Observable Plot, Plotly.js
- Statistical Process Control (SPC) charts
- Interactive dashboards
- Puppeteer for PDF generation

### Phase 5: Naming & Brand Identity
**Research Journey:**
- Initial suggestion: DASH
- Discovery: Major conflicts (Edge Health DASH, DASH risk assessment tool)
- Exploration: COMPASS, BEACON, ATLAS, PANORAMA, SPECTRUM, VISTA
- **Final Selection: ATLAS**

**Why ATLAS:**
- No conflicts in UK healthcare analytics space
- Perfect metaphor for comprehensive mapping/reference
- Suggests navigation, authority, completeness
- Not NHS-specific (covers full health & care system)
- Memorable, trustworthy, modern

### Phase 6: Brand Development
**Brand Identity Created:**
- Color palette: Deep Teal (#0A5C5F) primary, Amber (#D97E3F) accent
- Typography: Inter for UI, IBM Plex Mono for data/numbers
- Logo: Geometric layered planes (cartographic/data integration metaphor)
- Personality: "Modern Guide" - professional but approachable
- Tagline: "Navigate the complete picture"

**Deliverables:**
- Complete brand guidelines
- 4 logo concepts + production assets
- Interactive color/UI demos
- Implementation guides

---

## Project Scope Summary

**Data Coverage:**
- 28+ distinct health and care data domains
- National to site-level organizational reporting
- Cross-sector integration (health, care, emergency, public health)

**User Personas:**
- Clinical leaders and operational teams
- Trust executives and board members
- ICB commissioners and system leaders
- Analysts and performance teams
- National policy makers

**Technical Stack:**
- Frontend: Next.js / SvelteKit / Remix
- Database: SQLite (dev) / Cloudflare D1 (prod)
- Visualization: Chart.js, Apache ECharts, Observable Plot
- PDF Export: Puppeteer
- Hosting: Cloudflare Pages/Workers
- Data Pipeline: Automated scheduled ingestion

---

## Documentation Structure

This `prompt-prep` folder contains comprehensive documentation for building ATLAS:

1. **00-CONVERSATION-OVERVIEW.md** (this file) - Project journey and context
2. **01-DATA-SOURCES.md** - Complete catalog of all NHS and care datasets
3. **02-DATABASE-SCHEMA.md** - Detailed database design and relationships
4. **03-TECHNICAL-ARCHITECTURE.md** - Infrastructure, tech stack, deployment
5. **04-PLATFORM-FUNCTIONALITY.md** - Features, user stories, requirements
6. **05-ORGANIZATIONAL-HIERARCHY.md** - NHS structure, ODS, succession tracking
7. **06-IMPLEMENTATION-PROMPT.md** - Comprehensive build prompt for developers/AI
8. **07-NAMING-RESEARCH.md** - Naming journey and competitive analysis
9. **08-VISUALIZATION-REQUIREMENTS.md** - Chart libraries, SPC, interactivity

---

## Key Insights & Decisions

### Critical Success Factors

1. **Data Time Alignment**
   - Different datasets publish on different schedules
   - Must track availability and prevent mismatched time period comparisons
   - Solution: `data_availability` table tracking each source's publication calendar

2. **Organizational Complexity**
   - Trusts merge, split, and reorganize
   - Historical data must link to current structures
   - Solution: Succession chain tracking with predecessor/successor relationships

3. **Novel Categorization**
   - Traditional NHS reporting groups data by service type
   - ATLAS introduces insight-driven categories:
     - Patient journey stages
     - System pressure points
     - Population health determinants
     - Cross-agency collaboration
     - Quality & safety themes
     - Productivity & efficiency

4. **User Customization**
   - Every user has different reporting needs
   - Must support custom views, saved filters, folder organization
   - Report publishing for sharing across teams

5. **Free Hosting Constraint**
   - SQLite/Cloudflare D1 enables zero-cost data storage
   - Cloudflare Workers handle compute
   - Automated data pipelines run on schedule
   - Challenge: Working within D1 size limits for national datasets

### Technical Challenges Identified

1. **Data Volume**
   - HES alone is millions of records annually
   - Solution: Aggregate/summary tables, efficient indexing, optional detail drilldown

2. **Data Quality**
   - Publication delays, corrections, revisions
   - Solution: Version tracking, data quality flags, publication metadata

3. **Performance**
   - SQLite query optimization for large datasets
   - Solution: Materialized views, pre-aggregation, smart indexing

4. **Multi-source Integration**
   - Different schemas, coding standards, granularity
   - Solution: Unified data model with source-specific mapping tables

---

## Next Steps to Build ATLAS

1. **Review Complete Documentation**
   - Read through all markdown files in `prompt-prep/`
   - Understand data sources, schema, and architecture

2. **Set Up Development Environment**
   - Docker with SQLite for local prototyping
   - Cloudflare account for production deployment
   - Choose frontend framework (Next.js recommended)

3. **Implement Core Schema**
   - Start with organizational hierarchy (ODS integration)
   - Build data source catalog and availability tracking
   - Create initial data loading pipelines

4. **Build MVP Dashboard**
   - Single data source (e.g., RTT waiting times)
   - Basic filtering (by trust, region, time period)
   - Simple visualization (line chart, table)

5. **Iterate and Expand**
   - Add additional data sources incrementally
   - Build out custom view saving
   - Implement report generation
   - Deploy to Cloudflare

---

## Repository Contents

### Brand Identity (root)
- Complete ATLAS brand guidelines
- Logo assets (SVG)
- Interactive demos (HTML)
- Implementation guides

### Platform Preparation (`prompt-prep/`)
- Comprehensive technical documentation
- Data source catalogs
- Database schemas
- Architecture specifications
- Implementation prompts

---

*This documentation represents the complete planning output for building ATLAS - the national health and care intelligence platform.*

**Ready to build.**
