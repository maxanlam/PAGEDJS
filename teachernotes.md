# Pagedjs Beginner Tutorial – Complete Screen Recording Plan

## Comprehensive Coverage of All README Topics

## Project: Simple Paperback with Chapters, Running Titles, Page Numbers & Footnotes

---

## **PART 1: FOUNDATIONAL CONCEPTS**

### **Scene 1: What is Pagedjs? (1–2 min)**

_Goal: Set context — explain the problem and solution_

### What to show:

- Open browser → show a normal HTML file rendered as web page
- Show browser print dialog (Ctrl+P / Cmd+P)
- Point out: "Browser ignores most print CSS rules"
- Close dialog
- **Narrate**: "Pagedjs lets us use print CSS rules that browsers normally ignore. It simulates a print preview right in the browser."
- Show the pagedjs polyfill script line in code editor

### What NOT to show:

- Technical details about CSS Generated Content Module
- Browser compatibility charts
- Why pagedjs is needed (viewers don't need to know)

---

### **Scene 2: Print Preview Simulation (2 min)**

_Goal: Show pagedjs chunking a website into pages_

### What to show:

- Open HTML with pagedjs loaded (minimal content: a few paragraphs)
- Show code:
  ```html
  <script>
    window.PagedConfig = { auto: false };
  </script>
  <script src="./paged.polyfill.js" defer></script>
  <link rel="stylesheet" href="./interface.css" />
  <button onclick="window.PagedPolyfill.preview()">preview</button>
  ```
- Click the preview button
- Show the print preview window opening with pages displayed side-by-side
- Point out: "One long HTML document is now chunked into multiple pages"
- Show the dev tools (F12) → open inspector
- Point out: You can edit CSS live in dev tools and see changes instantly in the preview

### What NOT to show:

- The actual HTML structure of pagedjs (it's complex)
- Auto-preview vs manual preview difference in depth

---

## **PART 2: BASIC PAGE SETUP (@page rules)**

### **Scene 3: Page Size (size rule) (2 min)**

_Goal: Show how to define paper format_

### What to show:

- Code editor with `<style>` block
- Add basic @page rule:
  ```css
  @page {
    size: A4;
  }
  ```
- Click preview → show A4 page format
- Edit to:
  ```css
  @page {
    size: A4 landscape;
  }
  ```
- Click preview again → show landscape orientation
- Edit to custom size:
  ```css
  @page {
    size: 200mm 200mm;
  }
  ```
- Show the square page format
- **Highlight**: "size rule can only be defined once in @page"
- Revert to `size: A4;`

### What NOT to show:

- All DIN formats (A4 is sufficient)
- pt vs mm unit conversation
- Aspect ratio calculations

---

### **Scene 4: Margins (margin rule) (2 min)**

_Goal: Show how to set the type area / margins_

### What to show:

- In `@page` rule, add margins:
  ```css
  @page {
    size: A4;
    margin-top: 20mm;
    margin-bottom: 25mm;
    margin-left: 10mm;
    margin-right: 35mm;
  }
  ```
- Click preview → show the margins visually (gray area outside)
- Show shorthand version:
  ```css
  margin: 20mm 35mm 25mm 10mm;
  ```
- Click preview → show it's the same
- **Highlight**: Margins can be filled with content later (we'll see this with running titles)

### What NOT to show:

- Margin boxes yet (those come in generated content section)
- Complex asymmetric layouts

---

### **Scene 5: Bleed (bleed attribute) (1 min)**

_Goal: Introduce bleed concept for print_

### What to show:

- Add to `@page` rule:
  ```css
  bleed: 3mm;
  ```
- Click preview → show the slightly larger page with the bleed area
- **Narrate**: "Bleed is the area that gets cut off during printing. Usually 3mm."
- Optionally show in dev tools: the outer gray border around the page

### What NOT to show:

- Different bleed values per side
- Bleeding images into the bleed (that's Scene 14)

---

## **PART 3: TARGETING SPECIFIC PAGES**

### **Scene 6: Page Pseudo Selectors – :first, :blank, :left, :right (3 min)**

_Goal: Show how to style different page types_

### What to show:

- Add multiple chapters to HTML:

  ```html
  <h1 class="chapter-title">Chapter 1</h1>
  <p>Lorem ipsum dolor sit amet...</p>

  <h1 class="chapter-title">Chapter 2</h1>
  <p>More content...</p>

  <h1 class="chapter-title">Chapter 3</h1>
  <p>Even more content...</p>
  ```

- Add CSS for :first page:
  ```css
  @page:first {
    background-color: lightyellow;
  }
  ```
- Click preview → show first page is highlighted
- Switch to :left/:right:
  ```css
  @page:left {
    background-color: lightblue;
  }
  @page:right {
    background-color: lightgreen;
  }
  ```
- Click preview → flip through pages, showing alternating colors
- Show :blank selector:
  ```css
  @page:blank {
    display: none;
  }
  ```
- Click preview → note any blank pages are hidden
- Remove background colors (just for demo)

### What NOT to show:

- All pseudo-selectors stacked together
- Complex color schemes
- Mixing with other @page rules yet

---

### **Scene 7: Named Pages (page property) (2 min)**

_Goal: Show how to style specific chapters differently_

### What to show:

- Add `page` property to chapter:
  ```css
  h1.chapter-title:first-of-type {
    page: Introduction;
  }
  ```
- Add named @page rule:
  ```css
  @page Introduction {
    background-color: lightyellow;
  }
  ```
- Click preview → show Introduction chapter has yellow background
- Add another named page:

  ```css
  h1.chapter-title:nth-of-type(2) {
    page: MainContent;
  }

  @page MainContent {
    background-color: white;
  }
  ```

- Click preview → show different styling
- Combine with pseudo-selector:
  ```css
  @page Introduction:first {
    margin: 0;
  }
  ```
- Click preview → show first page of Introduction has no margin
- Remove background colors for next scenes

### What NOT to show:

- Complex selector logic
- More than 2–3 named pages
- Why you'd use named pages over pseudo-selectors

---

## **PART 4: DESIGNING FOR PRINT**

### **Scene 8: @media print query (1 min)**

_Goal: Show how to hide screen-only elements in print_

### What to show:

- Add a button to HTML:
  ```html
  <button class="no-print">Click Me</button>
  <p>This text prints.</p>
  ```
- Add CSS:
  ```css
  @media print {
    .no-print {
      display: none;
    }
  }
  ```
- Click preview → button is gone
- Show in normal browser view (resize editor window): button is still there
- **Narrate**: "@media print hides things only for printing"

### What NOT to show:

- @media screen differences
- Device-specific media queries
- Complex hiding strategies

---

### **Scene 9: Page Breaks – break-before & break-after (2 min)**

_Goal: Show how to control page flow_

### What to show:

- Add CSS to chapter titles:
  ```css
  h1.chapter-title {
    break-before: page;
    break-after: page;
  }
  ```
- Click preview → each chapter starts on its own page
- Show the break-before/after values:
  - `always` (currently using `page`, same effect)
  - `left` (try it):
    ```css
    h1.chapter-title {
      break-before: left;
    }
    ```
  - Click preview → Chapter 2 might be pushed to a left page
  - Revert to `page`
- Show `avoid`:
  ```css
  .important-box {
    break-inside: avoid;
  }
  ```

  - Add HTML: `<div class="important-box"><p>Don't split me!</p></div>`
  - Click preview → box stays together
- **Highlight**: "avoid tries to keep elements together, but will break if necessary"

### What NOT to show:

- `avoid-page` vs `avoid-column` distinction
- Complex break scenarios with multiple rules

---

### **Scene 10: break-inside: avoid (keeping elements together) (1 min)**

_Goal: Deep dive on preventing splits_

### What to show:

- Use previous `important-box` example
- Add more content to force a page break:
  ```html
  <div class="important-box">
    <h2>Important Info</h2>
    <p>This block should stay together</p>
  </div>
  <p>Lots of filler content...</p>
  <p>... (repeated to force break)</p>
  ```
- Click preview → show the box stays on one page, not split across pages
- **Narrate**: "break-inside: avoid is great for sidebars, quotes, images with captions"

### What NOT to show:

- What happens when you remove the rule (optional)
- Multiple break-inside values

---

## **PART 5: USING PAGEDJS CLASSES**

### **Scene 11: .pagedjs_left_page and .pagedjs_right_page classes (2 min)**

_Goal: Show pagedjs-generated classes for styling_

### What to show:

- Add CSS using pagedjs classes:
  ```css
  .pagedjs_left_page p {
    color: blue;
  }
  .pagedjs_right_page p {
    color: red;
  }
  ```
- Click preview → flip through pages, showing text color alternating
- Open dev tools → inspect a paragraph element
- Point out: "pagedjs automatically adds `.pagedjs_left_page` and `.pagedjs_right_page` classes"
- Remove color styling (for clarity in later scenes)

### What NOT to show:

- Other pagedjs classes (there are many)
- Structural details of pagedjs rendering

---

## **PART 6: GENERATED CONTENT & MARGIN BOXES**

### **Scene 12: Margin Boxes & Content Property (2 min)**

_Goal: Show how to fill margin areas with content_

### What to show:

- Update `@page` rule with margin boxes:
  ```css
  @page {
    size: A4;
    margin: 20mm 35mm 25mm 10mm;
    bleed: 3mm;

    @top-center {
      content: "My Book Title";
    }

    @bottom-center {
      content: "Page footer";
    }
  }
  ```
- Click preview → show text appears at top and bottom of every page
- **Narrate**: "Margin boxes are areas in the margins we can fill with content"
- List available margin boxes:
  ```css
  @page {
    @top-left {
      content: "Top Left";
    }
    @top-center {
      content: "Top Center";
    }
    @top-right {
      content: "Top Right";
    }
    @bottom-left {
      content: "Bottom Left";
    }
    @bottom-center {
      content: "Bottom Center";
    }
    @bottom-right {
      content: "Bottom Right";
    }
    @top-left-corner {
      content: "TL Corner";
    }
    @top-right-corner {
      content: "TR Corner";
    }
    @bottom-left-corner {
      content: "BL Corner";
    }
    @bottom-right-corner {
      content: "BR Corner";
    }
  }
  ```
- Click preview → show all positions filled
- Simplify back to just:
  ```css
  @page {
    @bottom-center {
      content: "Page footer";
    }
  }
  ```

### What NOT to show:

- Every possible margin box at once in final version
- Styling margin box content (font-size, etc.)

---

### **Scene 13: CSS Counters – counter-increment & counter-reset (2 min)**

_Goal: Show how to number elements_

### What to show:

- Add footnote HTML:
  ```html
  <p>
    This is text with a footnote<span class="footnote">Here's the note</span>.
  </p>
  <p>Another line with a footnote<span class="footnote">Another note</span>.</p>
  ```
- Add CSS to increment counter:
  ```css
  .footnote {
    counter-increment: footnote;
  }
  ```
- Add CSS to display counter:
  ```css
  .footnote::before {
    content: "[" counter(footnote) "]";
    font-weight: bold;
  }
  ```
- Click preview → see [1] [2] appearing
- Show counter-reset:
  ```css
  h1.chapter-title {
    counter-reset: footnote;
  }
  ```
- Click preview → footnote counter resets to [1] at each chapter
- **Highlight**: "counter-reset: footnote 4; would reset to 4 instead of 1"

### What NOT to show:

- Multiple custom counters (just footnote)
- Styling footnote text beyond the counter

---

### **Scene 14: Paged Media Counters – page & pages (2 min)**

_Goal: Show automatic page numbering_

### What to show:

- Update margin box:
  ```css
  @page {
    @bottom-center {
      content: counter(page);
    }
  }
  ```
- Click preview → page numbers appear (1, 2, 3...)
- Show combined counters:
  ```css
  @page {
    @bottom-center {
      content: counter(page) " / " counter(pages);
    }
  }
  ```
- Click preview → see "1 / 12" (if 12 pages total)
- Flip through pages to confirm total stays the same
- **Narrate**: "page is current page number, pages is total page count"
- Simplify back to just page counter for next scenes

### What NOT to show:

- Counter styling (we'll see that when combining with running titles)

---

### **Scene 15: Running Heads – string-set & string() (3 min)**

_Goal: Show how to capture and display article titles_

### What to show:

- Update chapter CSS to capture title:
  ```css
  h1.chapter-title {
    string-set: chapterTitle content(text);
    break-before: page;
  }
  ```
- Update margin box to display it:
  ```css
  @page {
    @bottom-center {
      content: string(chapterTitle);
    }
  }
  ```
- Click preview → chapter titles now appear at bottom of each page
- Flip through pages → see titles change when you reach a new chapter
- Show the `first`, `last`, `start` options:
  ```css
  @page {
    @bottom-center {
      content: string(chapterTitle, first);
    }
  }
  ```
- Click preview → always uses the first chapter title on the page
- Switch to `last`:
  ```css
  content: string(chapterTitle, last);
  ```
- Click preview → uses the last chapter title on the page
- Explain: "first is the earliest title on the page, last is the latest"
- Revert to just `string(chapterTitle)` for next scene

### What NOT to show:

- Using string-set for other elements (keep focused on titles)
- Complex content() syntax

---

### **Scene 16: Running Elements – position: running() & element() (2 min)**

_Goal: Show how to move elements to margin boxes_

### What to show:

- Add an element to HTML (e.g., an image or div):
  ```html
  <img src="corner-symbol.svg" class="running-element" alt="corner" />
  ```
- Use running() to move it:
  ```css
  img.running-element {
    position: running(cornerSymbol);
  }
  ```
- Add it to a margin box:
  ```css
  @page {
    @top-left-corner {
      content: element(cornerSymbol);
    }
  }
  ```
- Click preview → image appears in top-left corner of every page, removed from main content
- **Narrate**: "position: running() removes the element from the page flow and lets us place it in a margin box"
- Optional: Show it doesn't appear in the document body anymore

### What NOT to show:

- Multiple running elements
- Complex positioning logic

---

### **Excursus: Table of Contents (Bonus Scene, 2 min)**

_Goal: Show how to auto-generate page numbers in TOC_

### What to show:

- Add simple TOC HTML:
  ```html
  <h2>Table of Contents</h2>
  <ul>
    <li><a href="#ch1">Chapter 1</a></li>
    <li><a href="#ch2">Chapter 2</a></li>
  </ul>
  ```
- Update chapter IDs:
  ```html
  <h1 class="chapter-title" id="ch1">Chapter 1</h1>
  ```
- Add CSS for page numbers:
  ```css
  li a::after {
    content: target-counter(attr(href), page);
  }
  ```
- Click preview → TOC shows page numbers automatically
- Flip through pages → confirm numbers match chapter pages
- **Narrate**: "target-counter looks up the page number of the linked element"

### What NOT to show:

- JavaScript-generated TOC
- Styling the TOC heavily
- Line leaders (dots before page numbers)

---

## **PART 7: HELPFUL CSS RULES FOR PRINT**

### **Scene 17: Columns – Multi-column Layout (2 min)**

_Goal: Show text flowing across columns_

### What to show:

- Add CSS:
  ```css
  .multi-column {
    columns: 2 100mm;
  }
  ```
- Add HTML:
  ```html
  <div class="multi-column">
    <p>Lots of text that flows across columns...</p>
    <p>...continues here...</p>
  </div>
  ```
- Click preview → text flows into two columns
- Show the shorthand breakdown:
  ```css
  column-count: 2;
  column-width: 100mm;
  ```
- Click preview → same result
- Show column breaks:
  ```css
  .multi-column h3 {
    break-after: column;
  }
  ```
- Click preview → headings start new columns
- **Narrate**: "Columns work better than CSS grid for print because text reflows naturally"

### What NOT to show:

- Column gap, column rule (styling details)
- Complex multi-column layouts

---

### **Scene 18: Widows & Orphans (Chrome Only) (1 min)**

_Goal: Show how to control line breaks at page breaks_

### What to show:

- Add CSS:
  ```css
  p {
    widows: 2;
    orphans: 3;
  }
  ```
- Click preview → look for paragraphs that would break across pages
- Point out: Widows = min lines at start of new page, orphans = min lines before page break
- **Narrate**: "This prevents lonely lines from appearing at the top or bottom of pages"
- Mention: "This only works in Chrome for now"

### What NOT to show:

- Why orphans/widows are important (just show they work)
- Browser compatibility in depth

---

### **Scene 19: Floated Elements (float property) (1 min)**

_Goal: Show text wrapping around floated images_

### What to show:

- Add HTML with image and text:
  ```html
  <img class="float-image" src="image.jpg" alt="Floated image" />
  <p>Text that wraps around the image...</p>
  ```
- Add CSS:
  ```css
  img.float-image {
    float: left;
    width: 100mm;
    margin-right: 10mm;
  }
  ```
- Click preview → text wraps around image
- Show `float: right`:
  ```css
  img.float-image {
    float: right;
  }
  ```
- Click preview → text wraps on the other side
- Revert and show `clear` property to stop wrapping:
  ```css
  .no-wrap {
    clear: both;
  }
  ```
- Click preview → text below this element doesn't wrap

### What NOT to show:

- Advanced float layouts
- CSS Grid as alternative (different tutorial)

---

## **PART 8: PAGEDJS VARIABLES**

### **Scene 20: CSS Variables for Calculations (2 min)**

_Goal: Show how to use pagedjs-provided variables_

### What to show:

- In dev tools, inspect the page element
- Point out CSS variables available:
  ```css
  --pagedjs-pagebox-width: 148mm;
  --pagedjs-pagebox-height: 210mm;
  --pagedjs-width: (with bleed) --pagedjs-height: (with bleed)
    --pagedjs-margin-top: 31.5mm;
  --pagedjs-margin-right: 22mm;
  --pagedjs-margin-left: 9mm;
  --pagedjs-margin-bottom: 7mm;
  --pagedjs-bleed-top: 3mm;
  --pagedjs-bleed-right: 3mm;
  --pagedjs-bleed-bottom: 3mm;
  --pagedjs-bleed-left: 3mm;
  ```
- Use a variable in CSS:
  ```css
  .bleed-image {
    margin-left: calc(
      -1 * (var(--pagedjs-bleed-left) + var(--pagedjs-margin-left))
    );
    width: var(--pagedjs-width);
  }
  ```
- Add HTML:
  ```html
  <img class="bleed-image" src="full-bleed.jpg" alt="Full bleed" />
  ```
- Click preview → image extends into the bleed area
- **Narrate**: "These variables let us use pagedjs layout values in calculations"

### What NOT to show:

- All variables at once
- Complex calc() expressions

---

## **PART 9: ADVANCED STYLING & LAYOUT**

### **Scene 21: Left/Right Page Asymmetric Margins (2 min)**

_Goal: Show binding-aware margin layout_

### What to show:

- Update `@page` rule:

  ```css
  @page:left {
    margin: 20mm 35mm 25mm 10mm;
  }

  @page:right {
    margin: 20mm 10mm 25mm 35mm;
  }
  ```

- Click preview → flip through pages
- **Point out**: Inner margins (spine) are larger, outer margins are smaller
- Combine with running titles on left/right:

  ```css
  @page:left {
    margin: 20mm 35mm 25mm 10mm;
    @bottom-left {
      content: counter(page);
    }
    @bottom-right {
      content: string(chapterTitle);
    }
  }

  @page:right {
    margin: 20mm 10mm 25mm 35mm;
    @bottom-left {
      content: string(chapterTitle);
    }
    @bottom-right {
      content: counter(page);
    }
  }
  ```

- Click preview → see asymmetric layout with mirrored footers
- **Narrate**: "This is how real books are laid out — larger margins toward the spine"

### What NOT to show:

- Complex multi-level margin structures

---

## **PART 10: COMBINING EVERYTHING**

### **Scene 22: Complete Book Example – Full Walkthrough (4 min)**

_Goal: Show the final paperback book with all features_

### What to show:

- Show complete HTML structure:

  ```html
  <h1 class="chapter-title" id="ch1">Chapter 1: Introduction</h1>
  <p>Chapter content...</p>
  <p>Text with footnote<span class="footnote">Footnote text</span>.</p>

  <h1 class="chapter-title" id="ch2">Chapter 2: The Story</h1>
  <p>More content...</p>
  <p>Another footnote<span class="footnote">Note text</span>.</p>

  <h1 class="chapter-title" id="ch3">Chapter 3: Conclusion</h1>
  <p>Final chapter...</p>
  ```

- Show complete CSS:

  ```css
  @page {
    size: A4;
    margin: 20mm 35mm 25mm 10mm;
    bleed: 3mm;
  }

  @page:left {
    margin: 20mm 35mm 25mm 10mm;
    @bottom-left {
      content: counter(page);
    }
    @bottom-right {
      content: string(chapterTitle);
    }
  }

  @page:right {
    margin: 20mm 10mm 25mm 35mm;
    @bottom-left {
      content: string(chapterTitle);
    }
    @bottom-right {
      content: counter(page);
    }
  }

  h1.chapter-title {
    string-set: chapterTitle content(text);
    break-before: left;
    break-after: page;
    counter-reset: footnote;
  }

  .footnote {
    counter-increment: footnote;
  }

  .footnote::before {
    content: "[" counter(footnote) "] ";
    font-weight: bold;
  }

  @media print {
    .no-print {
      display: none;
    }
  }
  ```

- Click preview
- Flip through pages slowly, highlighting:
  - Chapter titles on new (left) pages
  - Alternating page numbers (left/right)
  - Running titles (chapter names) in footers
  - Footnotes numbered and resetting per chapter
  - Proper margins on left/right pages
  - Full page layout
- Scroll through entire document
- Open dev tools → inspect elements showing `.pagedjs_left_page` and `.pagedjs_right_page` classes

### What NOT to show:

- Editing the example (keep it static)
- Debug errors (assume it's perfect)

---

## **PART 11: JAVASCRIPT HOOKS & PLUGINS (Reference Only)**

### **Scene 23: JavaScript Hooks Intro (1 min)**

_Goal: Show that hooks exist for advanced users_

### What to show:

- In code editor, add a comment:
  ```javascript
  // Pagedjs hooks for advanced users:
  // - afterPageBreak
  // - beforePageBreak
  // - etc.
  ```
- Point to pagedjs documentation URL
- **Narrate**: "For advanced users, pagedjs provides JavaScript hooks to customize page generation. We won't cover this in this beginner tutorial."

### What NOT to show:

- Actual hook code
- How to implement hooks

---

### **Scene 24: Plugins Overview (1 min)**

_Goal: Mention available plugins_

### What to show:

- List the plugins mentioned in README:
  - **Imposition**: For imposing brochures (shingling)
  - **Full-Page**: Display images across entire page
  - **Table of Contents**: Auto-generate TOC
- **Narrate**: "Pagedjs has plugins for advanced features. You can find them in the pagedjs documentation."
- Point to documentation
- **Note**: "You don't need these for this basic paperback book."

### What NOT to show:

- How to install or use plugins
- Code examples for plugins

---

## **FINAL ASSEMBLY & EDITING**

### **Complete Scene Order (for editing):**

1. Scene 1: What is Pagedjs? (1–2 min)
2. Scene 2: Print Preview Simulation (2 min)
3. Scene 3: Page Size (2 min)
4. Scene 4: Margins (2 min)
5. Scene 5: Bleed (1 min)
6. Scene 6: Page Pseudo Selectors (3 min)
7. Scene 7: Named Pages (2 min)
8. Scene 8: @media print (1 min)
9. Scene 9: break-before & break-after (2 min)
10. Scene 10: break-inside: avoid (1 min)
11. Scene 11: Pagedjs Classes (2 min)
12. Scene 12: Margin Boxes & Content Property (2 min)
13. Scene 13: CSS Counters (2 min)
14. Scene 14: Page & Pages Counters (2 min)
15. Scene 15: Running Heads – string-set (3 min)
16. Scene 16: Running Elements (2 min)
17. Excursus: Table of Contents (2 min)
18. Scene 17: Columns (2 min)
19. Scene 18: Widows & Orphans (1 min)
20. Scene 19: Floated Elements (1 min)
21. Scene 20: Pagedjs Variables (2 min)
22. Scene 21: Left/Right Margins (2 min)
23. Scene 22: Complete Book Walkthrough (4 min)
24. Scene 23: JavaScript Hooks (1 min)
25. Scene 24: Plugins Overview (1 min)

---
