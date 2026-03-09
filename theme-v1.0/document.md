# SHOPIFY HORIZON THEME — CLAUDE CODE MASTER INSTRUCTIONS
# Project: HydroLid — Framer to Horizon Homepage Rebuild
# Developer: 10-year Shopify expert — skip basics, deliver precision
# Read this ENTIRE file before doing ANYTHING. No exceptions.

---

## ⚠️ CRITICAL RULES — READ BEFORE ANYTHING ELSE

### RULE 0 — THREE SOURCES OF TRUTH (priority order)

Every implementation decision must be cross-referenced against all three:

  PRIORITY 1 — html-homepage/ folder (highest authority for visual output)
    → This folder contains the HTML implementation of the HydroLid homepage
    → READ the HTML files here BEFORE touching any Shopify Liquid
    → This is the pixel-accurate visual target — Shopify output must match this
    → Extract from it: exact class names, exact markup structure, exact CSS values,
      exact font declarations, exact spacing, exact color hex/rgba values
    → When HTML source and Framer design differ → HTML folder wins

  PRIORITY 2 — Framer design (https://wondrous-beaver-851520.framer.app)
    → Use for layout intent, section order, interaction design, content
    → When HTML folder has the implementation → Framer confirms the design intent

  PRIORITY 3 — This CLAUDE.md master instructions
    → Architecture rules, schema patterns, Shopify-specific decisions
    → Horizon 3.4.0 file structure and modification rules

---

### RULE 1 — TYPOGRAPHY: MASTER INSTRUCTIONS ARE SOURCE OF TRUTH

Typography is defined in this CLAUDE.md — do not override from Framer HTML or
assumptions. The design system font specifications below are final.

EYEBROW / LABEL TEXT (e.g. "Mission Ready", "New Arrivals", "The System"):
  font-family: "Great Sejagad Regular", "Great Sejagad Regular Placeholder", sans-serif
  → This is the confirmed eyebrow font — use this EXACTLY, character for character
  → Do NOT use: Georgia, serif
  → Do NOT use any other font for eyebrow/label elements
  → "Great Sejagad Regular Placeholder" is the loading placeholder — always include it
  → This font file must be uploaded to assets/ as a .woff2 or .woff file
  → Load via @font-face in assets/custom-fonts.css (create if it doesn't exist)

ALL OTHER TEXT ELEMENTS — read from html-homepage/ folder:
  For each element type below, open the corresponding HTML file and find the
  exact computed font-family in the CSS:
  - H1 hero heading      → read html-homepage/ source
  - H2 section headings  → read html-homepage/ source
  - H3 subheadings       → read html-homepage/ source
  - H4 feature items     → read html-homepage/ source
  - Body paragraph       → read html-homepage/ source
  - Button text          → read html-homepage/ source
  - Price text           → read html-homepage/ source
  - Navigation links     → read html-homepage/ source

FONT LOADING RULE:
  - Custom fonts (not Google Fonts, not system fonts) → upload .woff2 to assets/
  - Create assets/custom-fonts.css with all @font-face declarations
  - Load custom-fonts.css in layout/theme.liquid (append, do not replace existing)
  - Use font_picker settings in settings_schema.json for fonts the store owner
    should be able to change
  - Eyebrow font (Great Sejagad Regular) is a design system font — not user-changeable
    → load it via assets/custom-fonts.css directly, not font_picker

---

### RULE 2 — HORIZON 3.4.0 ARCHITECTURE (Critical — different from Dawn)

This project uses Shopify Horizon version 3.4.0.
Horizon has a different architecture than Dawn. Rules that apply to Dawn
do NOT automatically apply here.

SECTION FILE IDENTIFICATION:
  Horizon 3.4.0 may use different file names than standard Shopify themes.
  Before assuming a section exists, READ the sections/ directory.
  Known Horizon-specific section names that differ from Dawn:
  - Announcement bar  → may be "header-announcements.liquid" OR "marquee.liquid"
                         NOT "announcement-bar.liquid" (that's Dawn)
  - Check the actual file — do not assume Dawn naming conventions

MANDATORY BEFORE MODIFYING ANY SECTION:
  1. READ sections/ directory listing — confirm the exact file name
  2. READ the full file — understand its complete existing architecture
  3. READ any snippets it renders — understand dependencies
  4. READ assets/ for any CSS already loaded by this section
  5. Only then plan your modifications

MODIFICATION RULES (non-negotiable):
  ✅ DO:
  - Read the ENTIRE existing section file first
  - Identify exactly what schema settings and blocks already exist
  - ADD new settings to the existing schema — append to settings array only
  - ADD new block types to blocks array only if needed
  - ADD new markup wrapped in conditional Liquid tags
    ({% if section.settings.enable_ticker %}...{% endif %})
    so existing functionality is never broken
  - ADD a new scoped CSS file for visual changes
    → naming: assets/section-custom-[section-name]-hydrolid.css
    → load it at top of section: {{ 'section-custom-[name]-hydrolid.css' | asset_url | stylesheet_tag }}
  - Preserve EVERY existing:
    → Liquid tags and filters
    → Block loop patterns ({% for block in section.blocks %})
    → content_for rendering patterns
    → {% render %} snippet calls
    → Schema settings (every existing id must remain)
    → Schema block types
    → CSS class names referenced by existing JS

  ❌ NEVER:
  - Rewrite the entire section file
  - Delete any existing schema setting (even unused-looking ones)
  - Remove any existing block type from schema
  - Break content_for_header or content_for_layout
  - Change existing section CSS class names
  - Remove any existing {% render %} snippet call
  - Replace the entire {% schema %} block — only append inside it
  - Use Dawn section names if Horizon uses a different name

MODIFICATION WORKFLOW (follow every time):
  Step 1: List sections/ directory — confirm exact file name
  Step 2: Read full file — output a summary:
          "This section has [N] settings: [list ids]"
          "It has [N] block types: [list types]"
          "It renders these snippets: [list]"
          "It loads these CSS files: [list]"
  Step 3: State minimum changes needed to match the html-homepage/ reference
  Step 4: Show a precise diff plan:
          "ADD to schema settings array: [new setting ids]"
          "ADD markup: [describe what, after which existing line]"
          "CREATE: assets/section-custom-[name]-hydrolid.css"
  Step 5: Wait for developer approval → then and only then write code
  Step 6: Output ONLY changed lines with ±5 lines of surrounding context
          (NOT the entire file rewritten)

---

### RULE 3 — IMPLEMENTATION REFERENCE HIERARCHY

When implementing any section, follow this lookup order:

  1. OPEN html-homepage/ folder → find this section's HTML markup
     → Use its exact class names, element structure, CSS values
  2. CROSS-CHECK against Framer design URL for layout intent
  3. APPLY Horizon architecture rules from this file
  4. REUSE existing Horizon snippets (card-product, card-article, image, etc.)
  5. USE color variables from snippets/color-schemes.liquid
  6. USE font variables from snippets/theme-styles-variables.liquid
     EXCEPT where this CLAUDE.md explicitly overrides (e.g. eyebrow font)

---

### RULE 4 — CONFIRM BEFORE CODING

For every section (new or modified), before writing any code:
  1. State: section name, target file, action (modify/create)
  2. Show: html-homepage/ reference summary for this section
  3. Show: precise diff plan — what will be added/changed and where
  4. Wait for developer approval
  5. Write code — output only changed lines, not full file rewrites
  6. Ask: "Section [N] done — confirmed? Move to Section [N+1]?"

---

## MANDATORY FILE READING ORDER — EVERY SINGLE SESSION

Before writing ONE line of code, read these files in this exact order.
After reading, confirm each file has been read and summarize what you found.

### STEP 1 — Color & Variable Systems (CRITICAL FIRST)

READ: snippets/color-schemes.liquid
  → List every CSS variable defined per scheme class
  → Note exact variable names: --color-background, --color-foreground,
    --color-accent, --color-button, --color-button-label, etc.
  → Note how scheme classes are named (.color-scheme-1, scheme-2, etc.)
  → This is the ONLY source of truth for colors — never hardcode hex values

READ: snippets/theme-styles-variables.liquid
  → List ALL CSS custom properties for:
    - Heading font: family, size scale, weight, line-height, letter-spacing
    - Body font: family, size, weight, line-height
    - Nav/label font: family, size, weight, text-transform
    - Button styles: radius, padding, font-size, font-family, text-transform
    - Border radius values
    - Spacing scale
  → Every typography variable found here MUST be used in custom sections

READ: assets/base.css
  → Extract: breakpoints (exact px values for mobile/tablet/desktop)
  → Extract: .page-width max-width value and padding
  → Extract: grid system classes and column structure
  → Extract: all utility classes (.visually-hidden, .button, .link, etc.)
  → Extract: existing animation/transition values
  → This defines Horizon's design system — match it everywhere

### STEP 2 — Theme Layout & Config

READ: layout/theme.liquid
  → Note how stylesheets are loaded (link tags vs asset_url)
  → Note content_for_header and content_for_layout placement
  → Note any global JS dependencies
  → Note the body class structure

READ: config/settings_schema.json
  → List all color scheme definitions (ids, labels)
  → List all font picker setting IDs
  → Note global settings (button style, badge position, etc.)

READ: config/settings_data.json
  → Note currently active font selections
  → Note currently active color scheme values
  → This tells you what the store owner has already configured

### STEP 3 — Homepage Structure

READ: templates/index.json
  → List EVERY section currently on the homepage in order
  → Note section IDs (the random string keys)
  → Note any existing block configurations
  → This is the source of truth for what is already on the homepage

### STEP 4 — Full Section Audit

READ ALL files in: sections/
  → For EACH section, note: file name, schema "name", available settings,
    available block types, whether it has presets
  → Build a complete list of what Horizon provides natively
  → Mark which sections can be reused for this project (see Section Map below)

### STEP 5 — Full Snippet Audit

READ ALL files in: snippets/
  → Note: card-product.liquid (product card markup + classes)
  → Note: card-article.liquid (blog card markup)
  → Note: image.liquid or similar (how Horizon renders responsive images)
  → Note: icon-*.liquid files available
  → Note: price.liquid (how prices are rendered)
  → Note: pagination.liquid
  → ALWAYS use these — never rewrite functionality that already exists

---

## HYDROLID HOMEPAGE — COMPLETE SECTION MAP
## (Analyze Framer: https://wondrous-beaver-851520.framer.app)

This is the full page top-to-bottom. Confirm section order matches Framer
before touching templates/index.json.

---

### SECTION 01 — Announcement Bar Ticker
═══════════════════════════════════════════════════════
TARGET FILE:    sections/announcement-bar.liquid
NEW CSS FILE:   assets/section-custom-announcement-ticker.css
ACTION:         MODIFY existing section — DO NOT rewrite entire file
═══════════════════════════════════════════════════════

FRAMER DESIGN SPEC:
  Layout:
    - Full-width bar, fixed at very top of page (above header)
    - Height: approximately 40-44px
    - Background: dark (near black or very dark navy — confirm from HTML source)
    - Content: single continuous horizontal scrolling ticker (marquee)
    - Scroll direction: left (content moves right → left continuously)
    - Loop: infinite, seamless (no gap between repeats)

  Ticker Content (repeating sequence):
    - Message 1: "HydroLid INT Now Compatible w/ Nalgene"
    - Separator: SVG icon (small logo/star/flame — confirm from Framer HTML source)
    - Message 2: "HydroLid EXT Now Compatible With YETI Ramblers!"
    - Separator: SVG icon (same as above)
    - Then repeats from Message 1 (seamless loop)

  Typography (confirm each from Framer HTML source — do not assume):
    - Font-family: inspect Framer rendered HTML source for this element
    - Font-size: inspect (approximately 12-13px — confirm from source)
    - Font-weight: inspect (approximately 500-600 — confirm from source)
    - Letter-spacing: inspect (approximately 0.05-0.08em — confirm from source)
    - Text-transform: inspect (uppercase or normal — confirm from source)
    - Color: light/white — confirm exact value from HTML source

  Animation:
    - CSS @keyframes marquee — translateX(0) to translateX(-50%)
    - Duration: approximately 20-25 seconds (make this a schema setting)
    - Timing: linear
    - No pause on hover (continuous operational feel)
    - The track must contain content duplicated exactly once
      so when it scrolls -50%, it loops back to identical start = seamless

PRE-TASK CHECKLIST (complete all before writing code):
  [ ] Read sections/announcement-bar.liquid fully
  [ ] List every existing schema setting found (id, type, label)
  [ ] List every existing block type found
  [ ] List existing CSS classes used in the file
  [ ] Read assets/base.css — note any existing announcement bar styles
  [ ] Check snippets/ for any icon snippet used in announcement bar
  [ ] Inspect Framer HTML source for exact font-family on ticker text
  [ ] Inspect Framer HTML source for exact background color of bar
  [ ] Inspect Framer HTML source for the separator SVG markup

SCHEMA CHANGES (append to existing schema — do not replace):
  ADD these settings to the existing settings array:
  1. header: { "content": "Ticker settings" }
  2. checkbox: enable_ticker
     - id: "enable_ticker"
     - label: "Enable scrolling ticker"
     - default: true
  3. range: ticker_speed
     - id: "ticker_speed"
     - label: "Ticker speed (seconds)"
     - min: 10, max: 60, step: 5, unit: "s", default: 20
  4. text: ticker_message_1
     - id: "ticker_message_1"
     - label: "Ticker message 1"
     - default: "HydroLid INT Now Compatible w/ Nalgene"
  5. text: ticker_message_2
     - id: "ticker_message_2"
     - label: "Ticker message 2"
     - default: "HydroLid EXT Now Compatible With YETI Ramblers!"
  6. image_picker: ticker_separator_icon
     - id: "ticker_separator_icon"
     - label: "Separator icon (optional)"

MARKUP CHANGES (targeted insertion only):
  - Locate the existing announcement bar content area in the liquid file
  - WRAP existing content in: {% if section.settings.enable_ticker == false %}
  - ADD the ticker markup as an {% else %} block:

  TICKER HTML STRUCTURE:
  ```liquid
  {% if section.settings.enable_ticker %}
    <div class="hydrolid-ticker-wrap" aria-label="Announcements">
      <div class="hydrolid-ticker-track"
           style="animation-duration: {{ section.settings.ticker_speed }}s;">
        {%- comment -%}Duplicate content exactly once for seamless loop{%- endcomment -%}
        {%- for i in (1..2) -%}
          <span class="hydrolid-ticker-item">
            {{- section.settings.ticker_message_1 -}}
          </span>
          {%- if section.settings.ticker_separator_icon != blank -%}
            <span class="hydrolid-ticker-sep" aria-hidden="true">
              {{- section.settings.ticker_separator_icon
                  | image_url: width: 20
                  | image_tag: loading: 'eager', class: 'ticker-icon' -}}
            </span>
          {%- else -%}
            <span class="hydrolid-ticker-sep" aria-hidden="true">✦</span>
          {%- endif -%}
          <span class="hydrolid-ticker-item">
            {{- section.settings.ticker_message_2 -}}
          </span>
          {%- if section.settings.ticker_separator_icon != blank -%}
            <span class="hydrolid-ticker-sep" aria-hidden="true">
              {{- section.settings.ticker_separator_icon
                  | image_url: width: 20
                  | image_tag: loading: 'eager', class: 'ticker-icon' -}}
            </span>
          {%- else -%}
            <span class="hydrolid-ticker-sep" aria-hidden="true">✦</span>
          {%- endif -%}
        {%- endfor -%}
      </div>
    </div>
  {% else %}
    {%- comment -%}Existing Horizon announcement bar markup stays here untouched{%- endcomment -%}
    [EXISTING HORIZON MARKUP PRESERVED AS-IS]
  {% endif %}
  ```

CSS FILE: assets/section-custom-announcement-ticker.css
  Rules:
  - Scope ALL rules to .announcement-bar (the existing Horizon wrapper class)
    to avoid affecting anything outside this section
  - .hydrolid-ticker-wrap:
      overflow: hidden
      width: 100%
      height: 100% (inherits bar height from existing Horizon styles)
      display: flex
      align-items: center
  - .hydrolid-ticker-track:
      display: flex
      align-items: center
      gap: [confirm from Framer — approx 32-48px between items]
      white-space: nowrap
      will-change: transform
      animation: hydrolid-marquee linear infinite
      width: max-content (critical — lets content size itself)
  - @keyframes hydrolid-marquee:
      from { transform: translateX(0); }
      to   { transform: translateX(-50%); }
      (−50% works because track content is duplicated exactly once)
  - .hydrolid-ticker-item:
      font-family: [value from Framer HTML source — see Rule 1]
      font-size: [value from Framer HTML source]
      font-weight: [value from Framer HTML source]
      letter-spacing: [value from Framer HTML source]
      color: var(--color-foreground) [or confirm from color-schemes.liquid]
  - .hydrolid-ticker-sep:
      display: inline-flex
      align-items: center
      margin: 0 [gap value]px
      opacity: 0.6
  - .ticker-icon:
      width: 16-20px
      height: auto
      vertical-align: middle
  - @media (prefers-reduced-motion: reduce):
      .hydrolid-ticker-track { animation: none; }

LINK THE CSS at the very top of announcement-bar.liquid (above existing stylesheet tags):
  {{ 'section-custom-announcement-ticker.css' | asset_url | stylesheet_tag }}

DELIVERABLE FORMAT:
  Show output as two clearly labelled code blocks:
  1. "CHANGES TO: sections/announcement-bar.liquid"
     → Show ONLY the lines being added/changed with surrounding context (±5 lines)
     → NOT the entire file rewritten
  2. "NEW FILE: assets/section-custom-announcement-ticker.css"
     → Full CSS file content

  Then ask: "Confirmed — shall I move to Section 02 (Header)?"

---

### SECTION 02 — Header / Navigation
FRAMER DESIGN:
  - Logo: HydroLid wordmark SVG (left aligned)
  - Nav links: Shop | About us | Care | Support | Info
  - Sticky or transparent on hero
  - Clean minimal bar, no mega menu visible at top level
HORIZON ACTION: USE sections/header.liquid
  - Upload logo via theme editor (image_picker setting)
  - Create matching nav menu in Shopify admin → Navigation
  - Menu handle: "main-menu"
  - DO NOT rewrite header — use Horizon's existing header section
FILE TO EDIT: sections/header.liquid (CSS adjustments only if needed)

---

### SECTION 03 — Hero Banner (CUSTOM — Complex)
FRAMER DESIGN (DESKTOP):
  - Full-viewport-height background image (dark outdoor/fire scene)
  - Image: framerusercontent.com/images/35bnAEaEVcpQPJrWCWWekD1J0Q0.png
  - Dark overlay on image
  - Content positioned LEFT side, vertically centered:
    * Eyebrow label: "Mission Ready" — small caps, letter-spaced, muted color
    * H1: "Ditch the Bladder. Upgrade Your Hydration." — large display font, bold
    * Paragraph: "Born in wildfire operations. Engineered for cold water access in extreme heat."
    * Two CTA buttons side by side:
      - Primary: "Shop Hydrolid" (solid, accent color)
      - Secondary: "Watch story" (ghost/outline style)
  - Product card: ABSOLUTELY POSITIONED — bottom-right corner of the hero
    * Product image thumbnail
    * Product name: "Field & Fire Kit Bundle..."
    * Price: $139.00 (bold)
    * Star rating: 5.0
    * Card has rounded corners, white/light background, subtle shadow
FRAMER DESIGN (MOBILE):
  - Different image: framerusercontent.com/images/ORPWWj5I3q0DFaXQM3Yli3L4.png
  - Stacked layout: image above, content below (NOT overlay)
  - Product card hidden or repositioned below content
HORIZON ACTION: CREATE custom section
FILE: sections/custom-hero.liquid
CSS: assets/section-custom-hero.css
SCHEMA SETTINGS NEEDED:
  - image_picker: desktop_image
  - image_picker: mobile_image
  - text: eyebrow_text (default: "Mission Ready")
  - textarea: heading (default: "Ditch the Bladder. Upgrade Your Hydration.")
  - textarea: subheading
  - text: primary_cta_label
  - url: primary_cta_url
  - text: secondary_cta_label
  - url: secondary_cta_url
  - checkbox: show_product_card
  - product: featured_product (for the bottom-right card)
  - color_scheme
  - range: overlay_opacity (0-90, step 5, default: 40)
  - range: padding_top, padding_bottom
CRITICAL LAYOUT NOTES:
  - Hero wrapper: position: relative, min-height: 100vh (desktop)
  - Background image: position: absolute, inset: 0, object-fit: cover, z-index: 0
  - Content wrapper: position: relative, z-index: 1, left-aligned, max-width: 600px
  - Product card: position: absolute, bottom: 40px, right: 40px, z-index: 2
  - On mobile: static layout, no absolute positioning, image as <img> not background

---

### SECTION 04 — New Arrivals / Featured Collection
FRAMER DESIGN:
  - Section label: "New Arrivals" (small eyebrow)
  - Section heading: "Built for the Field." (large H2)
  - "View All" link — top right aligned
  - Product grid: 4 products in a row
    * Each card: square product image, star rating (5.0), product name,
      price ($139.00 bold), "Add To Cart" button below
  - Clean white/light background
HORIZON ACTION: USE sections/featured-collection.liquid
  - Horizon's featured-collection handles product grid natively
  - Assign a collection via schema (collection picker)
  - Set columns to 4
  - Reuse snippets/card-product.liquid — DO NOT rewrite product cards
  - CUSTOMIZE: eyebrow label setting if not present in schema
  - CUSTOMIZE: "View All" button alignment CSS if needed
FILE TO EDIT: sections/featured-collection.liquid (schema + minor CSS)
CSS: assets/section-custom-featured-collection-hydrolid.css (eyebrow + layout tweaks only)

---

### SECTION 05 — The System (Image With Text — Split Layout)
FRAMER DESIGN:
  - Section label: "The System" (eyebrow)
  - Heading: "Not a Lid. A Complete Upgrade System."
  - Subheading: "HydroLid transforms your bottle into a hands-free hydration system."
  - Feature bullet list (4 items, each H4):
    * Modular hose system
    * Quick-access drinking without removing your pack
    * Compatible with insulated bottles
    * Built for rugged use
  - CTA button: "See the System"
  - Right side: large product/lifestyle image
  - Layout: text LEFT, image RIGHT (desktop)
  - Mobile: stacked (image above text)
  - Background: dark (near black)
HORIZON ACTION: USE sections/image-with-text.liquid
  - Horizon's image-with-text supports text + image split layout
  - Add blocks for feature bullet items using block type
  - CUSTOMIZE: eyebrow text setting, feature list block type
  - Color scheme: use dark scheme
FILE TO EDIT: sections/image-with-text.liquid
CSS: assets/section-custom-image-with-text-hydrolid.css (bullet styling, eyebrow)

---

### SECTION 06 — Pain Points / Problem Section
FRAMER DESIGN:
  - Top line: "And in extreme heat? They warm up fast." (italic or styled heading)
  - Main heading: "Hydration Bladders Add Bulk. Add Hassle. Add Complexity."
  - Three columns, each with:
    * Icon or heading label
    * H4 problem title: "Hard to clean" / "Heavy when full" / "Slows access"
    * Paragraph description
  - Dark background
HORIZON ACTION: USE sections/multicolumn.liquid
  - 3 blocks, each with heading + text
  - Add eyebrow/intro text setting if not present
  - Color scheme: dark
  - CUSTOMIZE: column icon support if multicolumn doesn't have it
FILE TO EDIT: sections/multicolumn.liquid (add intro heading block to schema if missing)
CSS: assets/section-custom-multicolumn-problems.css

---

### SECTION 07 — Audience Split Banners (CUSTOM)
FRAMER DESIGN:
  - Two side-by-side full-height image banners (50/50 split)
  - LEFT BANNER:
    * Background image: wildland firefighter scene
    * Dark overlay
    * Top label: "For Wildland Firefighters" (H2)
    * Subtext: "Operational hydration that won't fail when the heat hits triple digits..."
    * CTA: "Shop now" button (bottom left of card)
  - RIGHT BANNER:
    * Background image: hiking/nature scene
    * Dark overlay
    * Top label: "For Hikers & Backpackers" (H2)
    * Subtext: "Ditch the heavy bladder. Upgrade your trail experience..."
    * CTA: "Shop now" button
  - Both banners equal height, side by side on desktop
  - Mobile: stacked vertically, full width each
HORIZON ACTION: CREATE custom section
  (Horizon's image-banner.liquid is SINGLE banner — cannot do 50/50 split natively)
FILE: sections/custom-audience-split.liquid
CSS: assets/section-custom-audience-split.css
SCHEMA SETTINGS:
  - image_picker: left_image
  - text: left_heading
  - textarea: left_subtext
  - text: left_cta_label
  - url: left_cta_url
  - image_picker: right_image
  - text: right_heading
  - textarea: right_subtext
  - text: right_cta_label
  - url: right_cta_url
  - range: overlay_opacity
  - range: section_height (400-800px)
  - color_scheme
  - range: padding_top, padding_bottom
LAYOUT NOTES:
  - Wrapper: display grid, grid-template-columns: 1fr 1fr (desktop)
  - Each panel: position relative, overflow hidden
  - Background image: position absolute, inset 0, object-fit cover
  - Content: position relative, z-index 1, padding: 40-60px
  - Mobile: grid-template-columns: 1fr (stacked)

---

### SECTION 08 — Testimonials
FRAMER DESIGN:
  - Label: "The System" (eyebrow)
  - Heading: "Tested Where Failure Isn't an Option."
  - Rating badge: "4.9/5 Rating"
  - 3 testimonial cards in a row (or slider on mobile):
    * Each card: quote text, reviewer name (H6 bold), role/title, "Verified Buyer" badge
    * Card style: dark background, quote text white, clean card border
  - Background: near-black or very dark
HORIZON ACTION: USE sections/testimonials.liquid
  - Horizon testimonials section supports blocks with quote + author
  - Check if "role/title" field exists in schema — add if not
  - Add "rating_badge" text setting for the "4.9/5 Rating" display
  - Color scheme: dark
FILE TO EDIT: sections/testimonials.liquid
CSS: assets/section-custom-testimonials-hydrolid.css

---

### SECTION 09 — Origin Story / Timeline (CUSTOM)
FRAMER DESIGN:
  - Label: "Our Origin" (eyebrow)
  - Heading: "Born in Fire. Patented for Performance."
  - Horizontal 5-step timeline:
    * Step 1 — "The Spark" / "100° Wildfire Conditions" / description
    * Step 2 — "The Failure" / "Bladder Water Overheats" / description
    * Step 3 — "The Solution" / "Insulated Bottle Workaround" / description
    * Step 4 — "The Build" / "Field Tested Prototype" / description
    * Step 5 — "The Result" / "Patent Secured" / description
  - Large background image behind timeline
  - Timeline steps connected by line/dots
  - Video element below or behind (background video)
HORIZON ACTION: CREATE custom section
  (No equivalent in Horizon — this is a unique brand storytelling section)
FILE: sections/custom-origin-timeline.liquid
CSS: assets/section-custom-origin-timeline.css
SCHEMA SETTINGS:
  - image_picker: background_image
  - video: background_video (optional overlay)
  - text: eyebrow
  - textarea: heading
  - color_scheme
  - range: padding_top, padding_bottom
  BLOCKS (type: "step", max: 8):
    - text: step_label (e.g. "The Spark")
    - text: step_title (e.g. "100° Wildfire Conditions")
    - textarea: step_description
LAYOUT NOTES:
  - Desktop: flex row, steps equally spaced, connecting line via ::before pseudo
  - Mobile: flex column, vertical timeline
  - Step connector: absolute positioned horizontal line at midpoint of step icons

---

### SECTION 10 — The Gear / Second Products Grid
FRAMER DESIGN:
  - Label: "The Gear" (eyebrow)
  - Heading: "Build Your Hydration Setup"
  - "View All" link top right
  - 4 products grid — same card style as Section 04
  - Different collection (accessories/full gear lineup)
  - White/light background
HORIZON ACTION: USE sections/featured-collection.liquid
  - Second instance of featured-collection — different collection assigned
  - This is fine — templates/index.json can have multiple instances
  - Give it a unique section ID
FILE TO EDIT: sections/featured-collection.liquid (same file, second instance in index.json)

---

### SECTION 11 — Three Steps / How It Works
FRAMER DESIGN:
  - Label: "The Setup" (eyebrow)
  - Heading: "Three Steps To Smarter Hydration."
  - 3 equal columns:
    * "Attach" — step number/icon, title, description
    * "Route" — step number/icon, title, description
    * "Go" — step number/icon, title, description
  - Light background
HORIZON ACTION: USE sections/multicolumn.liquid
  - 3 blocks with step number, title, text
  - Add step_number setting to block schema if not present
  - Second instance of multicolumn in templates/index.json
FILE TO EDIT: sections/multicolumn.liquid
CSS: assets/section-custom-multicolumn-steps.css (numbered step styling)

---

### SECTION 12 — Brand Divider Strip
FRAMER DESIGN:
  - Full-width image strip (horizontal banner image)
  - Image: framerusercontent.com/images/W4MXCALFFY2uUB3RJob5dYyk08.png (1920x160)
  - Purely decorative — no text, just a visual break/terrain graphic
HORIZON ACTION: USE sections/image-banner.liquid
  - Set image, disable all text overlays in schema
  - Set section height to match 160px strip ratio
  - OR use sections/custom-marquee.liquid if it's a repeating pattern strip
FILE TO EDIT: sections/image-banner.liquid (instance in index.json, no text enabled)

---

### SECTION 13 — Effortless Hydration (Video + Text)
FRAMER DESIGN:
  - Label: "Effortless Hydration" (eyebrow)
  - Heading: "Install in seconds"
  - Body paragraph: "Convert your insulated water bottle into a hands-free hydration system..."
  - Two CTAs: "Shop Kit" (shown twice — likely primary + secondary)
  - RIGHT side: autoplay video (mp4)
  - Layout: text LEFT (40%), video RIGHT (60%) — desktop
  - Dark or medium background
HORIZON ACTION: USE sections/image-with-text.liquid
  - Horizon's image-with-text supports video as media type
  - Check if video setting exists in schema — it should in Horizon
  - Set video to autoplay, muted, loop
  - Color scheme: dark
FILE TO EDIT: sections/image-with-text.liquid (second instance, video media type)

---

### SECTION 14 — FAQ / Collapsible Content
FRAMER DESIGN:
  - Label: "Common Questions" (eyebrow)
  - Heading: "Find Your Answers Fast"
  - Subtext: "Everything you need to know about compatibility, durability, and field use."
  - "Contact support" link (right side)
  - 5 accordion items (collapsed by default, expand on click):
    * What is HydroLid, and what makes it unique?
    * Are HydroLid products compatible with other insulated bottles?
    * How can HydroLid products be adapted to various hydration capacities?
    * How does HydroLid support hydration needs during firefighting operations?
    * Is HydroLid compatible with the gear typically used by fire departments?
  - Clean, minimal accordion style
HORIZON ACTION: USE sections/collapsible-content.liquid
  - Blocks for each FAQ item (question as heading, answer as richtext)
  - Add "contact_link_label" and "contact_link_url" settings to schema if missing
  - Color scheme: light or neutral
FILE TO EDIT: sections/collapsible-content.liquid

---

### SECTION 15 — Blog / News Section
FRAMER DESIGN:
  - Label: "News" (eyebrow)
  - Heading: "Learn More About Smarter Hydration."
  - "View All" link top right
  - 3 blog article cards in a row:
    * Card 1: image (portrait ratio), title "Fishing with HydroLid...", excerpt, "Read More" link
    * Card 2: image, title "HydroLid: Essential Tool for Hydration", excerpt, "Read More"
    * Card 3: image, title "Camping with HydroLid: A New Level.", excerpt, "Read More"
  - Cards use snippets/card-article.liquid — DO NOT recreate
HORIZON ACTION: USE sections/featured-blog.liquid
  - Assign blog handle in schema
  - Set article count to 3
  - Reuse card-article.liquid snippet as-is
FILE TO EDIT: sections/featured-blog.liquid (eyebrow setting if missing)

---

### SECTION 16 — Trust Badges + CTA
FRAMER DESIGN:
  - Label: "The Setup" (eyebrow)
  - Heading: "Built to Perform. Backed by Us."
  - 4 icon + label trust badges in a row:
    * Free Shipping
    * Field-Tested Quality
    * Secure Checkout
    * Customer Support
  - Below badges: subheading "If Something Isn't Right, We'll Make it Right"
  - Body text + CTA button: "Upgrade Your Setup"
  - Light or white background
HORIZON ACTION: USE sections/multicolumn.liquid
  - 4 blocks (icon + label)
  - Add bottom CTA text + button settings to section schema if not present
  - Third instance of multicolumn in index.json
FILE TO EDIT: sections/multicolumn.liquid
CSS: assets/section-custom-multicolumn-trust.css (badge icon sizing)

---

### SECTION 17 — UGC Video Grid (CUSTOM)
FRAMER DESIGN:
  - Label: "In Use" (eyebrow)
  - Heading: "Used in the Wild. Trusted in the Field."
  - 4 video tiles in a row (or 2x2 grid):
    * Each tile: autoplay looping mp4 video (vertical/square ratio)
    * Overlay at bottom: product name + price
    * Tiles have rounded corners
  - Dark background
HORIZON ACTION: CREATE custom section
  (No native Shopify section handles this UGC video grid format)
FILE: sections/custom-ugc-video-grid.liquid
CSS: assets/section-custom-ugc-video-grid.css
SCHEMA SETTINGS:
  - text: eyebrow
  - textarea: heading
  - color_scheme
  - range: padding_top, padding_bottom
  BLOCKS (type: "video_tile", max: 8):
    - video: tile_video (Shopify-hosted) OR text: video_url (external mp4)
    - text: product_name
    - text: product_price
    - url: tile_link
LAYOUT NOTES:
  - Grid: 4 columns desktop, 2 columns tablet, 1-2 columns mobile
  - Each tile: position relative, border-radius: 12px, overflow: hidden
  - Video: width 100%, height 100%, object-fit: cover, autoplay muted loop
  - Overlay: position absolute, bottom 0, gradient from transparent to rgba black
  - Product info: position absolute, bottom: 16px, left: 16px, color: white

---

### SECTION 18 — Email Signup / Newsletter
FRAMER DESIGN:
  - Full-width section with background VIDEO (autoplay, muted)
  - Heading: "STAY FIELD READY." (large, centered, uppercase)
  - Subtext: "Get early access to new gear drops, field-tested updates..."
  - Email input field + submit button (centered)
  - Dark overlay on video
HORIZON ACTION: USE sections/email-signup.liquid
  - Add "background_video" video setting to schema if not present
  - Add overlay opacity range setting
  - Heading/subtext settings already exist in Horizon's email-signup
  - Color scheme: dark
FILE TO EDIT: sections/email-signup.liquid
CSS: assets/section-custom-email-signup-hydrolid.css (video background support)

---

### SECTION 19 — CTA Footer Banner
FRAMER DESIGN:
  - Full-width dark banner
  - Large heading: "Stop Settling For Warm Water."
  - Subtext: "Upgrade your hydration system today."
  - HydroLid logo image centered
  - Patent text: "US PATENT NO. 11,866,230"
  - Brand line: "Repositioning the standard for extreme hydration. Born in the fire, built for the mission."
HORIZON ACTION: USE sections/image-banner.liquid
  - Dark background (color scheme, no image needed)
  - Text overlay content: heading + subtext
  - Add logo image picker + patent text settings if not present
FILE TO EDIT: sections/image-banner.liquid (instance in index.json)
CSS: assets/section-custom-cta-banner-hydrolid.css (logo + patent text styling)

---

### SECTION 20 — Footer
FRAMER DESIGN:
  - Dark background, multi-column layout
  - Logo top left + tagline
  - Social icons: Facebook, Instagram
  - 3 link columns:
    * Equipment: Hydrolid System, Tactical Kits, Adventure Series, Replacement Parts
    * The Mission: Our Story, Field Testing, Patent Info, Bulk/Gov Orders
    * Information: Blog, Privacy Policy, Refund Policy, Shipping Policy, Terms of Service
  - Copyright: "© 2026 HydroLid. All rights reserved."
  - Payment icons row at bottom
HORIZON ACTION: USE sections/footer.liquid
  - Upload logo via theme editor
  - Create 3 matching menus in Shopify admin → Navigation
  - Menu handles: "footer-equipment", "footer-mission", "footer-information"
  - Social links via theme settings (not footer schema)
  - Payment icons are automatic from Shopify
FILE TO EDIT: sections/footer.liquid (column label settings)

---

## TEMPLATES/INDEX.JSON — SECTION ORDER

After building all sections, templates/index.json should follow this order:
```
1.  announcement-bar        (existing Horizon section)
2.  header                  (existing Horizon section)
3.  custom-hero             (NEW)
4.  featured-collection     (existing, "new-arrivals" instance)
5.  image-with-text         (existing, "the-system" instance)
6.  multicolumn             (existing, "pain-points" instance)
7.  custom-audience-split   (NEW)
8.  testimonials            (existing)
9.  custom-origin-timeline  (NEW)
10. featured-collection     (existing, "the-gear" instance)
11. multicolumn             (existing, "three-steps" instance)
12. image-banner            (existing, "divider-strip" instance)
13. image-with-text         (existing, "video-install" instance)
14. collapsible-content     (existing)
15. featured-blog           (existing)
16. multicolumn             (existing, "trust-badges" instance)
17. custom-ugc-video-grid   (NEW)
18. email-signup            (existing)
19. image-banner            (existing, "cta-banner" instance)
20. footer                  (existing Horizon section)
```

---

## NEW CUSTOM FILES SUMMARY

Files to CREATE (4 new sections + 4 new CSS files):
```
sections/custom-hero.liquid
sections/custom-audience-split.liquid
sections/custom-origin-timeline.liquid
sections/custom-ugc-video-grid.liquid

assets/section-custom-hero.css
assets/section-custom-audience-split.css
assets/section-custom-origin-timeline.css
assets/section-custom-ugc-video-grid.css
```

Files to EDIT (existing Horizon sections — schema + minor CSS only):
```
sections/announcement-bar.liquid      → marquee ticker CSS
sections/featured-collection.liquid   → eyebrow label setting
sections/image-with-text.liquid       → eyebrow + feature bullet blocks
sections/multicolumn.liquid           → step number + intro heading settings
sections/testimonials.liquid          → role/rating badge settings
sections/collapsible-content.liquid   → contact link settings
sections/featured-blog.liquid         → eyebrow setting
sections/email-signup.liquid          → video background setting
sections/image-banner.liquid          → logo/patent text for CTA banner

assets/section-custom-announcement-ticker.css   (new CSS for existing section)
assets/section-custom-featured-collection-hydrolid.css
assets/section-custom-image-with-text-hydrolid.css
assets/section-custom-multicolumn-problems.css
assets/section-custom-multicolumn-steps.css
assets/section-custom-multicolumn-trust.css
assets/section-custom-testimonials-hydrolid.css
assets/section-custom-cta-banner-hydrolid.css
assets/section-custom-email-signup-hydrolid.css
```

---

## TYPOGRAPHY SPECIFICATION (HydroLid Brand)

From Framer design analysis — apply these per element type:

EYEBROW / LABEL TEXT (e.g. "Mission Ready", "The System", "New Arrivals"):
  - Font: var(--font-body-family) OR a condensed/caps variant
  - Size: ~12-14px
  - Weight: 600-700
  - Letter-spacing: 0.1-0.15em
  - Text-transform: uppercase
  - Color: var(--color-foreground) at 60-70% opacity OR accent color

H1 / HERO HEADING ("Ditch the Bladder..."):
  - Font: var(--font-heading-family)
  - Size: clamp(40px, 6vw, 80px) — scales with viewport
  - Weight: 800-900 (extrabold/black)
  - Line-height: 1.05-1.1
  - Letter-spacing: -0.02em (tight)
  - Color: var(--color-foreground) (white on dark backgrounds)

H2 / SECTION HEADINGS ("Built for the Field.", "Not a Lid..."):
  - Font: var(--font-heading-family)
  - Size: clamp(28px, 4vw, 52px)
  - Weight: 700-800
  - Line-height: 1.1-1.15
  - Letter-spacing: -0.01em

H3 / SUBHEADINGS:
  - Font: var(--font-heading-family)
  - Size: clamp(18px, 2.5vw, 28px)
  - Weight: 600
  - Line-height: 1.3

H4 / FEATURE BULLETS AND STEP TITLES:
  - Font: var(--font-heading-family) OR var(--font-body-family)
  - Size: 16-18px
  - Weight: 600
  - Line-height: 1.4

BODY PARAGRAPH:
  - Font: var(--font-body-family)
  - Size: 16px (desktop), 15px (mobile)
  - Weight: 400
  - Line-height: 1.6-1.7
  - Color: var(--color-foreground) at 80% opacity on dark, full on light

BUTTON TEXT:
  - Font: var(--font-body-family)
  - Size: 14-15px
  - Weight: 600-700
  - Letter-spacing: 0.05em
  - Text-transform: uppercase or normal (check Framer buttons)

PRICE TEXT:
  - Font: var(--font-heading-family)
  - Weight: 700
  - Size: 18-20px

NEVER apply a single font-family to all text in a section.
ALWAYS identify which of the above categories each text element belongs to.

---

## LAYOUT & POSITIONING RULES

### Breakpoints (confirm against base.css after reading):
  - Mobile: max-width 749px
  - Tablet: 750px - 989px
  - Desktop: min-width 990px

### Container:
  - Use .page-width class for standard sections
  - Confirm max-width value from base.css (usually 1200-1440px)
  - Full-bleed backgrounds with .page-width content inside

### Absolute Positioning:
  - HERO product card: position absolute, bottom: 40px, right: 40px
  - NEVER center an element that is off-center in the Framer design
  - ALWAYS set parent to position: relative before using position: absolute
  - On mobile: convert absolute positioned elements to static flow

### Images:
  - ALWAYS use Horizon's image rendering snippet (check snippets/ for name)
  - Use image_url filter with width parameter for responsive images:
    {{ image | image_url: width: 1500 }}
  - ALWAYS set object-fit: cover on image containers
  - ALWAYS set explicit aspect-ratio on image wrappers
  - Hero desktop: aspect-ratio auto / min-height: 100vh
  - Product images: aspect-ratio: 1/1 (square)
  - Article/blog images: aspect-ratio: 4/3 or as per Framer

### Videos:
  - Set autoplay muted loop playsinline on all background/ambient videos
  - Width: 100%, height: 100%, object-fit: cover
  - Always provide image fallback via poster attribute

---

## CSS RULES

### Color Variables (mandatory — read color-schemes.liquid first):
  - Background: var(--color-background)
  - Foreground/text: var(--color-foreground)
  - Accent/CTA: var(--color-accent) or var(--color-button)
  - Button label: var(--color-button-label)
  - Border: var(--color-border)
  NEVER hardcode hex values. If a Framer color doesn't match any variable,
  add a new setting to settings_schema.json or use a local CSS variable
  scoped to the section.

### File Loading in Section:
  {{ 'section-custom-[name].css' | asset_url | stylesheet_tag }}
  Place this at the TOP of the section .liquid file, before the HTML.

### CSS Scoping:
  - All custom CSS scoped to section wrapper:
    .custom-hero-section { }   ← NOT just .hero { }
  - Use #shopify-section-{{ section.id }} for section-specific overrides

### Dynamic Padding via Schema:
  <style>
    #shopify-section-{{ section.id }} {
      padding-top: {{ section.settings.padding_top }}px;
      padding-bottom: {{ section.settings.padding_bottom }}px;
    }
  </style>

---

## SCHEMA TEMPLATE (Required for every new section)

```json
{
  "name": "Section Name",
  "tag": "section",
  "class": "custom-[name]-section",
  "settings": [
    {
      "type": "color_scheme",
      "id": "color_scheme",
      "label": "Color scheme",
      "default": "scheme-1"
    },
    {
      "type": "header",
      "content": "Section padding"
    },
    {
      "type": "range",
      "id": "padding_top",
      "min": 0, "max": 100, "step": 4, "unit": "px",
      "label": "Padding top",
      "default": 40
    },
    {
      "type": "range",
      "id": "padding_bottom",
      "min": 0, "max": 100, "step": 4, "unit": "px",
      "label": "Padding bottom",
      "default": 40
    }
  ],
  "blocks": [],
  "presets": [
    { "name": "Section Name" }
  ]
}
```

---

## BUILD RULES — ABSOLUTE NON-NEGOTIABLES

1. READ all required files before any code — no exceptions
2. Never recreate a section that exists in Horizon — use and customize it
3. Never recreate a snippet (card-product, card-article, image) — render it
4. Never use {% include %} — always {% render %}
5. Never add {% schema %} to snippets — only sections
6. Never put files in assets subfolders — assets/ is always flat
7. Never hardcode colors — always use color scheme variables
8. Never center an absolutely positioned element unless the design shows it centered
9. Never apply one font-family to all text in a section — each element has its own
10. Never write multiple sections in one go — build → confirm → next section
11. Never guess at design measurements — ASK the developer
12. Always provide mobile layout explicitly for every section
13. Always use object-fit: cover with explicit dimensions on all images

---

## BUILD SEQUENCE — ONE SECTION AT A TIME

Follow this exact order. Do not proceed to the next until developer confirms:

Phase 1 — Foundation:
  [ ] Read all required files, confirm understanding
  [ ] Present this section map to developer for approval
  [ ] Confirm templates/index.json current state

Phase 2 — Custom Sections (most complex first):
  [ ] Section 03: custom-hero.liquid
  [ ] Section 07: custom-audience-split.liquid
  [ ] Section 09: custom-origin-timeline.liquid
  [ ] Section 17: custom-ugc-video-grid.liquid

Phase 3 — Existing Section Modifications:
  [ ] Section 01: announcement-bar (ticker CSS)
  [ ] Section 04: featured-collection (eyebrow)
  [ ] Section 05: image-with-text (feature bullets)
  [ ] Section 06: multicolumn (pain points)
  [ ] Section 08: testimonials (role + rating)
  [ ] Section 11: multicolumn (steps)
  [ ] Section 13: image-with-text (video)
  [ ] Section 14: collapsible-content (FAQ)
  [ ] Section 15: featured-blog
  [ ] Section 16: multicolumn (trust badges)
  [ ] Section 18: email-signup (video bg)
  [ ] Section 19: image-banner (CTA)

Phase 4 — Assembly:
  [ ] Update templates/index.json with full section order
  [ ] Test all sections in shopify theme dev
  [ ] Mobile QA each section
  [ ] Push to GitHub branch

---

## SESSION STARTER — PASTE THIS EACH TIME

```
Read CLAUDE.md fully.
Then read these files in order:
1. snippets/color-schemes.liquid
2. snippets/theme-styles-variables.liquid
3. assets/base.css
4. layout/theme.liquid
5. config/settings_schema.json
6. templates/index.json
7. All files in sections/
8. All files in snippets/

After reading, confirm:
- List all CSS color variables from color-schemes.liquid
- List all font/typography variables from theme-styles-variables.liquid
- List exact breakpoint values from base.css
- List all sections currently on homepage from templates/index.json
- List all existing Horizon sections that will be reused

Then we will begin with Section 03: custom-hero.liquid
```
