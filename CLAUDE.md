# POWERS Website Evolution — Project Brief & Decision Log

## Deploy Workflow
- Versioned deploy folder: `POWERS-Website-Evolution-vX.X.X/`
- On every deploy: increment version, update `<title>` and visible UI version, offer ZIP download
- Pipeline: ZIP download → unzip → copy to local GitHub repo → commit + push via GitHub Desktop → Netlify auto-deploy
- Never attempt direct browser-to-API deploys

---

## Design System

### Brand Anchors
- Navy: `#183a61` (navy-800) — H1, header bg (if navy), logo, primary CTA bg
- Gold: `#eabb71` (gold-400) — eyebrow labels, rule lines, logo mark, icon accents, header bottom border

### Full Color Scale
| Token | Hex | Usage |
|---|---|---|
| navy-50 | #f0f4f8 | Subtle tint backgrounds |
| navy-100 | #d6e2ee | Section background tints |
| navy-200 | #a8c0d6 | Borders, dividers |
| navy-400 | #4a6a8a | H2/subheads, tagline, secondary nav text |
| navy-600 | #2b5070 | Body links |
| navy-800 | #183a61 | BRAND ANCHOR |
| navy-900 | #0d2442 | Footer background |
| gold-50 | #fdf6e8 | Warm section tints |
| gold-100 | #f7e4b8 | Card highlights |
| gold-200 | #f2d08a | Accent borders |
| gold-400 | #eabb71 | BRAND ANCHOR |
| gold-600 | #c9963e | CTA hover, interactive emphasis |
| white | #ffffff | Page background, cards |
| gray-50 | #f5f5f3 | Alternating section background |
| gray-100 | #e8e8e4 | Borders, hairlines |
| gray-200 | #c4c4be | Disabled, placeholder |
| gray-400 | #888884 | Captions, meta text |
| gray-700 | #3a3a38 | Body copy |
| gray-900 | #1a1a18 | Strong headings on white |

### Retired Colors — DO NOT USE
- `#0058ED` — blue from old Figma prototype
- `#FFD700` — safety yellow
- Any teal, any light blue, any gradient
- Gold-800 and Gold-900 (read as brown)

### Typography Rules
| Element | Size | Weight | Color |
|---|---|---|---|
| Eyebrow | 12px | 500 | Gold-400 #eabb71, uppercase, letter-spacing 0.18em, margin-bottom 24px (DESIGN STANDARD — matches homepage hero "Manufacturing Performance Consulting" + section eyebrows like "The Manufacturing Moment") |
| H1 (light bg) | 44–64px | 700–800 | Navy-800 #183a61 |
| H1 (navy bg) | 44–64px | 700–800 | White #ffffff |
| H2 / Subhead | 30px | 700 | Navy-400 #4a6a8a — NEVER navy-800 |
| H3 | 22px | 600 | Navy-800 or gray-900 |
| Body | 16–18px | 300 | Gray-700 #3a3a38 — never pure black, never navy |
| Caption | 12px | 400 | Gray-400 #888884 |
| Nav links | 13px | 400 | Navy-800 on white header; gold on hover |

**Font:** Proxima Nova via Adobe Fonts (Typekit ID: dhv8kja)
**Fallback:** -apple-system, BlinkMacSystemFont, Segoe UI, Helvetica Neue, Arial, sans-serif

### Absolute Rules
1. H2/subheads = Navy-400 (#4a6a8a). Never Navy-800. Ever.
2. Gold is decorative at small scale — eyebrows, rules, borders, icons only. Not body text.
3. Body copy = Gray-700 (#3a3a38). Never pure black. Never navy.
4. Header bottom border = Gold-400 (#eabb71), 1px.
5. No dark mode unless explicitly requested.
6. No em dashes anywhere. Use periods or commas instead.
7. No gradients. No drop shadows. No border radius on cards or buttons.
8. Max content width = 1280px on ALL pages. Header, hero, controls, body content — all centered at 1280px.

---

## Homepage Card Sections — Data Strategy

### Case Study Cards (Section 6) and Insights Cards (Section 7)
Both sections display exactly 3 cards on the homepage. In the HTML prototype phase these are hardcoded placeholder content — curated manually by Gary/Method when real content is ready.

In the Faust.js build, Patrik replaces these with dynamic WPGraphQL queries:

**Case Studies — 3 most recent:**
```graphql
query FeaturedCaseStudies {
  caseStudies(first: 3, where: { orderby: { field: DATE, order: DESC }}) {
    nodes {
      title
      acfFields { headlineResult, industry, summary }
    }
  }
}
```

**Insights — 3 most recent:**
```graphql
query FeaturedInsights {
  posts(first: 3, where: { orderby: { field: DATE, order: DESC }}) {
    nodes {
      title
      excerpt
      categories { nodes { name } }
      slug
    }
  }
}
```

Once live, publishing a new case study or insight in WordPress automatically promotes it to the homepage. No code changes required.

---
| Page | File | Type | Status |
|---|---|---|---|
| Homepage | index.html | Static | ✅ Live v0.1.4 |
| Case Study Library | powers-case-study-library.html | Static | ✅ Live v0.1.4 |
| Operational Readiness | — | Static | Pending |
| Frontline Capability | — | Static | Pending |
| Equipment Reliability | — | Static | Pending |
| Supply Chain Performance | — | Static | Pending |
| Results / Case Studies | powers-case-study-library.html | Static HTML | ✅ Built |
| Insights / Blog | — | Dynamic (WPGraphQL) | Pending |
| About | — | Static | Pending |
| Contact | — | Static | Pending |
| 404 | — | Static | Pending |

---

## Component Index
| Component | File | Status |
|---|---|---|
| Header + Navigation | POWERS v2.html | ✅ Approved |
| Hero Section | POWERS v2.html | ✅ Approved |
| Section 2 — The Moment | POWERS v2.html | ✅ Built |
| Section 3 — What POWERS Does | POWERS v2.html | ✅ Built |
| Section 4 — Four Expertise Areas | POWERS v2.html | ✅ Built |
| Section 5 — How We Work | POWERS v2.html | ✅ Built |
| Section 6 — Results Entry Point | POWERS v2.html | ✅ Built |
| Section 7 — Insights Entry Point | POWERS v2.html | ✅ Built |
| Section 8 — Footer CTA | POWERS v2.html | ✅ Built |
| Footer | POWERS v2.html | ✅ Built |
| Case Study Library | powers-case-study-library.html | ✅ Built |
| Client Logo Bar | — | Pending |
| Insights Entry Cards | — | Pending |

---

## Header Spec (Approved)
- Background: white #ffffff
- Bottom border: gold-400 #eabb71, 1px solid
- Logo: powers-logo-refined-2026.png, height 57px
- Tagline: "Make Performance Stick" — 12px, italic, weight 300, navy-400, desktop only
- Nav items: Results (mega), About (mega), Insights (direct), Let's Talk (ghost button)
- Nav font: 13px, weight 400, navy-800, gold on hover
- Nav gap: 44px
- Let's Talk: ghost button, navy border + text, gold border + text on hover
- Search icon: 27×27px, far right, gray-100 border, navy-400 icon
- Mega menu top border: 1px gold #eabb71, flush with header bottom border (marginTop: -1px)
- Header height: 84px
- Max content width: 1280px
- Mobile: hamburger (40×40px), right-side sheet drawer

### Results Mega Menu
- Two-column layout, width 640px
- Left: Our Approach (link) | Expertise Areas (navy, 14px, weight 400 — not a link) | 4 indented sub-links | 
- Right: Industries Served (link) | Case Studies (link → powers-case-study-library.html)

### About Mega Menu
- Single column, width 260px: History | Leadership | Company News | Careers

---

## Hero Spec (Approved)
- Background: navy-900 #0d2442 + video overlay rgba(24,58,97,0.60)
- Video: https://www.thepowerscompany.com/wp-content/uploads/2025/10/POWERS-Banner-Video-2026.mp4
- Eyebrow: "MANUFACTURING PERFORMANCE CONSULTING" — gold, 12px, 0.18em tracked, uppercase, weight 500
- H1: "Turning Manufacturing Strategy into Measurable Results" — white, clamp(36px,4.2vw,56px), weight 800
- Body: "POWERS closes the gap between executive intent and shop floor performance. We build the systems, processes, and leadership behaviors that make improvement stick, no matter the conditions." — white 80% opacity, 18px, weight 300
- Primary CTA: "Start a Conversation" — solid gold #eabb71, navy text, no border radius, gold-600 on hover
- Secondary CTA: "See Our Results →" — white text, underline on hover → links to powers-case-study-library.html

---

## Homepage Section Architecture
| Section | Background | Key Elements |
|---|---|---|
| Hero | navy-900 + video overlay | Eyebrow, H1, subhead, dual CTAs |
| 2 — The Moment | navy-800 #183a61 | Centered column, white H2, gold eyebrow, no CTA |
| 3 — What POWERS Does | white | Centered, navy H2, body, gold bridge line |
| 4 — Expertise Areas | gray-50 #f5f5f3 | 4-col card grid, white cards, hover gold top border |
| 5 — How We Work | white (split) | Copy left, image placeholder right, pull quote navy block |
| 6 — Results Entry Point | navy-800 | 3 case study cards, "See All Case Studies →" → case studies page |
| 7 — Insights Entry Point | gray-50 | 3 article cards, "Visit Insights Hub →" |
| 8 — Footer CTA | navy-900 #0d2442 | Centered, gold "Start a Conversation" button |
| Footer | navy-900 #0d2442 | 4-col grid, logo, tagline, nav links, contact, legal bar |

---

## Footer Spec (Approved)
- Background: #0d2442
- Logo: powers-logo-refined-for-dark-backgrounds-2026.png, 140px wide
- Tagline: "Make Performance Stick." — gold #eabb71, 13px, letter-spacing 0.14em, weight 500
- 4 columns: Brand | Results | About | Let's Talk (Contact)
- Contact col: phone, email, address, ghost button "Start a Conversation"
- Legal bar: 1px gold rule at 20% opacity, copyright + links at white 40% opacity
- Max width: 1280px

---

## Case Study Library Page Spec
- File: powers-case-study-library.html
- Header + Footer: shared React components, identical to homepage
- No standalone header — uses site header
- Hero: eyebrow "Case Studies" (11px, weight 500, 0.18em, gold), H1 "The Work Speaks for Itself.", subhead at 15px weight 300 white 70%
- Stats row: removed entirely
- Search/filter bar: sticky below header (top: 84px), max-width 1280px inner wrapper
- Content width: 1280px max on all sections — hero, controls bar, card grid
- Filtering/search/sort: all functional, untouched
- Fonts: Proxima Nova throughout (Playfair Display and DM Sans removed)
- Brand tokens: corrected to POWERS spec

---

## Key Design Decisions
- No dark mode toggle in header (removed per reset brief)
- Search icon is placeholder — no modal built yet
- "Let's Talk" is ghost button, not plain text link
- Mega menus use marginTop: -1px to sit flush against header gold border
- Value chain strip removed from hero bottom
- H2 color in section headlines uses navy-800 (these are H3-level section titles, not true H2 subheads)
- All pages use max-width: 1280px for content — established as global rule
- No em dashes anywhere in copy
- No gradients, no drop shadows, no border radius on cards or buttons

---

## Version Log

## Version Log

### v0.1.15 — 2026-05-05
**Deploy 15 — History + Careers pages built**
- history.html: full content build per approved prompt. Hero (navy, eyebrow + H1 + subhead), Section 1 Where It Started (white, narrow column), Section 2 How We Evolved (gray-50, narrow column), Section 3 A New Chapter (white, split layout with image placeholder), Section 4 The Constants (navy, narrow column with quote list — 5 founding principles separated by gold rules at 30% opacity), Section 5 CTA (navy-900, "Read the Case Studies" link → case-studies.html)
- careers.html: full content build per approved prompt. Hero (navy), Section 1 What The Work Actually Looks Like (white, narrow column), Section 2 Who Thrives Here (gray-50, split layout with image placeholder), Section 3 What POWERS Offers (white, narrow column), Section 4 CTA (navy-900, gold "View Open Positions" button → placeholder #)
- Eyebrow size: 12px (matches homepage hero "Manufacturing Performance Consulting" and "The Manufacturing Moment" — the design standard). CLAUDE.md typography table corrected from 11px to 12px in same deploy.
- Image placeholders use striped SVG pattern with monospace label per default aesthetic, on navy background to read as professional/dark
- Section pattern reused: outer element no horizontal padding, inner wrapper max-width 1280px with padding 96px 48px (per global rule v0.1.11)
- Fully responsive: split layouts collapse to single column at 1023px; section padding tightens to 64px 24px at 767px
- Header + footer: existing React components unchanged on both pages
- Meta descriptions added to both pages

**Open questions to confirm:**
1. Image placeholders: confirm replacement images for (a) Atlanta skyline / 1801 Peachtree NE on history Section 3, and (b) manufacturing floor / consultant on shift on careers Section 2.
2. Careers CTA button "View Open Positions" wired to placeholder href="#" — pending client direction on LinkedIn Jobs vs iSolveHire destination.

### v0.1.14 — 2026-05-04
**Deploy 14**
- Hero "Start a Conversation" CTA wired to contact.html
- Hero "See Our Results →" underline on hover removed, arrow animation retained
- CLAUDE.md updated

### v0.1.13 — 2026-05-04
**Deploy 13**
- Root Cause section: "Frontline" → "Front Line" (two words) in headline
- Root Cause section: "frontline capability" → "frontline leadership capability" in body
- Root Cause section: link added "Build the Frontline Leaders That Will Make Performance Stick →" → frontline-leadership.html
- "See what that looks like in practice →" capitalized to initial caps
- Sitewide link audit passed

### v0.1.12 — 2026-05-04
**Deploy 12**
- "Visit the Insights Hub →" wired to insights.html
- "Start a Conversation" button in Footer CTA wired to contact.html
- Sitewide link audit passed: all 14 files clean

### v0.1.11 — 2026-05-04
**Deploy 11 — Full site nav stabilization**
- case-studies.html inline nav replaced with site-nav.jsx content — all menu links now work from case studies page
- Responsive CSS class names unified: nav-desktop / nav-mobile / nav-tagline across all pages
- About mega menu wired on homepage: History, Leadership, Company News, Careers
- All root-relative hrefs (/page.html) corrected to plain relative (page.html) across all 14 files
- Skeleton page hero: double-padding removed, outer section has no horizontal padding, inner wrapper is max-width 1280px with padding 96px 48px — matches header/footer column exactly
- case-studies.html hero depth matched to skeleton pages: min-height 320px, padding 96px 48px
- Sitewide link audit passed: zero broken links, zero root-relative paths, zero unwired nav items across all 14 files

### Global rules added to spec:
- All pages use plain relative hrefs (no leading slash) — works in preview and on Netlify
- Hero sections on all pages: outer element has NO horizontal padding. Inner wrapper: max-width 1280px, margin 0 auto, padding 96px 48px
- Sitewide link audit runs before every deploy
- Shared nav (site-nav.jsx) is the single source of truth for skeleton pages — never duplicate inline

### v0.1.10 — 2026-05-04
**Deploy 10 — Full site nav build**
- Homepage renamed from POWERS v2.html to index.html
- powers-case-study-library.html renamed to case-studies.html, all references updated
- 12 skeleton pages built: our-approach, operational-readiness, frontline-leadership, equipment-reliability, supply-chain, industries-served, insights, history, leadership, company-news, careers, contact
- All internal nav links wired with plain relative paths (no leading slash) — works in preview and on Netlify
- About mega menu wired: History, Leadership, Company News, Careers
- Expertise sub-links wired to respective skeleton pages
- Expertise cards on homepage wired to respective pages
- Full sitewide link audit: all 14 files clean, zero broken or root-relative links
- site-nav.jsx shared component created for skeleton pages

### v0.1.9 — 2026-05-04
**Deploy 9**
- New section added: "The Root Cause" — white bg, centered column, between The Moment and How We're Different
- "How We're Different" section background changed from white to navy-800 (#183a61), text inverted to white/gold
- Manufacturing floor placeholder replaced with real photography (POWERS Homepage Placeholder 1280 x 960.png)
- 1px gold #eabb71 top border added to footer
- Homepage section architecture updated in CLAUDE.md

### v0.1.8 — 2026-05-04
**Deploy 8 — Best practices audit**
- All logo links set to index.html across all pages (header, footer, mobile drawer)
- Meta description + OG tags added to both pages
- Dead CSS removed from case study page (.site-header, .logo, .header-label)
- Card border-radius removed (brand spec: no border radius on cards)
- Card box-shadow removed (brand spec: no drop shadows)
- Card hover translateY removed
- Old gold rgba(201,168,76) corrected to rgba(234,187,113) throughout case study page
- Decorative hero circle pseudo-elements removed from case study page (off-brand)
- CLAUDE.md to be updated automatically on every deploy going forward

### v0.1.7 — 2026-05-04
**Deploy 7**
- Fixed controls bar width on case study page — padding moved entirely to inner wrapper, outer element padding set to 0, so 1280px max-width correctly aligns with header and hero content

### v0.1.6 — 2026-05-04
**Deploy 6**
- Fixed hero and controls bar content width on case study page — padding moved from outer elements to inner wrappers so max-width 1280px correctly constrains content to match header column

### v0.1.5 — 2026-05-04
**Deploy 5**
- Content width standardized to 1280px across all sections on case study page (hero inner wrapper, controls bar inner wrapper, card grid)
- CLAUDE.md fully updated with complete project state

### v0.1.4 — 2026-05-04
**Deploy 4**
- Full homepage section architecture built: Sections 2-8 (The Moment, What POWERS Does, Expertise Areas, How We Work, Results Entry Point, Insights Entry Point, Footer CTA)
- Footer added with 4-column layout, logo, tagline, nav links, contact info, legal bar
- Case Study Library page integrated (powers-case-study-library.html) — brand tokens corrected, header/footer added, stats row removed
- Case Studies wired across all nav locations (mega menu, mobile drawer, footer, CTAs)
- "See Our Results" hero CTA linked to case studies page
- "See All Case Studies" linked to case studies page
- Eyebrow weight corrected on case study page (600 → 500)
- Content width standardized to 1280px across all sections on case study page

### v0.1.3 — 2026-05-04
**Deploy 3**
- Tagline font changed to sentence case, no letter-spacing
- Tagline size increased to 12px
- Tagline set to italic

### v0.1.2 — 2026-05-04
**Deploy 2**
- "Expertise Areas" label in Results mega menu changed to match nav link treatment (14px, weight 400, navy) — no longer gold uppercase label
- Same fix applied to mobile drawer and static demo panel
- Duplicate CSS properties cleaned up

### v0.1.1 — 2026-05-04
**Deploy 1**
- Nav font reduced to 13px
- Nav item gap increased to 44px
- Search button added to header far right (27×27px)
- Hero body copy updated to approved text
- Tagline updated to "Make Performance Stick"
- Logo increased to 57px
- Header height increased to 84px
- Mega menu gold top border restored (1px #eabb71), flush with header
- Value chain strip removed from hero bottom
- Reindustrialization Bridge section added
- Strategy to Execution section added (pending image approval)

### v0.1.0 — 2026-05-04
**Initial build**
- Header component: white bg, gold border, logo, tagline, nav, mega menus (Results + About), ghost CTA, search button
- Hero section: navy bg, video, overlay, eyebrow, H1, subhead, dual CTAs
- Reindustrialization Bridge: navy-800, single statement
- Strategy to Execution: two alternating image-text blocks
- Value Chain Diagram: in progress

**Key decisions:**
- Full reset from v1 (navy header) to v2 (white header) per client brief correction
- Nav font: 13px (reduced from 14px)
- Logo height: 57px
- Header height: 84px
- Tagline: "Make Performance Stick"
- Mega menu gold top border flush with header bottom border
