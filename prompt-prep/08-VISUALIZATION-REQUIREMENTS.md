# ATLAS Visualization Requirements

Complete specification for charts, graphs, and data visualization in ATLAS.

---

## Visualization Principles

### 1. Clarity Over Complexity
- Data should be immediately understandable
- Avoid chart junk and unnecessary decoration
- Use clear labels and legends
- Consistent color coding

### 2. Accessibility
- WCAG AA contrast compliance (minimum 4.5:1)
- Colorblind-safe palettes available
- Alt text for screen readers
- Keyboard navigation for interactions

### 3. Brand Consistency
- Apply ATLAS color palette
- Use Inter font for labels, IBM Plex Mono for data values
- Consistent styling across all chart types
- ATLAS branding on exported charts/reports

### 4. Performance
- Fast rendering (< 1 second for initial chart)
- Progressive loading for large datasets
- Efficient re-rendering on filter changes
- Lazy loading for off-screen charts

---

## Chart Library Selection

### Primary Libraries (Use All Three)

#### 1. **Chart.js** - General Purpose Charts

**Use For:**
- Line charts
- Bar charts (vertical and horizontal)
- Pie/doughnut charts
- Simple area charts

**Why:**
- Lightweight (187KB)
- Simple API
- Good performance
- Responsive out of the box
- Wide browser support

**Installation:**
```bash
npm install chart.js react-chartjs-2
```

**Example Chart Config:**
```typescript
const options = {
  responsive: true,
  maintainAspectRatio: true,
  plugins: {
    legend: {
      position: 'bottom' as const,
      labels: {
        font: {
          family: 'Inter',
          size: 14,
        },
        color: '#2C3E42', // ATLAS Charcoal
      },
    },
    title: {
      display: true,
      text: 'RTT Waiting List Trend',
      font: {
        family: 'Inter',
        size: 18,
        weight: '600',
      },
      color: '#2C3E42',
    },
  },
  scales: {
    y: {
      beginAtZero: true,
      ticks: {
        font: {
          family: 'IBM Plex Mono',
          size: 12,
        },
        color: '#5A6C70', // ATLAS Slate Gray
      },
    },
    x: {
      ticks: {
        font: {
          family: 'Inter',
          size: 12,
        },
        color: '#5A6C70',
      },
    },
  },
};
```

---

#### 2. **Apache ECharts** - Advanced Visualizations

**Use For:**
- Heatmaps
- Sankey diagrams (flow charts)
- Geographic maps (choropleth)
- Complex multi-axis charts
- Custom visualizations

**Why:**
- Extremely powerful and flexible
- Excellent documentation
- Good performance with large datasets
- Rich interaction options

**Installation:**
```bash
npm install echarts echarts-for-react
```

**Example Heatmap Config:**
```typescript
const heatmapOption = {
  tooltip: {
    position: 'top',
  },
  grid: {
    height: '70%',
    top: '10%',
  },
  xAxis: {
    type: 'category',
    data: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
    splitArea: {
      show: true,
    },
  },
  yAxis: {
    type: 'category',
    data: ['Trust A', 'Trust B', 'Trust C', 'Trust D'],
    splitArea: {
      show: true,
    },
  },
  visualMap: {
    min: 0,
    max: 100,
    calculable: true,
    orient: 'horizontal',
    left: 'center',
    bottom: '5%',
    inRange: {
      color: ['#E8F4F5', '#0A5C5F'], // ATLAS brand colors
    },
  },
  series: [
    {
      name: 'RTT Performance',
      type: 'heatmap',
      data: heatmapData,
      label: {
        show: true,
      },
    },
  ],
};
```

---

#### 3. **Observable Plot** - Statistical Graphics

**Use For:**
- Statistical distributions
- Box plots
- Violin plots
- Density plots
- Faceted/small multiple charts

**Why:**
- Excellent for exploratory analysis
- Concise declarative syntax
- D3-powered but simpler API
- Great for statistical patterns

**Installation:**
```bash
npm install @observablehq/plot
```

**Example Box Plot:**
```typescript
import * as Plot from '@observablehq/plot';

Plot.plot({
  marks: [
    Plot.boxY(data, {
      x: 'trust',
      y: 'waiting_time',
      fill: '#0A5C5F', // ATLAS Deep Teal
    }),
  ],
  x: {
    label: 'Trust',
  },
  y: {
    label: 'Median Wait Time (weeks)',
    grid: true,
  },
  color: {
    legend: true,
  },
});
```

---

### Custom Visualizations with D3.js

**Use For:**
- Statistical Process Control (SPC) charts
- Custom interactive visualizations
- Specialized NHS-specific charts

**Installation:**
```bash
npm install d3
```

---

## Standard Chart Types

### 1. Line Chart

**Use Cases:**
- Time series trends
- Performance over time
- Comparison of multiple series

**Key Features:**
- Multiple series support
- Annotations for policy changes
- Confidence intervals (shaded areas)
- Interactive tooltips
- Zoom/pan for long time series

**Data Format:**
```typescript
interface LineChartData {
  labels: string[]; // ['Jan 2024', 'Feb 2024', ...]
  datasets: {
    label: string; // 'Somerset NHS FT'
    data: number[]; // [8423, 8651, 8798, ...]
    borderColor?: string;
    backgroundColor?: string;
    fill?: boolean;
  }[];
}
```

**Example Use:**
- RTT waiting list size over 12 months
- A&E attendance trends
- Workforce FTE changes

---

### 2. Bar Chart

**Use Cases:**
- Comparisons between categories
- Benchmarking (trust vs peers)
- Specialty breakdowns

**Key Features:**
- Horizontal or vertical orientation
- Grouped or stacked bars
- Reference lines (e.g., national average)
- Sort by value
- Color coding by threshold

**Variants:**
- **Grouped:** Compare multiple metrics side-by-side
- **Stacked:** Show composition (e.g., staff groups)
- **Horizontal:** Better for long organization names

**Example Use:**
- Trust comparison: Total waiting list by trust in ICB
- Staff group breakdown: Nursing, Medical, AHP, etc.
- Performance vs target: 4-hour A&E performance by month

---

### 3. Table

**Use Cases:**
- Detailed data display
- Multi-metric comparison
- Exportable data view

**Key Features:**
- Sortable columns (click header)
- Filterable rows (search box)
- Sparklines in cells (mini trend charts)
- Conditional formatting (color-coded cells)
- Row/column totals and subtotals
- Pagination or virtual scrolling
- Export to CSV/Excel

**Column Types:**
- Text (left-aligned)
- Numbers (right-aligned, IBM Plex Mono font)
- Percentages (right-aligned, % symbol)
- Trends (sparkline chart)
- Status (icon/badge)

**Example Use:**
- RTT performance table: Trust, Specialty, Total Waiting, 18+weeks, 52+weeks
- Workforce table: Trust, Staff Group, Headcount, Vacancies, Turnover

---

### 4. Heatmap

**Use Cases:**
- Geographic variation
- Time-series intensity (rows=orgs, cols=months)
- Correlation matrices

**Key Features:**
- Color scale (sequential or diverging)
- Interactive cells (click for detail)
- Dendrogram/clustering (optional)
- Export as image

**ATLAS Color Scales:**

**Sequential (Low to High):**
```
#E8F4F5 → #B8D8DA → #7EB4B7 → #3B7A7D → #0A5C5F → #084649
(Light Teal → Deep Teal)
```

**Diverging (Bad ← Neutral → Good):**
```
#B8654D ← #E8D4CC ← #F7F9FA → #C8E1E3 → #3B7A7D
(Terracotta ← Gray → Teal)
```

**Example Use:**
- RTT performance heatmap: Trust (rows) × Month (columns), color = % 18+weeks
- Geographic heatmap: ICB performance by region
- Correlation: Data source A vs Data source B

---

### 5. Map (Choropleth)

**Use Cases:**
- Geographic distribution
- Regional variation
- ICB/LA-level metrics

**Key Features:**
- Color-coded regions
- Interactive tooltips
- Zoom/pan
- Click region for detail
- Legend with scale

**Geographic Levels:**
- NHS Regions (7)
- ICBs (36)
- Local Authorities (~150)
- Sub-ICB Locations

**Data Source:**
- ONS boundary files (GeoJSON/TopoJSON)
- NHS region boundaries from NHS Digital

**Example Use:**
- ICB-level RTT performance map
- Local authority deprivation and health outcomes
- Regional workforce vacancy rates

---

### 6. Sankey Diagram

**Use Cases:**
- Flow visualization
- Pathway analysis
- Resource allocation

**Key Features:**
- Node and link widths proportional to values
- Interactive highlighting (hover path)
- Color-coded by category
- Export as SVG

**Example Use:**
- Patient flow: Referral → Triage → Treatment → Discharge
- Funding flow: National → ICB → Provider
- Workforce flow: Recruitment → In-post → Leaving

---

## Statistical Process Control (SPC) Charts

### XmR Chart (Individuals and Moving Range)

**Purpose:**
- Monitor process performance over time
- Detect special cause variation (signals requiring action)
- Distinguish from common cause (natural variation)

**NHS Context:**
- NHS-R Community "Making Data Count" standards
- Used for quality improvement
- Patient safety metrics

### Key Components

**1. Data Points (X Chart)**
- Individual measurements over time
- Connected by lines

**2. Center Line (Mean)**
- Average of all data points
- Horizontal line

**3. Control Limits**
- **Upper Control Limit (UCL):** Mean + 2.66 × MR̄
- **Lower Control Limit (LCL):** Mean - 2.66 × MR̄
- Represents ±3 sigma equivalent
- Natural process limits

**4. Special Cause Rules** (NHS-R Making Data Count)

Detect signals indicating non-random variation:

**Rule 1: Any point outside control limits**
- Single point beyond UCL or LCL

**Rule 2: Shift**
- 8 or more consecutive points all above or all below the center line

**Rule 3: Trend**
- 6 or more consecutive points all increasing or all decreasing

**Rule 4: Two out of three points near control limit**
- 2 out of 3 consecutive points in outer third (between center and control limit)

**Rule 5: Astronomical point**
- Point beyond 3.5 sigma (rare, extreme outlier)

### Visual Styling

**Colors (ATLAS Brand):**
- Data points: #0A5C5F (Deep Teal) - normal points
- Special cause: #D97E3F (Amber) - flagged points
- Center line: #2C3E42 (Charcoal) - solid line
- Control limits: #8F9FA3 (Cool Gray) - dashed lines

**Annotations:**
- Label special cause points with rule number
- Option to add process change annotations (e.g., "New policy implemented")

### Implementation (D3.js)

```typescript
import * as d3 from 'd3';

interface SPCData {
  date: Date;
  value: number;
  movingRange?: number;
  specialCause?: string; // Rule name if flagged
}

function calculateControlLimits(data: number[]) {
  const mean = d3.mean(data);
  const movingRanges = data.slice(1).map((v, i) => Math.abs(v - data[i]));
  const mrBar = d3.mean(movingRanges);
  const ucl = mean + 2.66 * mrBar;
  const lcl = mean - 2.66 * mrBar;
  
  return { mean, ucl, lcl: Math.max(0, lcl) }; // LCL cannot be negative for count data
}

function detectSpecialCause(data: SPCData[], limits: { mean: number; ucl: number; lcl: number }) {
  // Rule 1: Point outside limits
  data.forEach((d, i) => {
    if (d.value > limits.ucl || d.value < limits.lcl) {
      d.specialCause = 'Rule 1: Outside limits';
    }
  });

  // Rule 2: Shift (8+ points same side of mean)
  for (let i = 7; i < data.length; i++) {
    const last8 = data.slice(i - 7, i + 1);
    if (last8.every(d => d.value > limits.mean) || last8.every(d => d.value < limits.mean)) {
      last8.forEach(d => {
        if (!d.specialCause) d.specialCause = 'Rule 2: Shift';
      });
    }
  }

  // Rule 3: Trend (6+ consecutive increasing/decreasing)
  for (let i = 5; i < data.length; i++) {
    const last6 = data.slice(i - 5, i + 1);
    const increasing = last6.every((d, j) => j === 0 || d.value > last6[j - 1].value);
    const decreasing = last6.every((d, j) => j === 0 || d.value < last6[j - 1].value);
    if (increasing || decreasing) {
      last6.forEach(d => {
        if (!d.specialCause) d.specialCause = 'Rule 3: Trend';
      });
    }
  }

  return data;
}

// Render SPC chart with D3
function renderSPCChart(data: SPCData[], container: HTMLElement) {
  const margin = { top: 40, right: 40, bottom: 60, left: 60 };
  const width = 800 - margin.left - margin.right;
  const height = 400 - margin.top - margin.bottom;

  const svg = d3.select(container)
    .append('svg')
    .attr('width', width + margin.left + margin.right)
    .attr('height', height + margin.top + margin.bottom)
    .append('g')
    .attr('transform', `translate(${margin.left},${margin.top})`);

  // Scales
  const x = d3.scaleTime()
    .domain(d3.extent(data, d => d.date))
    .range([0, width]);

  const y = d3.scaleLinear()
    .domain([0, d3.max(data, d => Math.max(d.value, limits.ucl)) * 1.1])
    .range([height, 0]);

  // Axes
  svg.append('g')
    .attr('transform', `translate(0,${height})`)
    .call(d3.axisBottom(x));

  svg.append('g')
    .call(d3.axisLeft(y));

  // Control limits
  svg.append('line')
    .attr('x1', 0)
    .attr('x2', width)
    .attr('y1', y(limits.ucl))
    .attr('y2', y(limits.ucl))
    .attr('stroke', '#8F9FA3')
    .attr('stroke-width', 2)
    .attr('stroke-dasharray', '5,5');

  svg.append('line')
    .attr('x1', 0)
    .attr('x2', width)
    .attr('y1', y(limits.mean))
    .attr('y2', y(limits.mean))
    .attr('stroke', '#2C3E42')
    .attr('stroke-width', 2);

  svg.append('line')
    .attr('x1', 0)
    .attr('x2', width)
    .attr('y1', y(limits.lcl))
    .attr('y2', y(limits.lcl))
    .attr('stroke', '#8F9FA3')
    .attr('stroke-width', 2)
    .attr('stroke-dasharray', '5,5');

  // Data line
  const line = d3.line<SPCData>()
    .x(d => x(d.date))
    .y(d => y(d.value));

  svg.append('path')
    .datum(data)
    .attr('fill', 'none')
    .attr('stroke', '#0A5C5F')
    .attr('stroke-width', 2)
    .attr('d', line);

  // Data points
  svg.selectAll('.dot')
    .data(data)
    .enter()
    .append('circle')
    .attr('cx', d => x(d.date))
    .attr('cy', d => y(d.value))
    .attr('r', 4)
    .attr('fill', d => d.specialCause ? '#D97E3F' : '#0A5C5F')
    .attr('stroke', '#FFFFFF')
    .attr('stroke-width', 2);

  // Labels for special cause
  svg.selectAll('.special-label')
    .data(data.filter(d => d.specialCause))
    .enter()
    .append('text')
    .attr('x', d => x(d.date))
    .attr('y', d => y(d.value) - 10)
    .attr('text-anchor', 'middle')
    .attr('font-family', 'Inter')
    .attr('font-size', '10px')
    .attr('fill', '#D97E3F')
    .text(d => d.specialCause.split(':')[0]); // Just rule number
}
```

**Example Use:**
- Hospital mortality rates (SHMI) over time
- Infection rates (HCAI) with special cause detection
- A&E 4-hour performance by week

---

## Interactive Features

### 1. Tooltips
- Show on hover
- Display exact values
- Include context (e.g., org name, date)
- Format numbers with IBM Plex Mono

### 2. Click Actions
- Drill-down to detail
- Filter other charts
- Open detail modal
- Navigate to related view

### 3. Brush & Zoom
- Select time range on line chart
- Zoom to selection
- Reset zoom button

### 4. Legend Interaction
- Click legend item to toggle series
- Highlight series on hover

### 5. Cross-filtering
- Click bar in chart A → filters chart B
- Maintain filter state across views

---

## Export Capabilities

### Image Export
- **PNG:** Raster image (charts, maps)
- **SVG:** Vector image (scalable, editable)
- Resolution: 2x for retina displays

### Data Export
- **CSV:** Underlying data table
- **Excel:** Formatted workbook with charts
- **JSON:** Structured data for API

### Report Export
- **PDF:** Multi-page report with charts embedded
- Branding applied (ATLAS logo, colors)
- Page numbers and table of contents

---

## Accessibility Requirements

### Color
- Never rely on color alone
- Use patterns/shapes in addition to color
- Provide colorblind-safe palette option
- Maintain 4.5:1 contrast ratio

### Keyboard Navigation
- Tab through interactive elements
- Enter/Space to activate
- Arrow keys to navigate chart data points
- Esc to close modal/tooltip

### Screen Readers
- Descriptive alt text for charts
- ARIA labels for interactive elements
- Table view alternative for every chart
- Announce updates on filter changes

---

## Performance Targets

### Initial Render
- < 1 second for simple charts (< 1000 data points)
- < 3 seconds for complex charts (< 10,000 data points)
- Loading spinner for > 1 second

### Interactions
- Tooltip: < 100ms
- Filter application: < 500ms
- Drill-down: < 1 second

### Large Datasets
- Virtual scrolling for tables (> 1000 rows)
- Pagination option
- Server-side aggregation for > 100K rows
- Progressive loading (load visible first)

---

## Chart Style Guide

### Colors (ATLAS Brand)

**Primary Data Series:**
1. #0A5C5F (Deep Teal)
2. #D97E3F (Amber)
3. #527A6E (Mineral Green)
4. #B8654D (Terracotta)
5. #3B7A7D (Slate Teal)
6. #8B6F47 (Warm Brown)

**Backgrounds:**
- Chart background: #FFFFFF (White)
- Page background: #F7F9FA (Background Tint)

**Text:**
- Primary: #2C3E42 (Charcoal)
- Secondary: #5A6C70 (Slate Gray)
- Tertiary: #8F9FA3 (Cool Gray)

**Grid Lines:**
- #D8E1E3 (Light Gray), 1px solid

### Typography

**Chart Titles:**
- Font: Inter SemiBold
- Size: 18px
- Color: #2C3E42

**Axis Labels:**
- Font: Inter Regular
- Size: 14px
- Color: #5A6C70

**Axis Values:**
- Font: IBM Plex Mono Regular
- Size: 12px
- Color: #5A6C70

**Data Labels:**
- Font: IBM Plex Mono Medium
- Size: 12-14px
- Color: #2C3E42

### Spacing
- Chart padding: 24px
- Grid line spacing: Auto (based on data range)
- Legend item spacing: 12px
- Minimum touch target: 44×44px (mobile)

---

*This visualization specification ensures consistent, accessible, and performant charts across ATLAS while maintaining brand identity and meeting NHS analytical standards.*
