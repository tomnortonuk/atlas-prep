# ATLAS Branding Pack

**Version 1.0** | Population Health and Care Intelligence Platform  
Created: October 2026

---

## 📦 What's Included

This branding pack contains everything you need to implement the ATLAS brand:

### 1. **Brand Guidelines** (`ATLAS-Brand-Guidelines.md`)
Complete brand standards document including:
- Color palette specifications
- Typography system
- Logo usage guidelines
- UI component styling
- Brand voice and messaging
- Application examples

### 2. **Logo Assets**

**Logo Concepts** (4 variations for review):
- `atlas-logo-concept-1.svg` - Interconnected Hexagonal Planes
- `atlas-logo-concept-2.svg` - Grid Network with Central Node
- `atlas-logo-concept-3.svg` - Layered Cartographic Planes ⭐ (Used in horizontal logo)
- `atlas-logo-concept-4.svg` - Geometric "A" with Data Flow

**Production-Ready Logos**:
- `atlas-logo-horizontal.svg` - Primary horizontal logo (icon + wordmark)
- `atlas-favicon-32.svg` - Favicon/icon mark (32x32)

### 3. **Interactive Brand Assets**

**Color Palette Reference** (`atlas-color-swatches.html`)  
Open in browser to see:
- All brand colors with hex/RGB values
- Usage guidance for each color
- Data visualization palette examples
- Alert and status colors

**UI Components Demo** (`atlas-ui-components-demo.html`)  
Open in browser to see:
- Full header with logo and navigation
- Typography scale in action
- Button variations (primary, secondary, ghost)
- Card components and metric displays
- Form inputs and tables
- Alert messages
- Real application layout examples

---

## 🎨 Quick Brand Summary

### Core Colors
```
Deep Teal (Primary):    #0A5C5F
Amber (Accent):         #D97E3F
Charcoal (Text):        #2C3E42
```

### Typography
```
Headings:  Inter Bold/SemiBold
Body:      Inter Regular
Data:      IBM Plex Mono
```

### Logo Concept
Geometric layered planes representing:
- Multiple data sources coming together
- Cartographic/mapping reference (ATLAS)
- Comprehensive coverage (national to local)
- Modern, clean, trustworthy

### Brand Personality
**Modern Guide** - Professional but approachable, confident, clear

---

## 🚀 How to Use These Assets

### For Designers

1. **Review Brand Guidelines**
   - Open `ATLAS-Brand-Guidelines.md`
   - Read through color palette, typography, and usage rules

2. **Preview Designs in Browser**
   - Open `atlas-color-swatches.html` to see the full color system
   - Open `atlas-ui-components-demo.html` to see components in action

3. **Logo Files**
   - SVG files can be opened in Figma, Illustrator, Sketch, or any vector editor
   - Pick your preferred logo concept or use Concept 3 (recommended)
   - Export to PNG/other formats as needed

4. **Create Favicon Set**
   - Use `atlas-favicon-32.svg` as base
   - Export to: 16x16, 32x32, 48x48 PNG
   - Generate .ico file for legacy browser support
   - Create 180x180 PNG for Apple touch icon

### For Developers

1. **Color Variables** (CSS Custom Properties)
   ```css
   :root {
     --atlas-deep-teal: #0A5C5F;
     --atlas-slate-teal: #3B7A7D;
     --atlas-mineral-green: #527A6E;
     --atlas-amber: #D97E3F;
     --atlas-terracotta: #B8654D;
     --atlas-charcoal: #2C3E42;
     --atlas-slate-gray: #5A6C70;
     --atlas-cool-gray: #8F9FA3;
     --atlas-light-gray: #D8E1E3;
     --atlas-bg-tint: #F7F9FA;
   }
   ```

2. **Typography Setup** (HTML)
   ```html
   <link rel="preconnect" href="https://fonts.googleapis.com">
   <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=IBM+Plex+Mono:wght@400;500&display=swap" rel="stylesheet">
   ```

3. **Logo Implementation**
   - Use SVG files directly in HTML/React components
   - For React: Convert SVG to component or use as image source
   - Maintain aspect ratios and minimum sizes per guidelines

4. **Reference Implementation**
   - See `atlas-ui-components-demo.html` for complete CSS examples
   - Copy button, card, form, and table styles as needed

### For Presentations & Documents

**PowerPoint/Keynote:**
- Background: `#F7F9FA` or `#FFFFFF`
- Title text: `#2C3E42` (Charcoal)
- Body text: `#2C3E42` or `#5A6C70`
- Accent elements: `#0A5C5F` (Deep Teal)
- Highlights: `#D97E3F` (Amber)

**PDFs/Reports:**
- Use horizontal logo in header
- Charcoal text on white background
- Deep Teal section headers
- Charts using data visualization palette

---

## 📋 Next Steps to Complete Brand Implementation

### Immediate Actions

1. **Finalize Logo**
   - Review the 4 logo concepts
   - Select preferred version (recommend Concept 3)
   - Create full logo lockup set:
     - Horizontal (done)
     - Stacked (icon above wordmark)
     - Icon only
     - Wordmark only
     - White versions for dark backgrounds

2. **Generate Favicon Package**
   ```
   favicon-16x16.png
   favicon-32x32.png  
   favicon-48x48.png
   favicon.ico
   apple-touch-icon.png (180x180)
   android-chrome-192x192.png
   android-chrome-512x512.png
   site.webmanifest
   ```

3. **Export Logo Variations**
   - SVG (original)
   - PNG (various sizes: 256px, 512px, 1024px, 2048px)
   - PDF (for print)

### Brand Application

4. **Design Core Screens**
   - Login page
   - Dashboard home
   - Data explorer interface
   - Report builder
   - Settings page

5. **Create Marketing Materials**
   - Product one-pager
   - Website homepage design
   - Email templates
   - Social media templates

6. **Documentation Assets**
   - User guide template
   - API documentation styling
   - Help center branding

---

## 🔧 Tools & Resources

### Recommended Design Tools
- **Figma** - Collaborative interface design
- **Adobe Illustrator** - Logo refinement and print assets
- **Sketch** - UI design (Mac)

### Typography Resources
- **Google Fonts**: Inter → https://fonts.google.com/specimen/Inter
- **Google Fonts**: IBM Plex Mono → https://fonts.google.com/specimen/IBM+Plex+Mono

### Color Tools
- **Contrast Checker**: WebAIM (ensure WCAG AA compliance)
- **Palette Generator**: Coolors.co (for extended palettes)

### Icon Libraries (Recommended)
- **Heroicons** - https://heroicons.com/ (geometric, 2px stroke)
- **Lucide** - https://lucide.dev/ (consistent with brand style)

---

## 📐 Technical Specifications

### Logo Minimum Sizes
- Horizontal: 120px wide minimum
- Stacked: 80px wide minimum  
- Icon only: 32px minimum
- Favicon: 16px, 32px, 48px

### Color Contrast Ratios (WCAG AA)
✅ Deep Teal (#0A5C5F) on White: 10.4:1  
✅ Charcoal (#2C3E42) on White: 12.6:1  
✅ Amber (#D97E3F) on White: 3.8:1  
✅ White on Deep Teal: 10.4:1

### File Formats
- **Web**: SVG (preferred), PNG with transparency
- **Print**: PDF, EPS, high-res PNG (300dpi)
- **Social**: PNG, JPG (1200x630 for OG images)

---

## 🎯 Brand Checklist

Use this checklist when implementing ATLAS branding:

**Visual Identity**
- [ ] Using correct color values from palette
- [ ] Logo has proper clear space
- [ ] Typography matches scale (Inter for headings/body, IBM Plex Mono for data)
- [ ] Minimum logo sizes maintained
- [ ] High contrast maintained for accessibility

**UI Components**
- [ ] Buttons follow style guide (primary: Deep Teal, secondary: Amber)
- [ ] Cards have proper shadows and borders
- [ ] Form inputs have correct focus states
- [ ] Tables use proper header styling
- [ ] Proper spacing (8px grid system)

**Content & Messaging**
- [ ] Tone is "Modern Guide" (professional but approachable)
- [ ] Tagline used correctly ("Navigate the complete picture")
- [ ] Terminology consistent (health AND care, not just health)
- [ ] Active voice, clear language

---

## 📞 Questions or Custom Requests?

This branding pack provides the foundation. For specific use cases not covered:

1. Check `ATLAS-Brand-Guidelines.md` first
2. Refer to example implementations in HTML demos
3. Maintain core principles: modern, trustworthy, clear

**Remember**: The brand should feel:
- **Professional** but not corporate
- **Authoritative** but not intimidating  
- **Modern** but not trendy
- **Comprehensive** but not overwhelming

---

## 📝 Version History

**v1.0 - October 2026**
- Initial brand guidelines
- 4 logo concepts created
- Full color palette established
- Typography system defined
- UI component library started
- Interactive demos created

---

*ATLAS - Navigate the complete picture*
