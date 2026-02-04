# Nordic EPUB 2015-1 Checklist

Checklist for **Nordic Guidelines for Production of Accessible EPUB 3, version 2015-1**. Use only when the EPUB declares 2015-1 (`nordic:guidelines` or `dcterms:conformsTo`).

**Verify requirements against the official guideline:** [Nordic EPUB 2015-1](https://format.mtm.se/nordic_epub/2015-1/). The authoritative source is the PDF *Requirements for Quality Content Production in EPUB 3.0/XHTML (v.2015-1)* where available. For comparison with 2025-1 see [nordic-epub-checklist.md](nordic-epub-checklist.md). The structure below follows the same sections as the 2025-1 checklist, with 2015-1 exceptions stated where they apply.

## Version-specific rules (2015-1)

Compared with 2025-1, 2015-1 does **not** require:

- `role="doc-toc"` / `role="doc-pagelist"` on nav elements (epub:type is sufficient)
- `role="doc-pagebreak"` on page break elements (epub:type="pagebreak" is sufficient)
- The full 2025-1 accessibility metadata set (schema: accessMode, accessibilityFeature, etc.); 2015-1 specifies only dc: and nordic/dcterms meta in the package

2015-1 **does** require:

- NCX document (nav.ncx) in addition to the EPUB 3 nav document; spine must have `toc` attribute pointing to the NCX manifest item
- Page break elements use `title` attribute for page number (not aria-label)
- Specific metadata set and file naming as listed below

---

## Package Document (package.opf)

### Root Element

```xml
<package xmlns="http://www.idpf.org/2007/opf"
         xmlns:dc="http://purl.org/dc/elements/1.1/"
         prefix="nordic: http://www.mtm.se/epub/"
         version="3.0"
         unique-identifier="pub-identifier">
```

- [ ] `xmlns` namespace correct (<http://www.idpf.org/2007/opf>)
- [ ] `xmlns:dc` namespace correct (<http://purl.org/dc/elements/1.1/>)
- [ ] `prefix` includes `nordic: http://www.mtm.se/epub/`
- [ ] `version="3.0"`
- [ ] `unique-identifier` references `dc:identifier` (value pub-identifier)
- [ ] Package document filename is package.opf

### Required Metadata

Per 2015-1 spec (2.3.2), the following are required:

- [ ] `dc:identifier` with content = production UID; attribute `id="pub-identifier"`
- [ ] `dc:title` with publication title
- [ ] `dc:language` with RFC 5646 conformant value (e.g. nb, nn, sv, da, fi)
- [ ] `dc:date` in YYYY-MM-DD format
- [ ] `dc:publisher` (name of ordering agency)
- [ ] `meta property="dcterms:modified"` with value CCYY-MM-DDThh:mm:ssZ (last change by supplier)
- [ ] `meta name="dcterms:modified"` with `content` = same date (OPF2 backwards compatibility)
- [ ] `dc:creator` (author of publication)
- [ ] `dc:source` with value `urn:isbn:[ISBN of the publication]`
- [ ] `meta property="nordic:guidelines"` with value **2015-1**
- [ ] `meta name="nordic:guidelines"` with `content` = 2015-1 (OPF2 backwards compatibility)
- [ ] `meta property="nordic:supplier"` (name of supplier)
- [ ] `meta name="nordic:supplier"` with `content` = supplier name (OPF2 backwards compatibility)

### Accessibility metadata

- [ ] 2015-1 does **not** require the full 2025-1 schema: accessibility metadata set. Verify only what is required in the official 2015-1 guideline if any additional accessibility meta is specified.

### Manifest

- [ ] All publication documents and resources listed in `<manifest>`; each as an `<item>` element
- [ ] Exactly one `<item>` defines the NCX content document (e.g. nav.ncx)
- [ ] Fallback attribute applied for each item that references a fallback resource when applicable
- [ ] Navigation document (nav.xhtml) has `properties="nav"`
- [ ] Cover image has `properties="cover-image"`
- [ ] All `href` paths relative and correct
- [ ] Correct `media-type` for each item; MathML files have `properties="mathml"` when present

### Spine

- [ ] Reading order (sequence of `<itemref>`) corresponds to structure in source material unless otherwise requested
- [ ] Spine has **`toc` attribute** identifying the manifest item that defines the NCX (required in 2015-1)
- [ ] Cover `itemref` has `linear="no"` where applicable
- [ ] `idref` values match manifest `id` values
- [ ] Chapter endnotes / rearnotes: `linear="no"` on corresponding itemref when epub:type="rearnotes bodymatter"

---

## Navigation Document (nav.xhtml)

File name must contain the string "nav" (e.g. nav.xhtml). Required: `<nav epub:type="toc">` and `<nav epub:type="page-list">`. **role="doc-toc"** and **role="doc-pagelist"** are **not** required in 2015-1.

### Document Structure

- [ ] XML declaration: `<?xml version="1.0" encoding="utf-8"?>`
- [ ] DOCTYPE: `<!DOCTYPE html>`
- [ ] Correct namespaces on `<html>` (xmlns, xmlns:epub, epub:prefix, xml:lang, lang)
- [ ] `<title>` matches main heading or publication title
- [ ] `<link>` to CSS stylesheet where required by ordering agency

### Table of Contents

```html
<nav epub:type="toc">
  <h1>[Language-specific heading]</h1>
  <ol>...</ol>
</nav>
```

- [ ] **`epub:type="toc"`** present (required)
- [ ] `role="doc-toc"` **not** required for 2015-1 (optional)
- [ ] Heading and list structure; links to sections or pagebreak targets as per 2015-1 (see Content TOC below)

### Page List (if paginated)

```html
<nav epub:type="page-list">
  <h1>[Language-specific heading]</h1>
  <ol>...</ol>
</nav>
```

- [ ] **`epub:type="page-list"`** present when work is paginated (required)
- [ ] `role="doc-pagelist"` **not** required for 2015-1 (optional)
- [ ] Page list entries match page breaks in content; unnumbered pages indicated (e.g. hyphen-minus in list item) per 2015-1

### Landmarks

- [ ] Optional; `epub:type="landmarks"` if present

### NCX (required in 2015-1)

- [ ] EPUB contains an NCX file; file name must contain "nav" (e.g. nav.ncx)
- [ ] NCX has .ncx extension
- [ ] Spine `toc` attribute references the manifest `id` of the NCX item

---

## Content Documents

### XHTML Requirements

- [ ] XML declaration: `<?xml version="1.0" encoding="utf-8"?>`
- [ ] DOCTYPE: `<!DOCTYPE html>`
- [ ] File extension .xhtml
- [ ] `<html>` has required attributes: xmlns, xmlns:epub, epub:prefix, xml:lang, lang
- [ ] Namespaces: xmlns=<http://www.w3.org/1999/xhtml>, xmlns:epub=<http://www.idpf.org/2007/ops>
- [ ] **epub:prefix** includes `z3998: <http://www.daisy.org/z3998/2012/vocab/structure/#>`
- [ ] `<head>` children in required order: (1) `<meta charset="UTF-8">`, (2) `<title>` (must match dc:title in package), (3) `<meta name="dc:identifier" content="[production UID]">` (must match package), (4) `<meta name="viewport" content="width=device-width">`
- [ ] `xml:lang` and `lang` same value per content file

### File Naming

Pattern: `[pid]-[XXX].xhtml` or `[pid]-XXX-[epub-type].xhtml`. The values frontmatter, bodymatter, backmatter must **not** appear in the file name.

- [ ] Prefix (pid) identical to `dc:identifier` value
- [ ] XXX = unique numeric string (zero-padded) corresponding to spine order
- [ ] When applicable, epub-type suffix: cover, titlepage, colophon, toc, part, index, appendix, glossary, footnotes, rearnotes (per 3.1.2, 3.1.3)

### Body and structural divisions

- [ ] `<body>` has **epub:type** on every content file; value one of: frontmatter, bodymatter, backmatter (exception: cover uses epub:type="cover")
- [ ] epub:type may have multiple values where required (e.g. "frontmatter toc", "part bodymatter", "backmatter index")
- [ ] Section subdivisions use `<section>`; headings with appropriate `<h1>`–`<h6>`; no nested section as first child of body when heading structure does not warrant it (4.10)

### Headings

- [ ] Heading hierarchy present; no skipped levels
- [ ] `<h1>` for top-level content heading; when pagebreak div is first, `<h1>` follows it
- [ ] Section headings in `<h2>`–`<h6>` as appropriate
- [ ] Typographic emphasis and line breaks in headings removed unless otherwise indicated by ordering agency (4.25)

### Pagination (epub:type="pagebreak")

Block: `<div epub:type="pagebreak" class="page-normal" title="38" id="page-38"/>`  
Inline: `<span epub:type="pagebreak" class="page-normal" title="38" id="page-38"/>`

- [ ] **`epub:type="pagebreak"`** present on all page break elements (required)
- [ ] `role="doc-pagebreak"` **not** required for 2015-1 (optional)
- [ ] **`title`** attribute = page number (2015-1 uses title, not aria-label)
- [ ] **`class`** is one of: `page-normal`, `page-front` (frontmatter when numbering not continued in bodymatter), `page-special` (e.g. appendix numbering, unnumbered inserts)
- [ ] **`id`** present and unique across the EPUB
- [ ] Empty pages marked with pagebreak element; unnumbered pages: class="page-special", id with supplier-generated value, title omitted (4.7.2, 4.7.6)
- [ ] Pagebreak placement at verso-recto change; block `<div>` at section start when page begins with section; inline `<span>` where appropriate (4.7.1)
- [ ] Tables: pagebreak in first `<td>` or `<th>` of new page when page change occurs in table (4.8)

### Images

- [ ] Each image and its caption in `<figure class="image">`; one image per figure (3.1.4.14)
- [ ] `<img>` has **alt** attribute (required). Per 2015-1 (3.2.2): standard value `"image"`; empty alt for publisher logotypes, vignettes, formatting-only images; iconic images: value as provided by ordering agency
- [ ] `<figcaption>` directly after `<img>`; for image series, series caption in figcaption first in parent figure (3.1.4.13)
- [ ] Image series: parent `<figure class="image-series">` containing child figures (4.6)
- [ ] Images stored in folder named **images** (no subfolders); format JPEG, extension .jpg (2.5)
- [ ] Cover image named cover.jpg; manifest item has `properties="cover-image"`
- [ ] Max 600 px on longest side unless legibility of text-rich images requires more (2.5.1, 3.2.1)
- [ ] Aspect ratio maintained; no degradation in colour/legibility per 2015-1 image requirements

### Tables

- [ ] `<caption>` for table title (first child of `<table>` where applicable)
- [ ] `<thead>` for header rows; `<th>` for column/row headings; colspan/rowspan when cell spans (3.1.4.35–41)
- [ ] `<tbody>` for main content; `<tfoot>` when footer within table; otherwise `<p>` (3.1.4.39)
- [ ] `<tr>`, `<td>` for rows and cells
- [ ] Table notes: `<aside>` with epub:type="note", class="notebody", id; not moved from position; reference in table data (3.1.4.42)

### Notes (footnotes, rearnotes, noteref)

- [ ] Note references: `<a href="..." epub:type="noteref" class="noteref">`; href value matches id of associated note (3.1.4.24)
- [ ] Footnotes in separate file [pid]-XXX-footnotes.xhtml; body epub:type="footnotes backmatter"; `<ol>`, each `<li epub:type="footnote" class="notebody" id="...">` (3.1.4.11)
- [ ] Rearnotes / chapter endnotes: file [pid]-XXX-rearnotes.xhtml; body epub:type="rearnotes backmatter" or "rearnotes bodymatter"; `<ol>`, each `<li epub:type="rearnote" class="notebody" id="...">` (3.1.4.6, 3.1.4.29)
- [ ] id on note matches href in noteref; one-to-one relationship

### Lists

- [ ] `<ol>` for ordered lists; manual numbers/formatting removed unless specified; type/start/reversed/value per 2015-1 when needed (3.1.4.19)
- [ ] `<ul>` for unordered lists; bullets/dashes removed from text unless specified (3.1.4.20)
- [ ] `<li>` for each list item (3.1.4.21)
- [ ] `<dl>`, `<dt>`, `<dd>` for definition lists (3.1.4.8–10)
- [ ] Nested lists: use `<p>` inside `<li>` when mixing text and nested list to avoid flow issues (4.2)
- [ ] TOC in content: `<span class="lic">` for heading text and page number (4.18, 3.1.4.22)

### Poetry / verse

- [ ] Container: `<section epub:type="z3998:poem" class="poem">` (2015-1 uses z3998:poem, not z3998:verse)
- [ ] `<div class="linegroup">` and `<p class="line">` as required children (3.1.4.27)
- [ ] Line numbering when present: `<span class="linenum">` (4.22)

### Table of contents in content (when TOC is in a content file)

- [ ] Body has epub:type="frontmatter toc" or "backmatter toc" (4.18)
- [ ] `<ol>` for TOC entries; each entry **linked to the corresponding epub:type="pagebreak"** (not to headings)
- [ ] `<span class="lic">` for chapter title and page number in each list item (4.18)
- [ ] Whitespace between items normalized to single space between spans

### Language tagging

- [ ] Block-level: `xml:lang` (and `lang` where applicable) for longer extracts (at least one full sentence) in a different language (4.15)
- [ ] IETF RFC 3066; ISO 639-1 two-letter code when applicable
- [ ] Inline language markup not required unless specified by ordering agency

### Thematic breaks

- [ ] `<hr class="emptyline"/>` for thematic break represented by empty line (4.11)
- [ ] `<hr class="separator"/>` for thematic break represented by visual marker (e.g. asterisk, rule); marker not rendered in content

### Computer code

- [ ] Inline: `<code>` (3.1.4.7.2)
- [ ] Block: `<pre>` containing `<code>` (3.1.4.7.1); whitespace preserved

### Text emphasis

- [ ] `<strong>` for bold (3.1.4.4)
- [ ] `<em>` for italics (3.1.4.16)
- [ ] Underline / other styling: `<span class="...">` when requested via optional markup (5.3)

### Empty elements

- [ ] No empty elements except: `<img/>`, `<br/>`, `<meta/>`, `<link/>`, `<td/>`, `<div epub:type="pagebreak"/>`, `<span epub:type="pagebreak"/>` (4.24)

---

## Special Content

### Cover

- [ ] File named [pid]-[XXX]-cover.xhtml (3.1.3.1)
- [ ] `<body epub:type="cover">`
- [ ] `<h1>` first in body
- [ ] Sections where applicable: `<section class="frontcover">`, `rearcover`, `leftflap`, `rightflap`
- [ ] Front cover image: cover.jpg, `properties="cover-image"` in manifest
- [ ] Cover itemref in spine has `linear="no"`

### Title page

- [ ] File named [pid]-[XXX]-titlepage.xhtml (3.1.3.2)
- [ ] Body has epub:type="titlepage" (4.16)
- [ ] Full title in `<h1 epub:type="fulltitle" class="title">`
- [ ] Subtitle when present: `<span epub:type="title">` / `<span epub:type="subtitle">`
- [ ] Author: `<p epub:type="z3998:author" class="docauthor">`

### Colophon

- [ ] File [pid]-[XXX]-colophon.xhtml; body epub:type="frontmatter colophon" or "backmatter colophon" (3.1.3.3)
- [ ] ISBN when present in `<p class="isbn">`

### Part, index, appendix, glossary

- [ ] Part: [pid]-XXX-part.xhtml, body epub:type="part bodymatter", part heading in `<h1>` (3.1.3.5)
- [ ] Index: [pid]-XXX-index.xhtml, body epub:type="backmatter index" (3.1.3.6, 4.20)
- [ ] Appendix: [pid]-XXX-appendix.xhtml, body epub:type="appendix backmatter" (3.1.3.7)
- [ ] Glossary: [pid]-XXX-glossary.xhtml, body epub:type="glossary backmatter" (3.1.3.8)

---

## Reference

- Nordic EPUB Guidelines 2015-1: <https://format.mtm.se/nordic_epub/2015-1/>
- Authoritative source: *Requirements for Quality Content Production in EPUB 3.0/XHTML (v.2015-1)* (PDF, Nordic Guidelines 2015-1)
- Comparison with 2025-1: [nordic-epub-checklist.md](nordic-epub-checklist.md)
