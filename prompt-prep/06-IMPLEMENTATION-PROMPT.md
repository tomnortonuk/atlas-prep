# ATLAS Implementation Prompt

**Complete prompt for building the ATLAS platform from scratch.**

Use this prompt with an AI coding assistant or as a specification for a development team.

---

## Project Brief

Build **ATLAS** - a national health and care data intelligence platform that enables any hospital, trust, ICB, or organization to view its own data, explore peer data, and develop insights into national datasets.

**Core Value Proposition:**
Navigate the complete picture of population health and care across England, from national trends to individual organizational performance, with innovative multi-dimensional insights beyond traditional NHS reporting.

---

## Technical Requirements

### Hard Constraints

1. **Local Development:** Docker with SQLite database
2. **Production Hosting:** Cloudflare Pages + Workers + D1 (SQLite at edge)
3. **Zero-Cost Goal:** Stay within Cloudflare free tiers
4. **Data Storage:** SQLite/D1 only (no PostgreSQL, MySQL, etc.)
5. **Automated Data Loading:** Scheduled checks with timed imports when data becomes available

### Recommended Tech Stack

**Frontend:**
- Framework: Next.js 14+ (App Router) with React 18+
- UI Components: shadcn/ui + Radix UI + Tailwind CSS
- Styling: Tailwind CSS matching ATLAS brand guidelines
- State Management: React Context / Zustand for global state
- Forms: React Hook Form + Zod validation

**Database & ORM:**
- Local: SQLite via better-sqlite3
- Production: Cloudflare D1 (SQLite at edge)
- ORM: Drizzle ORM (TypeScript-first, excellent D1 support)
- Migrations: Drizzle Kit

**Visualization:**
- Chart.js (general charts - line, bar, pie)
- Apache ECharts (advanced - heatmaps, Sankey, maps)
- Observable Plot (statistical graphics)
- Custom D3.js for SPC (Statistical Process Control) charts

**Backend / API:**
- Next.js API routes (local development)
- Cloudflare Workers (production edge functions)
- Cloudflare Workers Cron for scheduled data ingestion

**Authentication:**
- Clerk (recommended - easy Cloudflare integration, free tier 10K MAU)
- Alternative: Auth.js (NextAuth) with Cloudflare adapter

**PDF Generation:**
- Puppeteer via external service (browserless.io or self-hosted Gotenberg)
- Store PDFs in Cloudflare R2 object storage

### Project Structure

```
atlas/
├── src/
│   ├── app/                # Next.js App Router pages
│   │   ├── (auth)/         # Auth-protected routes
│   │   ├── api/            # API routes
│   │   ├── dashboard/      # Main dashboard
│   │   ├── explore/        # Data explorer
│   │   ├── reports/        # Report builder
│   │   └── layout.tsx
│   ├── components/         # React components
│   │   ├── ui/             # shadcn/ui components
│   │   ├── charts/         # Chart components
│   │   ├── filters/        # Filter components
│   │   └── layout/         # Layout components
│   ├── db/                 # Database
│   │   ├── schema.ts       # Drizzle schema definitions
│   │   ├── migrations/     # SQL migrations
│   │   └── client.ts       # DB connection
│   ├── lib/                # Utilities
│   │   ├── data-loader/    # Data ingestion scripts
│   │   ├── utils.ts        # Helper functions
│   │   └── constants.ts    # App constants
│   └── styles/             # Global styles
├── workers/                # Cloudflare Workers
│   ├── data-ingestion/     # Scheduled data loaders
│   └── api/                # Edge API handlers
├── public/                 # Static assets
│   └── brand/              # Logo, favicon, etc.
├── scripts/                # Build & deployment scripts
│   ├── seed-reference.ts   # Seed ODS data
│   └── migrate.ts          # Run migrations
├── docker-compose.yml      # Local development
├── Dockerfile              # App container
├── wrangler.toml           # Cloudflare config
├── drizzle.config.ts       # Drizzle ORM config
├── tailwind.config.ts      # Tailwind configuration
├── next.config.js          # Next.js config
└── package.json
```

---

## Implementation Phases

### Phase 1: Foundation (MVP - Week 1-2)

**Goal:** Basic platform with single data source (RTT waiting lists)

**Tasks:**

1. **Setup & Infrastructure**
   ```bash
   npx create-next-app@latest atlas --typescript --tailwind --app
   cd atlas
   npm install drizzle-orm better-sqlite3
   npm install -D drizzle-kit
   npm install @radix-ui/react-* tailwindcss-animate class-variance-authority
   ```

2. **Database Schema**
   - Implement core tables from `02-DATABASE-SCHEMA.md`:
     - `data_sources`
     - `data_availability`
     - `organizations` (load from ODS API)
     - `icb_hierarchy`
     - `rtt_waiting_list` (initial data table)
   - Create Drizzle schema definitions
   - Generate migrations
   - Seed with ODS organizational data

3. **Brand Implementation**
   - Copy ATLAS brand colors to Tailwind config
   - Implement logo in header
   - Apply Inter font for UI, IBM Plex Mono for data
   - Create reusable branded components (buttons, cards)

4. **Authentication**
   - Set up Clerk or Auth.js
   - Protect dashboard routes
   - Basic user profile

5. **Data Loading**
   - Script to download RTT CSV from NHS England
   - Parse and transform to database schema
   - Insert into SQLite
   - Track in `data_availability`

6. **Basic UI**
   - Header with logo and navigation
   - Organization selector (autocomplete search)
   - Time period selector
   - Simple data table showing RTT waiting lists
   - Basic line chart (Chart.js)

7. **Docker Setup**
   ```yaml
   # docker-compose.yml
   services:
     atlas:
       build: .
       ports:
         - "3000:3000"
       volumes:
         - ./src:/app/src
         - ./data:/app/data
       environment:
         - DATABASE_URL=file:./data/atlas.db
   ```

**MVP Deliverable:** 
- Working local application
- View RTT waiting list data for any trust
- Filter by time period
- Simple chart and table
- Docker-based deployment

---

### Phase 2: Multi-Source & Hierarchy (Week 3-4)

**Goal:** Add multiple data sources and organizational hierarchy

**Tasks:**

1. **Additional Data Sources**
   - A&E performance (ECDS / MSitAE)
   - Workforce (ESR)
   - Create data tables for each
   - Data loading scripts

2. **Organizational Hierarchy Navigator**
   - Tree component for org hierarchy
   - Drill-down: National → Region → ICB → Trust → Site
   - Aggregate up functionality
   - Recursive SQL queries for hierarchy

3. **Multi-Source Selection**
   - Checkbox selector for data sources
   - Dynamic table/chart based on selection
   - Handle different schemas gracefully

4. **Geographic Filtering**
   - Load ONS geographic lookups
   - ICB/Region/LA filters
   - Map visualization (optional)

5. **Data Synchronization**
   - Scheduled check for new data (manual trigger for now)
   - Update `data_availability` tracker
   - Notification when new data loaded

**Phase 2 Deliverable:**
- Multiple data sources integrated
- Hierarchical organization navigation
- Filter by geography
- Drill-down and aggregate-up capabilities

---

### Phase 3: Custom Views & Visualization (Week 5-6)

**Goal:** User customization and advanced charts

**Tasks:**

1. **Insight Categories**
   - Implement category taxonomy from `04-PLATFORM-FUNCTIONALITY.md`
   - Tag data sources with categories
   - Category-based navigation

2. **Chart Library Integration**
   - Chart.js for standard charts
   - Apache ECharts for heatmaps, Sankey
   - Observable Plot for statistical charts
   - Basic SPC chart (XmR) with D3.js

3. **Custom View Builder**
   - Multi-step wizard:
     1. Select data sources
     2. Choose aggregation level
     3. Apply filters
     4. Select metrics
     5. Choose visualization
     6. Save view
   - Save view configuration to database

4. **Saved Views Management**
   - List user's saved views
   - Folder organization
   - Edit/delete/duplicate views
   - Quick load saved view

5. **Dashboard Builder**
   - Drag-and-drop layout
   - Add saved views as widgets
   - Resize and arrange
   - Save dashboard configuration

**Phase 3 Deliverable:**
- Insight category navigation
- Multiple chart types
- User-created custom views
- Personal dashboards

---

### Phase 4: Reports & Export (Week 7-8)

**Goal:** Report generation and sharing

**Tasks:**

1. **Quick Report Templates**
   - Trust Performance Pack
   - ICB System Report
   - National Overview
   - Pre-configured views

2. **Custom Report Builder**
   - Select multiple saved views
   - Add commentary text boxes
   - Configure layout
   - Cover page with branding

3. **PDF Generation**
   - Integrate Puppeteer (via external service)
   - Render report HTML
   - Generate PDF with charts as images
   - Apply ATLAS branding
   - Store in Cloudflare R2

4. **Export Capabilities**
   - CSV export (current view)
   - Excel export (multiple sheets)
   - JSON export (API format)
   - Chart images (PNG/SVG)

5. **Report Publishing**
   - Share report with link
   - Access control (private, org, public)
   - Email distribution
   - Report library

**Phase 4 Deliverable:**
- PDF report generation
- Multiple export formats
- Report sharing and access control

---

### Phase 5: Cloudflare Deployment (Week 9-10)

**Goal:** Production deployment to Cloudflare

**Tasks:**

1. **Cloudflare D1 Setup**
   ```bash
   npx wrangler d1 create atlas-db
   npx wrangler d1 migrations apply atlas-db --remote
   ```

2. **Migrate Database**
   - Export local SQLite
   - Import to D1
   - Test queries on edge

3. **Cloudflare Workers**
   - API Worker for data queries
   - Scheduled Workers for data ingestion
   - Configure Cron triggers
   - Test edge execution

4. **Cloudflare Pages**
   - Deploy Next.js to Pages
   - Configure Pages Functions (Workers integration)
   - Custom domain setup
   - SSL certificate

5. **Environment Configuration**
   ```toml
   # wrangler.toml
   name = "atlas"
   compatibility_date = "2024-10-01"

   [[d1_databases]]
   binding = "DB"
   database_name = "atlas-db"
   database_id = "<db_id>"

   [[r2_buckets]]
   binding = "REPORTS"
   bucket_name = "atlas-reports"

   [triggers]
   crons = ["0 9 * * 1"]  # Monday 9am
   ```

6. **CI/CD Pipeline**
   - GitHub Actions workflow
   - Auto-deploy on push to main
   - Run tests before deploy
   - Database migration on deploy

**Phase 5 Deliverable:**
- Fully deployed to Cloudflare
- Automated data ingestion
- Edge-optimized performance
- CI/CD pipeline

---

### Phase 6: Advanced Features (Week 11-12)

**Goal:** Polish and enhancements

**Tasks:**

1. **Alerts & Notifications**
   - Custom alert builder
   - Threshold monitoring
   - Email notifications
   - In-app notifications

2. **Data Quality Dashboard**
   - Freshness indicators
   - Missing data alerts
   - Publication lag tracker
   - Data dictionary

3. **Performance Optimization**
   - Aggregate tables for common queries
   - Caching strategy (Workers KV)
   - Lazy loading for large datasets
   - Virtual scrolling for tables

4. **Mobile Responsiveness**
   - Responsive layouts
   - Touch-optimized interactions
   - Mobile-specific views

5. **Help & Documentation**
   - In-app tooltips
   - Contextual help
   - Video tutorials
   - FAQ page

**Phase 6 Deliverable:**
- Polished production-ready platform
- Alerts and monitoring
- Optimized performance
- Complete documentation

---

## Detailed Implementation Guide

### Database Setup

**1. Initialize Drizzle**

```typescript
// drizzle.config.ts
import type { Config } from 'drizzle-kit';

export default {
  schema: './src/db/schema.ts',
  out: './src/db/migrations',
  driver: 'better-sqlite3',
  dbCredentials: {
    url: process.env.DATABASE_URL || './data/atlas.db',
  },
} satisfies Config;
```

**2. Define Schema**

```typescript
// src/db/schema.ts
import { sqliteTable, text, integer, real } from 'drizzle-orm/sqlite-core';

export const organizations = sqliteTable('organizations', {
  odsCode: text('ods_code').primaryKey(),
  organizationName: text('organization_name').notNull(),
  organizationType: text('organization_type').notNull(),
  status: text('status').default('Active'),
  icbCode: text('icb_code'),
  regionCode: text('region_code'),
  // ... more fields from schema.md
});

export const rttWaitingList = sqliteTable('rtt_waiting_list', {
  recordId: integer('record_id').primaryKey({ autoIncrement: true }),
  reportingPeriod: text('reporting_period').notNull(),
  providerOdsCode: text('provider_ods_code').notNull(),
  specialtyCode: text('specialty_code').notNull(),
  weeksWaitingBand: text('weeks_waiting_band'),
  patientCount: integer('patient_count').notNull(),
});

// ... more tables
```

**3. Create Database Client**

```typescript
// src/db/client.ts
import { drizzle } from 'drizzle-orm/better-sqlite3';
import Database from 'better-sqlite3';
import * as schema from './schema';

const sqlite = new Database(process.env.DATABASE_URL || './data/atlas.db');
export const db = drizzle(sqlite, { schema });
```

**4. Run Migrations**

```bash
npx drizzle-kit generate:sqlite
npx drizzle-kit push:sqlite
```

---

### Data Loading Scripts

**Example: RTT Data Loader**

```typescript
// src/lib/data-loader/rtt-loader.ts
import { db } from '@/db/client';
import { rttWaitingList, dataAvailability } from '@/db/schema';
import { parse } from 'csv-parse/sync';

export async function loadRTTData(period: string) {
  // 1. Check if already loaded
  const existing = await db
    .select()
    .from(dataAvailability)
    .where(eq(dataAvailability.sourceId, 'RTT'))
    .where(eq(dataAvailability.reportingPeriodStart, period))
    .get();

  if (existing?.loadStatus === 'complete') {
    console.log('RTT data already loaded for', period);
    return;
  }

  // 2. Download CSV from NHS England
  const url = `https://www.england.nhs.uk/statistics/wp-content/uploads/sites/2/...`;
  const response = await fetch(url);
  const csvText = await response.text();

  // 3. Parse CSV
  const records = parse(csvText, {
    columns: true,
    skip_empty_lines: true,
  });

  // 4. Transform and insert
  const transformed = records.map((row: any) => ({
    reportingPeriod: period,
    providerOdsCode: row['Provider Code'],
    specialtyCode: row['Specialty Code'],
    weeksWaitingBand: row['Weeks Waiting'],
    patientCount: parseInt(row['Patients']),
  }));

  await db.insert(rttWaitingList).values(transformed);

  // 5. Update availability tracker
  await db.insert(dataAvailability).values({
    sourceId: 'RTT',
    reportingPeriodStart: period,
    reportingPeriodEnd: period,
    publicationDate: new Date().toISOString().split('T')[0],
    loadedDate: new Date().toISOString().split('T')[0],
    loadStatus: 'complete',
    recordCount: transformed.length,
  });

  console.log(`Loaded ${transformed.length} RTT records for ${period}`);
}
```

---

### API Routes

**Example: Query RTT Data**

```typescript
// src/app/api/data/rtt/route.ts
import { NextRequest, NextResponse } from 'next/server';
import { db } from '@/db/client';
import { rttWaitingList, organizations } from '@/db/schema';
import { eq, and } from 'drizzle-orm';

export async function GET(request: NextRequest) {
  const searchParams = request.nextUrl.searchParams;
  const period = searchParams.get('period');
  const orgCode = searchParams.get('org');

  if (!period) {
    return NextResponse.json({ error: 'Period required' }, { status: 400 });
  }

  const query = db
    .select({
      providerOds: rttWaitingList.providerOdsCode,
      providerName: organizations.organizationName,
      specialty: rttWaitingList.specialtyCode,
      weeksWaiting: rttWaitingList.weeksWaitingBand,
      patientCount: rttWaitingList.patientCount,
    })
    .from(rttWaitingList)
    .leftJoin(
      organizations,
      eq(rttWaitingList.providerOdsCode, organizations.odsCode)
    )
    .where(eq(rttWaitingList.reportingPeriod, period));

  if (orgCode) {
    query.where(eq(rttWaitingList.providerOdsCode, orgCode));
  }

  const results = await query.all();

  return NextResponse.json({
    meta: {
      source: 'RTT',
      period,
      recordCount: results.length,
    },
    data: results,
  });
}
```

---

### Frontend Components

**Example: Chart Component**

```typescript
// src/components/charts/LineChart.tsx
'use client';

import { Line } from 'react-chartjs-2';
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend,
} from 'chart.js';

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
);

interface LineChartProps {
  data: {
    labels: string[];
    datasets: {
      label: string;
      data: number[];
      borderColor?: string;
      backgroundColor?: string;
    }[];
  };
  title: string;
}

export function LineChart({ data, title }: LineChartProps) {
  const options = {
    responsive: true,
    plugins: {
      legend: {
        position: 'bottom' as const,
      },
      title: {
        display: true,
        text: title,
        font: {
          family: 'Inter',
          size: 18,
          weight: '600',
        },
        color: '#2C3E42', // ATLAS Charcoal
      },
    },
    scales: {
      y: {
        beginAtZero: true,
      },
    },
  };

  // Apply ATLAS brand colors
  const styledData = {
    ...data,
    datasets: data.datasets.map((dataset, i) => ({
      ...dataset,
      borderColor: dataset.borderColor || ['#0A5C5F', '#D97E3F', '#527A6E'][i],
      backgroundColor:
        dataset.backgroundColor || ['#0A5C5F', '#D97E3F', '#527A6E'][i],
    })),
  };

  return <Line options={options} data={styledData} />;
}
```

---

## Testing Strategy

### Unit Tests
```typescript
// __tests__/data-loader.test.ts
import { describe, it, expect } from 'vitest';
import { loadRTTData } from '@/lib/data-loader/rtt-loader';

describe('RTT Data Loader', () => {
  it('should load RTT data for a period', async () => {
    await loadRTTData('2024-09');
    // Assert records inserted
  });
});
```

### Integration Tests
- API endpoint responses
- Database queries
- Authentication flows

### E2E Tests (Playwright)
- User login
- Navigate hierarchy
- Create custom view
- Generate report

---

## Success Criteria

**MVP (Phase 1) Complete When:**
- ✅ User can log in
- ✅ User can select their organization
- ✅ User can view RTT waiting list data
- ✅ User can filter by time period
- ✅ User can see a simple chart
- ✅ Application runs in Docker locally

**Production Ready (Phase 5) Complete When:**
- ✅ Deployed to Cloudflare
- ✅ Multiple data sources available
- ✅ Organizational hierarchy working
- ✅ Custom views can be saved
- ✅ PDF reports can be generated
- ✅ Automated data updates running

**Feature Complete (Phase 6) Complete When:**
- ✅ All 28+ data sources integrated
- ✅ Insight categories fully implemented
- ✅ Dashboard builder working
- ✅ Alerts and notifications active
- ✅ Performance optimized
- ✅ Documentation complete

---

## Common Pitfalls & Solutions

### Pitfall 1: SQLite Scalability
**Problem:** Large datasets (millions of rows) slow down queries

**Solution:**
- Pre-aggregate common queries into summary tables
- Create indexes on all filter columns
- Use LIMIT for initial page loads
- Implement pagination
- Consider archiving old data to R2

### Pitfall 2: Time Zone Handling
**Problem:** Date inconsistencies between data sources

**Solution:**
- Store all dates as ISO 8601 strings (YYYY-MM-DD)
- Always use UTC for timestamps
- Display dates in user's local time zone in UI
- Document time zone assumptions

### Pitfall 3: Cloudflare D1 Limits
**Problem:** Hitting free tier limits

**Solution:**
- Implement caching in Workers KV
- Aggregate data before querying
- Use Cloudflare Cache API for responses
- Monitor usage with analytics

### Pitfall 4: Data Quality Issues
**Problem:** Missing, inconsistent, or erroneous data

**Solution:**
- Validation on data import
- Data quality flags in database
- User-facing quality indicators
- Manual review queue for anomalies

---

## Monitoring & Maintenance

### Daily
- Check data freshness
- Monitor error logs
- Review user feedback

### Weekly
- Review data quality metrics
- Check for new data publications
- Update ODS organizational data

### Monthly
- Performance review
- User analytics
- Cost monitoring (Cloudflare usage)

### Quarterly
- Major data source additions
- Feature releases
- User training sessions

---

## Next Steps After Build

1. **User Testing** - Beta test with 3-5 NHS trusts
2. **Feedback Loop** - Iterate based on user feedback
3. **Training Materials** - Create videos and guides
4. **Marketing** - NHS Communications, conferences
5. **Scale** - Support more users, add more data

---

*This implementation prompt provides everything needed to build ATLAS from scratch. Follow the phases sequentially, test thoroughly at each stage, and iterate based on feedback.*
