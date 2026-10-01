# ATLAS - Population Health and Care Intelligence Platform

**Navigate the complete picture**

ATLAS is a national health and care data intelligence platform bringing together insights from across acute, community, mental health, social care, emergency services, and public health into one comprehensive, navigable system.

---

## 📦 Repository Contents

This repository contains the **complete planning, research, brand identity, and implementation specifications** for building the ATLAS platform.

### Brand Assets

- **ATLAS-Brand-Guidelines.md** - Complete brand guidelines (colors, typography, voice)
- **ATLAS-BRANDING-README.md** - Implementation guide for designers and developers
- **ATLAS-Brand-Summary.html** - One-page visual brand overview (open in browser)

### Logo Files

- `atlas-logo-concept-1.svg` - Interconnected Hexagonal Planes
- `atlas-logo-concept-2.svg` - Grid Network with Central Node
- `atlas-logo-concept-3.svg` - Layered Cartographic Planes ⭐ (Recommended)
- `atlas-logo-concept-4.svg` - Geometric "A" with Data Flow
- `atlas-logo-horizontal.svg` - Production horizontal logo (icon + wordmark)
- `atlas-favicon-32.svg` - Favicon/icon mark

### Interactive Demos

- **atlas-color-swatches.html** - Interactive color palette reference
- **atlas-ui-components-demo.html** - Full UI component showcase

### Platform Preparation (`prompt-prep/`)

Complete technical documentation for building ATLAS:

1. **00-CONVERSATION-OVERVIEW.md** - Project journey and decision history
2. **01-DATA-SOURCES.md** - Comprehensive catalog of 28+ NHS and care datasets
3. **02-DATABASE-SCHEMA.md** - Complete SQLite database design with examples
4. **03-TECHNICAL-ARCHITECTURE.md** - Infrastructure, tech stack, and deployment
5. **04-PLATFORM-FUNCTIONALITY.md** - Feature specifications and user requirements
6. **05-ORGANIZATIONAL-HIERARCHY.md** - NHS structure, ODS, succession tracking
7. **06-IMPLEMENTATION-PROMPT.md** - **Complete build prompt for developers/AI**
8. **07-NAMING-RESEARCH.md** - Naming journey and competitive analysis
9. **08-VISUALIZATION-REQUIREMENTS.md** - Chart specifications and SPC standards

---

## 🎨 Quick Brand Reference

### Colors
```
Primary:   #0A5C5F (Deep Teal)
Accent:    #D97E3F (Amber)
Text:      #2C3E42 (Charcoal)
```

### Typography
```
Headings:  Inter Bold/SemiBold
Body:      Inter Regular
Data:      IBM Plex Mono
```

### Tagline
"Navigate the complete picture"

---

## 🚀 Getting Started

### For Building the Platform

1. **Read the conversation overview:**
   ```bash
   open prompt-prep/00-CONVERSATION-OVERVIEW.md
   ```

2. **Review the implementation prompt:**
   ```bash
   open prompt-prep/06-IMPLEMENTATION-PROMPT.md
   ```
   This is your complete guide to building ATLAS from scratch.

3. **Understand the data sources:**
   ```bash
   open prompt-prep/01-DATA-SOURCES.md
   ```
   28+ datasets cataloged with details on access, granularity, and update frequency.

4. **Study the database schema:**
   ```bash
   open prompt-prep/02-DATABASE-SCHEMA.md
   ```
   Complete SQLite schema with examples and recursive queries.

5. **Review the technical architecture:**
   ```bash
   open prompt-prep/03-TECHNICAL-ARCHITECTURE.md
   ```
   Cloudflare deployment, tech stack, and performance optimization.

### For Brand Implementation

1. **View the brand visually:**
   ```bash
   open ATLAS-Brand-Summary.html
   open atlas-ui-components-demo.html
   ```

2. **Read the guidelines:**
   - Start with `ATLAS-BRANDING-README.md` for overview
   - Dive into `ATLAS-Brand-Guidelines.md` for complete specs

3. **Use the logo files:**
   - All SVG files can be opened in Figma, Illustrator, Sketch
   - Recommended: Use Concept 3 (Layered Cartographic Planes)
   - Horizontal logo ready for web/app implementation

---

## 💡 Brand Personality

**Modern Guide** - Professional but approachable, confident, clear

### Core Values
- 🗺️ **Comprehensive** - Every sector, every insight, one view
- ✓ **Trusted** - Objective, evidence-based, authoritative
- 💡 **Clear** - Complex data made navigable and actionable
- → **Modern** - Forward-thinking, accessible, user-focused

---

## 📋 Platform Scope

ATLAS integrates data from:

- **Acute Care**: Hospital Episode Statistics (HES), critical care
- **Mental Health**: MHSDS, IAPT/NHS Talking Therapies
- **Maternity**: Maternity Services Dataset (MSDS)
- **Emergency**: Emergency Care Dataset (ECDS), ambulance services
- **Community**: Community Services Dataset (CSDS)
- **Social Care**: Adult Social Care, Care Quality (CQC)
- **Workforce**: ESR statistics, bank and agency data
- **Quality & Safety**: LFPSE, HCAI surveillance, clinical audits
- **Operational**: RTT, waiting lists, Model Health System

Multi-level reporting:
- National → Regional → ICB → Sub-ICB → Trust → Site level
- Provider systems, local authority integration
- Cross-sector insights and population health analytics

---

## 🛠️ Technical Stack (Planned)

- **Database**: SQLite (local Docker) / Cloudflare D1 (production)
- **Frontend**: Next.js / SvelteKit / Remix
- **Visualization**: Chart.js, Apache ECharts, Observable Plot
- **Deployment**: Cloudflare Pages/Workers
- **Data Ingestion**: Automated scheduled updates from national datasets

---

## 📄 License

Brand assets and documentation are proprietary. 

---

## 🤝 Contributing

This is the brand identity repository. For platform development, see the main ATLAS application repository.

---

## 📞 Contact

For brand usage questions or custom requests, refer to the guidelines in `ATLAS-Brand-Guidelines.md`.

---

*ATLAS - Navigate the complete picture*
*Population Health and Care Intelligence*
