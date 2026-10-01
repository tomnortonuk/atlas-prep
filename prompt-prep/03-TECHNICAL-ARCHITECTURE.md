# ATLAS Technical Architecture

Complete technical specification for building and deploying ATLAS.

---

## Architecture Overview

**ATLAS** is a serverless, edge-deployed data intelligence platform built on:
- **Frontend:** Modern JavaScript framework (Next.js / SvelteKit / Remix)
- **Database:** SQLite (local) / Cloudflare D1 (production)
- **Compute:** Cloudflare Workers (serverless functions at the edge)
- **Hosting:** Cloudflare Pages (static assets + dynamic rendering)
- **Data Pipeline:** Scheduled Workers for automated data ingestion

### Design Principles

1. **Free/Low-Cost Hosting:** Cloudflare's free tier supports significant usage
2. **Edge Performance:** Global distribution via Cloudflare's CDN
3. **SQLite Simplicity:** No database server management, portability
4. **Progressive Enhancement:** Works offline, fast initial load
5. **Data Privacy:** No third-party analytics or tracking by default

---

## System Components

```
┌─────────────────────────────────────────────────────────────┐
│                         USER BROWSER                        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │   Dashboard  │  │   Explorer   │  │   Reports    │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
└────────────────────────────┬────────────────────────────────┘
                             │ HTTPS
                             ▼
┌─────────────────────────────────────────────────────────────┐
│                   CLOUDFLARE PAGES (CDN)                    │
│  • Static assets (JS, CSS, images)                          │
│  • Server-side rendered pages (via Workers)                 │
│  • API routes (via Workers)                                 │
└────────────────────────────┬────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────┐
│                    CLOUDFLARE WORKERS                       │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  API Layer                                          │   │
│  │  • Data queries                                     │   │
│  │  • User auth                                        │   │
│  │  • Report generation                                │   │
│  └─────────────────────────────────────────────────────┘   │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  Data Ingestion Workers (Scheduled)                 │   │
│  │  • Check for new data                               │   │
│  │  • Download and process                             │   │
│  │  • Update D1 database                               │   │
│  └─────────────────────────────────────────────────────┘   │
└────────────────────────────┬────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────┐
│                      CLOUDFLARE D1                          │
│  • SQLite database at the edge                              │
│  • Replicated across Cloudflare regions                     │
│  • Accessed via Workers                                     │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                   EXTERNAL DATA SOURCES                     │
│  • NHS England Open Data Portal                             │
│  • NHS Digital dissemination                                │
│  • CQC API                                                  │
│  • OHID Fingertips API                                      │
└─────────────────────────────────────────────────────────────┘
```

---

## Local Development Stack

### Docker Compose Setup

```yaml
# docker-compose.yml
version: '3.8'

services:
  atlas-app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - ./src:/app/src
      - ./data:/app/data        # SQLite database files
    environment:
      - NODE_ENV=development
      - DATABASE_PATH=/app/data/atlas.db
    depends_on:
      - data-loader

  data-loader:
    build:
      context: .
      dockerfile: Dockerfile.loader
    volumes:
      - ./data:/app/data
      - ./scripts:/app/scripts
    environment:
      - DATABASE_PATH=/app/data/atlas.db
    # Run data loading scripts on schedule or manually
```

### Local Database

```bash
# Initialize SQLite database
sqlite3 data/atlas.db < schema.sql

# Seed with reference data
node scripts/seed-reference-data.js

# Test data loading
node scripts/load-rtt-data.js
```

---

## Production Architecture (Cloudflare)

### Cloudflare D1 Database

**Limits (Free Tier):**
- 5GB storage
- 5M reads/day
- 100K writes/day
- First 1000 reads/day free, then $0.001 per 1000

**Optimization Strategies:**
1. **Aggregate tables** - Pre-compute summaries to reduce query load
2. **Caching** - Cache frequently accessed data in Workers KV
3. **Lazy loading** - Load detail data on-demand
4. **Time-based partitioning** - Archive old data to R2 storage

### Cloudflare Workers

**Limits (Free Tier):**
- 100,000 requests/day
- 10ms CPU time per request
- 128MB memory per Worker

**Worker Types:**

1. **API Worker** (`/api/*`)
   - Handle data queries from frontend
   - User authentication
   - Report generation
   - Response caching

2. **Data Ingestion Workers** (Scheduled)
   - Check NHS England for new data publications
   - Download CSV/Excel files
   - Parse and transform data
   - Insert/update D1 database
   - Run daily/weekly via Cron Triggers

3. **PDF Generation Worker**
   - Use Puppeteer (via browserless API or local)
   - Generate static PDF reports
   - Store in R2 object storage
   - Return signed URL

### Cloudflare Pages

**Features:**
- Automatic Git integration (push to deploy)
- Preview deployments for PRs
- Custom domains with free SSL
- Global CDN
- Server-side rendering via Workers integration

### Cloudflare R2 (Object Storage)

**Use Cases:**
- Static PDF reports
- Large CSV exports
- Archive historical data
- User uploads (if needed)

**Limits (Free Tier):**
- 10GB storage
- 1M Class A operations/month
- 10M Class B operations/month

---

## Tech Stack Recommendations

### Frontend Framework

**Recommended: Next.js 14+ (App Router)**

**Why:**
- Best-in-class DX and performance
- Server components for efficient data loading
- API routes built-in
- Static generation + SSR hybrid
- Great visualization library ecosystem
- Cloudflare Pages adapter available

**Alternative: SvelteKit**
- Lighter weight
- Excellent performance
- Less ecosystem maturity for data viz

**Alternative: Remix**
- Data-focused architecture
- Nested routing great for complex UIs
- Good Cloudflare Workers support

### UI Component Library

**Recommended: shadcn/ui + Tailwind CSS**

- Unstyled primitives (Radix UI)
- Copy-paste components (no dependency)
- Fully customizable (matches ATLAS brand)
- Excellent accessibility
- TypeScript native

**Alternative: MUI (Material-UI)**
- Comprehensive component set
- Good data table components
- Heavier bundle size

### Data Visualization

**Multi-library approach recommended:**

1. **Chart.js** - General charts
   ```bash
   npm install chart.js react-chartjs-2
   ```
   - Simple, performant
   - Good for basic line, bar, pie charts
   - Smaller bundle

2. **Apache ECharts** - Advanced charts
   ```bash
   npm install echarts echarts-for-react
   ```
   - Powerful customization
   - Heatmaps, Sankey diagrams, geo maps
   - Large dataset handling

3. **Observable Plot** - Statistical viz
   ```bash
   npm install @observablehq/plot
   ```
   - Excellent for statistical graphics
   - Concise declarative syntax
   - Good for exploratory analysis

4. **Custom D3.js** - SPC charts
   ```bash
   npm install d3
   ```
   - Full control for Statistical Process Control charts
   - NHS-R Making Data Count standards
   - XmR chart rules

### Database ORM / Query Builder

**Recommended: Drizzle ORM**

```bash
npm install drizzle-orm better-sqlite3
npm install -D drizzle-kit
```

**Why:**
- TypeScript-first
- Excellent SQLite support
- Minimal runtime overhead
- Great Cloudflare D1 support
- Type-safe queries

**Schema Example:**

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
});

export const rttWaitingList = sqliteTable('rtt_waiting_list', {
  recordId: integer('record_id').primaryKey({ autoIncrement: true }),
  reportingPeriod: text('reporting_period').notNull(),
  providerOdsCode: text('provider_ods_code').notNull(),
  specialtyCode: text('specialty_code').notNull(),
  weeksWaitingBand: text('weeks_waiting_band'),
  patientCount: integer('patient_count').notNull(),
});
```

### PDF Generation

**Recommended: Puppeteer (via external service for Cloudflare)**

Options:
1. **browserless.io** - Managed Puppeteer API (paid)
2. **Gotenberg** - Self-hosted PDF service (Docker)
3. **PDFKit** - Pure JS PDF generation (limited layout)

For Cloudflare Workers (no Node.js):
```typescript
// Use external API
const pdf = await fetch('https://api.browserless.io/pdf', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    html: reportHTML,
    options: { format: 'A4', printBackground: true }
  })
});
```

### Authentication

**Recommended: Clerk**

```bash
npm install @clerk/nextjs
```

**Why:**
- Easy Cloudflare Workers integration
- User management UI
- Multi-factor authentication
- Organization support
- Free tier: 10,000 MAU

**Alternative: Auth.js (NextAuth)**
- Open source
- More configuration required
- Cloudflare adapter available

---

## Data Pipeline Architecture

### Scheduled Data Ingestion

```typescript
// workers/data-ingestion/rtt-loader.ts

export default {
  async scheduled(event: ScheduledEvent, env: Env) {
    // 1. Check if new data available
    const latestPublished = await checkNHSEnglandPublications('RTT');
    const latestLoaded = await getLatestLoadedPeriod(env.DB, 'RTT');
    
    if (latestPublished > latestLoaded) {
      // 2. Download new data
      const data = await downloadRTTData(latestPublished);
      
      // 3. Parse and transform
      const records = parseRTTCSV(data);
      
      // 4. Load into D1
      await loadRTTData(env.DB, records, latestPublished);
      
      // 5. Update availability tracker
      await updateDataAvailability(env.DB, 'RTT', latestPublished, 'complete');
      
      console.log(`RTT data loaded for period: ${latestPublished}`);
    }
  }
};
```

### Cron Schedule (wrangler.toml)

```toml
[triggers]
crons = [
  "0 9 * * 1 data-ingestion-rtt",      # Monday 9am - check RTT
  "0 9 * * 2 data-ingestion-ecds",     # Tuesday 9am - check ECDS
  "0 9 * * 3 data-ingestion-mhsds",    # Wednesday 9am - check MHSDS
  "0 9 1 * * data-ingestion-workforce" # 1st of month - check workforce
]
```

---

## API Design

### RESTful Endpoints

```
GET  /api/organizations                    # List all organizations
GET  /api/organizations/:ods                # Get organization details
GET  /api/data-sources                      # List available data sources
GET  /api/data-sources/:sourceId/latest     # Get latest period for source
GET  /api/data/rtt                          # Query RTT data (with filters)
GET  /api/data/ae-performance               # Query A&E data
GET  /api/data/workforce                    # Query workforce data
POST /api/reports/generate                  # Generate custom report
GET  /api/reports/:reportId                 # Get published report
POST /api/saved-views                       # Save user view
GET  /api/saved-views                       # List user's saved views
```

### Query Parameters (Standard)

```
?period=2024-09        # Reporting period (YYYY-MM or YYYY-MM-DD)
?org=RVJ               # Filter by organization ODS code
?icb=QWE               # Filter by ICB
?region=Y56            # Filter by region
?specialty=100         # Filter by specialty (where applicable)
?groupBy=trust         # Aggregation level (trust, icb, region, national)
?format=json           # Response format (json, csv)
```

### Response Format

```json
{
  "meta": {
    "source": "RTT",
    "period": "2024-09",
    "queryTime": "2024-10-01T12:00:00Z",
    "recordCount": 1247
  },
  "data": [
    {
      "providerOds": "RVJ",
      "providerName": "King's College Hospital NHS Foundation Trust",
      "specialty": "General Surgery",
      "totalWaiting": 8423,
      "waiting18Plus": 1256,
      "waiting52Plus": 42
    }
  ],
  "pagination": {
    "page": 1,
    "perPage": 50,
    "totalPages": 25
  }
}
```

---

## Performance Optimization

### Database Optimization

1. **Indexes on all query paths**
   ```sql
   CREATE INDEX idx_rtt_composite ON rtt_waiting_list(
       reporting_period, provider_ods_code, specialty_code
   );
   ```

2. **Pre-aggregated views**
   ```sql
   CREATE TABLE agg_rtt_monthly_trust AS
   SELECT 
       reporting_period,
       provider_ods_code,
       SUM(patient_count) as total_waiting
   FROM rtt_waiting_list
   GROUP BY reporting_period, provider_ods_code;
   ```

3. **Limit result sets**
   ```sql
   SELECT * FROM large_table LIMIT 1000;
   ```

### Worker Optimization

1. **Cache responses**
   ```typescript
   const cache = caches.default;
   const cacheKey = new Request(url, request);
   let response = await cache.match(cacheKey);
   
   if (!response) {
     response = await fetchData();
     await cache.put(cacheKey, response.clone());
   }
   ```

2. **Batch database queries**
   ```typescript
   // Bad: N+1 queries
   for (const org of orgs) {
     const data = await db.query('SELECT * FROM rtt WHERE org = ?', org);
   }
   
   // Good: Single query
   const allData = await db.query(
     'SELECT * FROM rtt WHERE org IN (?, ?, ?)', 
     orgs
   );
   ```

3. **Stream large responses**
   ```typescript
   const { readable, writable } = new TransformStream();
   streamJSONArray(data, writable);
   return new Response(readable, {
     headers: { 'Content-Type': 'application/json' }
   });
   ```

### Frontend Optimization

1. **Code splitting**
   ```typescript
   const Chart = dynamic(() => import('./Chart'), { ssr: false });
   ```

2. **Virtual scrolling** for large tables
   ```typescript
   import { useVirtualizer } from '@tanstack/react-virtual';
   ```

3. **Progressive loading**
   - Load summary first
   - Detail on demand
   - Virtualize long lists

---

## Security Considerations

### Data Access Control

1. **Row-level security**
   ```sql
   -- Users can only see their own organization's data
   CREATE VIEW user_data AS
   SELECT * FROM sensitive_table
   WHERE org_code = get_user_org_code();
   ```

2. **API authentication**
   ```typescript
   import { getAuth } from '@clerk/nextjs/server';
   
   export async function GET(request: Request) {
     const { userId } = getAuth(request);
     if (!userId) return new Response('Unauthorized', { status: 401 });
     // ... handle request
   }
   ```

3. **Rate limiting**
   ```typescript
   import { Ratelimit } from '@upstash/ratelimit';
   const ratelimit = new Ratelimit({
     redis: env.REDIS,
     limiter: Ratelimit.slidingWindow(100, '1 h'),
   });
   ```

### Data Privacy

- **Aggregate data only** (no patient-level data stored)
- **Pseudonymization** where required
- **HTTPS everywhere**
- **No third-party tracking**
- **GDPR compliance** for user data

---

## Deployment Process

### GitHub Actions CI/CD

```yaml
# .github/workflows/deploy.yml
name: Deploy to Cloudflare

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm ci
      - run: npm run build
      - uses: cloudflare/wrangler-action@v3
        with:
          apiToken: ${{ secrets.CLOUDFLARE_API_TOKEN }}
```

### Deployment Steps

```bash
# 1. Build frontend
npm run build

# 2. Deploy to Cloudflare Pages
npx wrangler pages deploy ./out --project-name=atlas

# 3. Run database migrations (if needed)
npx wrangler d1 migrations apply atlas-db --remote

# 4. Deploy Workers
npx wrangler deploy workers/data-ingestion
```

---

## Monitoring & Observability

### Cloudflare Analytics
- Request volume
- Error rates
- Response times
- Geographic distribution

### Custom Logging

```typescript
console.log(JSON.stringify({
  timestamp: new Date().toISOString(),
  level: 'info',
  message: 'Data loaded successfully',
  metadata: { source: 'RTT', period: '2024-09', records: 12450 }
}));
```

### Sentry Integration (Optional)

```typescript
import * as Sentry from '@sentry/nextjs';

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
});
```

---

## Cost Estimation

### Cloudflare Free Tier

| Service | Free Tier | Expected Usage | Cost |
|---------|-----------|----------------|------|
| Pages | Unlimited | 1 site | $0 |
| Workers | 100K requests/day | ~10K/day | $0 |
| D1 | 5GB, 5M reads/day | <1GB, <100K reads/day | $0 |
| R2 | 10GB storage | <5GB PDFs | $0 |

**Total Estimated Cost: $0/month** (within free tiers)

### Scaling Beyond Free Tier

If usage grows:
- Workers Paid: $5/month + $0.50 per million requests
- D1 Paid: $0.75/GB-month + $0.001 per 1000 reads
- R2 Paid: $0.015/GB-month storage

---

*This architecture provides a scalable, performant, and cost-effective foundation for ATLAS while maintaining simplicity for local development.*
