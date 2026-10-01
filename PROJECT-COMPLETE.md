# ATLAS Project - Complete Documentation Package

**Repository:** https://github.com/tomnortonuk/atlas-prep

**Status:** ✅ **COMPLETE AND READY TO BUILD**

---

## What's Been Delivered

### 🎨 Brand Identity (Root Directory)

**Complete ATLAS brand package:**
- Brand guidelines (colors, typography, voice)
- 4 logo concepts + production assets (SVG)
- Interactive demos (color swatches, UI components)
- Implementation guides for designers and developers
- Favicon assets

**Files:**
- `ATLAS-Brand-Guidelines.md` - Complete brand bible
- `ATLAS-BRANDING-README.md` - Implementation guide
- `ATLAS-Brand-Summary.html` - **Open this first!** One-page visual overview
- `atlas-logo-*.svg` - Logo variations
- `atlas-color-swatches.html` - Interactive color reference
- `atlas-ui-components-demo.html` - Live UI showcase

---

### 📚 Platform Preparation (`prompt-prep/` folder)

**Complete technical documentation for building ATLAS:**

#### 00-CONVERSATION-OVERVIEW.md
- Project journey from inception to brand selection
- Evolution of requirements
- Key decisions and rationale
- Phase-by-phase summary

#### 01-DATA-SOURCES.md (28+ Datasets)
- **Acute Care:** HES, SUS, CDS
- **Emergency:** ECDS, MSitAE, Ambulance
- **Mental Health:** MHSDS, IAPT
- **Maternity:** MSDS
- **Community:** CSDS
- **Social Care:** ASC-FR, CLD, CQC
- **Specialist:** RTT, WLMDS, Cancer
- **Mortality:** SHMI
- **Palliative:** NACEL, Hospice data
- **CHC:** Continuing Healthcare
- **Substance Misuse:** NDTMS
- **Criminal Justice:** S136, Prison health
- **Fire:** Safe & Well visits
- **Safety:** LFPSE (replaced NRLS)
- **HCAI:** UKHSA infection surveillance
- **Audits:** NCAPOP clinical audits
- **Operational:** NOF, diagnostics, cancer waits
- **Workforce:** ESR, Staff Survey
- **Finance:** Model Hospital, GIRFT, PLICS
- **Primary Care:** GP Patient Survey, QOF
- **Population Health:** OHID Fingertips

*Each with details on: source, granularity, update frequency, access methods*

#### 02-DATABASE-SCHEMA.md
- Complete SQLite schema for all platform tables
- Data source management
- Organizational hierarchy (with ODS integration)
- Geographic mapping (LSOA → ICB → Region)
- Insight categorization (novel multi-dimensional)
- User customization (saved views, folders, reports)
- Example data tables (RTT, A&E, workforce, SHMI)
- Recursive queries for hierarchies
- Performance optimization strategies

#### 03-TECHNICAL-ARCHITECTURE.md
- Architecture overview and system components
- Local development (Docker + SQLite)
- Production deployment (Cloudflare Pages + Workers + D1)
- Tech stack recommendations:
  - Frontend: Next.js 14+ with React
  - UI: shadcn/ui + Tailwind CSS
  - Database: SQLite local, Cloudflare D1 production
  - ORM: Drizzle (TypeScript-first)
  - Viz: Chart.js, Apache ECharts, Observable Plot, D3
  - Auth: Clerk (recommended)
  - PDF: Puppeteer via external service
- Data pipeline architecture
- API design
- Performance optimization
- Security considerations
- Deployment process
- Cost estimation (free tier targets)

#### 04-PLATFORM-FUNCTIONALITY.md
- **User personas:** 5 detailed personas with needs and use cases
- **Core features:**
  - Data Explorer with multi-source selection
  - Organizational Hierarchy Navigator
  - Novel Insight Categories (patient journey, system pressure, etc.)
  - Visualization Suite (8+ chart types)
  - Custom View Builder
  - Report Generation (quick templates + custom builder)
  - User Workspace Management
  - Data Export & Integration
  - Alerts & Notifications
  - Data Quality Metadata
- **User journey examples** with time estimates
- **Future enhancements** roadmap

#### 05-ORGANIZATIONAL-HIERARCHY.md
- NHS England structure (National → Regional → ICB → Trust → Site)
- 36 ICBs (post-April 2026 reorganization) - complete list
- 7 NHS Regions
- Sub-ICB locations (place-based partnerships)
- Provider organizations (acute, mental health, community, ambulance)
- Trust mergers and succession tracking
- Primary care (GP practices, PCNs)
- Social care (local authorities, CQC-regulated providers)
- ODS API integration guide
- Geographic hierarchies (LSOA → MSOA → LA → ICB → Region)
- Multi-level reporting scenarios with SQL examples

#### 06-IMPLEMENTATION-PROMPT.md ⭐ **MOST IMPORTANT**
- **Complete build specification for developers or AI assistants**
- Project brief and technical requirements
- Recommended tech stack with justification
- Project structure (complete directory layout)
- **6-phase implementation plan:**
  - Phase 1: Foundation (MVP - Week 1-2)
  - Phase 2: Multi-Source & Hierarchy (Week 3-4)
  - Phase 3: Custom Views & Visualization (Week 5-6)
  - Phase 4: Reports & Export (Week 7-8)
  - Phase 5: Cloudflare Deployment (Week 9-10)
  - Phase 6: Advanced Features (Week 11-12)
- Detailed implementation guide with code examples:
  - Database setup (Drizzle schema)
  - Data loading scripts
  - API routes
  - Frontend components
- Testing strategy
- Success criteria for each phase
- Common pitfalls and solutions
- Monitoring and maintenance plan

#### 07-NAMING-RESEARCH.md
- Complete naming journey (DASH → ATLAS)
- Why DASH was initially appealing
- **Major conflicts discovered:**
  - Edge Health DASH (direct competitor - £55k-110k UK NHS analytics)
  - DASH Risk Assessment (NHS safeguarding tool - very high recognition)
  - UK Gov DaSH Portal (medicine shortages)
  - Plotly Dash (analytics framework)
  - 5+ other healthcare DASH acronyms
- Alternative names investigated (15+ options)
- Why ATLAS was selected (zero conflicts, perfect metaphor)
- Competitive landscape analysis
- Brand guidelines reference
- Lessons learned for future naming

#### 08-VISUALIZATION-REQUIREMENTS.md
- Visualization principles (clarity, accessibility, brand, performance)
- **Chart library selection:**
  - Chart.js for general charts (line, bar, pie)
  - Apache ECharts for advanced (heatmaps, Sankey, maps)
  - Observable Plot for statistical graphics
  - Custom D3.js for SPC charts
- **Standard chart types:**
  - Line charts (time trends)
  - Bar charts (comparisons, benchmarking)
  - Tables (with sparklines, conditional formatting)
  - Heatmaps (geographic, time-series)
  - Maps (choropleth by ICB/LA)
  - Sankey diagrams (flow visualization)
- **Statistical Process Control (SPC) charts:**
  - XmR chart implementation
  - NHS-R "Making Data Count" standards
  - Special cause detection (5 rules)
  - Complete D3.js code example
- Interactive features (tooltips, drill-down, cross-filtering)
- Export capabilities (image, data, PDF)
- Accessibility requirements (WCAG AA, keyboard, screen readers)
- Performance targets
- Complete ATLAS brand styling guide for charts

---

## How to Use This Repository

### If You're Building the Platform

1. **Start here:** `prompt-prep/06-IMPLEMENTATION-PROMPT.md`
   - This is your complete build specification
   - Copy this entire prompt into an AI coding assistant (Claude, GPT, etc.)
   - Or use as specification document for development team

2. **Reference these as needed:**
   - `01-DATA-SOURCES.md` - When adding data sources
   - `02-DATABASE-SCHEMA.md` - For database structure
   - `03-TECHNICAL-ARCHITECTURE.md` - For deployment decisions
   - `04-PLATFORM-FUNCTIONALITY.md` - For feature requirements
   - `05-ORGANIZATIONAL-HIERARCHY.md` - For NHS structure understanding
   - `08-VISUALIZATION-REQUIREMENTS.md` - For chart implementation

3. **Apply the brand:**
   - Open `ATLAS-Brand-Summary.html` in browser
   - Review `ATLAS-Brand-Guidelines.md`
   - Use logo files from root directory
   - Follow color/typography specs

### If You're Designing/Branding

1. **Visual overview:** Open `ATLAS-Brand-Summary.html`
2. **Interactive demos:** Open `atlas-ui-components-demo.html`
3. **Complete specs:** Read `ATLAS-Brand-Guidelines.md`
4. **Implementation:** Follow `ATLAS-BRANDING-README.md`
5. **Logo assets:** Use SVG files in root directory

---

## What You Can Build Now

With this documentation, you have everything needed to build:

✅ **MVP (4-8 weeks)**
- Working ATLAS platform with single data source (RTT)
- Organization selector and time period filtering
- Basic charts and tables
- Docker-based local deployment

✅ **Production Platform (12 weeks)**
- Multiple data sources integrated
- Organizational hierarchy navigation
- Custom view builder
- Report generation and export
- Deployed to Cloudflare (free tier)
- Automated data updates

✅ **Full Feature Platform (ongoing)**
- All 28+ data sources
- Novel insight categorization
- Dashboard builder
- Alerts and notifications
- Performance optimized
- Complete documentation

---

## Repository Statistics

**Total Documentation:**
- 9 comprehensive markdown files
- 8 brand/design assets
- ~50,000 words of technical specification
- Complete code examples and schemas
- Ready-to-use implementation prompts

**Coverage:**
- 28+ data sources cataloged
- 36 ICBs documented
- 6-phase implementation plan
- 5 user personas
- 8+ chart types specified
- Complete tech stack recommendations

---

## Next Steps

1. **Review the documentation** (start with `00-CONVERSATION-OVERVIEW.md`)
2. **Choose your path:**
   - Building: Use `06-IMPLEMENTATION-PROMPT.md`
   - Designing: Use brand assets and guidelines
3. **Set up development environment** (Docker + Node.js)
4. **Begin Phase 1 implementation** (Foundation/MVP)
5. **Iterate based on user feedback**

---

## Questions or Issues?

All documentation is self-contained in this repository. For:
- **Technical questions:** Reference `03-TECHNICAL-ARCHITECTURE.md`
- **Feature questions:** Reference `04-PLATFORM-FUNCTIONALITY.md`
- **Data questions:** Reference `01-DATA-SOURCES.md`
- **Brand questions:** Reference `ATLAS-Brand-Guidelines.md`
- **Build questions:** Reference `06-IMPLEMENTATION-PROMPT.md`

---

## License & Usage

This repository contains:
- **Brand assets:** ATLAS brand identity (proprietary to project)
- **Technical specifications:** Documentation for building the platform
- **Research outputs:** Naming research, competitive analysis

Use these materials to build the ATLAS platform as specified.

---

## Acknowledgments

**Data Sources:** NHS England, NHS Digital, UKHSA, CQC, OHID, ONS

**Frameworks:** Next.js, Cloudflare, Drizzle ORM, Chart.js, Apache ECharts, Observable Plot, D3.js

**Standards:** NHS-R Community (Making Data Count), NHS England Open Data

---

**Repository URL:** https://github.com/tomnortonuk/atlas-prep

**Status:** ✅ Complete - Ready to Build

**Last Updated:** October 1, 2026

---

*ATLAS - Navigate the complete picture*
