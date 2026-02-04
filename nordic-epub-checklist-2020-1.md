# Nordic EPUB 2020-1 Checklist

Checklist for **Nordic Guidelines for Production of Accessible EPUB 3, version 2020-1**. Use only when the EPUB declares 2020-1 (`nordic:guidelines` or `dcterms:conformsTo`).

**Verify requirements against the official guideline:** [Nordic EPUB 2020-1](https://format.mtm.se/nordic_epub/2020-1/). This checklist reflects typical 2020-1 requirements; the official document is authoritative. For comparison with 2025-1 see [nordic-epub-checklist.md](nordic-epub-checklist.md). The structure below follows the same sections as the 2025-1 checklist, with 2020-1 exceptions stated where they apply.

## Version-specific rules (2020-1)

Compared with 2025-1, 2020-1 does **not** require:

- `role="doc-toc"` / `role="doc-pagelist"` on nav elements (epub:type is sufficient; **role recommended**)
- `role="doc-pagebreak"` on page break elements (epub:type="pagebreak" is sufficient; **role recommended**)
- The full 2025-1 accessibility metadata set; 2020-1 specifies its own set in the official guideline

2020-1 **does not** require:

- NCX document (2020-1 uses EPUB 3 nav only; unlike 2015-1)

Page number on page break: consult the 2020-1 spec for whether `title` or `aria-label` is required.

---

## Package Document (package.opf)

### Root Element

```xml
<package xmlns="http://www.idpf.org/2007/opf"
         xmlns:dc="http://purl.org/dc/elements/1.1/"
         prefix="nordic: http://www.mtm.se/epub/"
         version="3.0"
         xml:lang="[publication language]"
         unique-identifier="pub-identifier">
```

- [ ] `xmlns` namespace correct (<http://www.idpf.org/2007/opf>)
- [ ] `xmlns:dc` namespace correct (<http://purl.org/dc/elements/1.1/>)
- [ ] `prefix` includes `nordic: http://www.mtm.se/epub/`
- [ ] `version="3.0"`
- [ ] `xml:lang` matches publication language (consult 2020-1 spec)
- [ ] `unique-identifier` references `dc:identifier` (value pub-identifier)
- [ ] Package document filename is package.opf

### Required Metadata

Per 2020-1 spec, the following are required:

- [ ] `dc:identifier` with content = production UID; attribute `id="pub-identifier"`
- [ ] `dc:title` with publication title
- [ ] `dc:language` with RFC 5646 conformant value (e.g. nb, nn, sv, da, fi)
- [ ] `dc:date` in YYYY-MM-DD format
- [ ] `dc:publisher` (name of ordering agency)
- [ ] `meta property="dcterms:modified"` with value CCYY-MM-DDThh:mm:ssZ (last change by supplier)
- [ ] `dc:creator` (author of publication)
- [ ] `dc:source` with value containing ISBN of the publication (format per 2020-1 spec)
- [ ] `meta property="nordic:guidelines"` with value **2020-1**
- [ ] `meta property="nordic:supplier"` (name of supplier)

### Accessibility metadata

- [ ] Consult the official 2020-1 guideline for the required accessibility metadata set. Do not assume the 2025-1 schema set is required.

### Manifest

- [ ] All publication documents and resources listed in `<manifest>`; each as an `<item>` element
- [ ] Fallback attribute applied for each item that references a fallback resource when applicable
- [ ] Navigation document (nav.xhtml) has `properties="nav"`
- [ ] Cover image has `properties="cover-image"`
- [ ] All `href` paths relative and correct
- [ ] Correct `media-type` for each item; MathML files have `properties="mathml"` when present

### Spine

- [ ] Reading order (sequence of `<itemref>`) corresponds to structure in source material unless otherwise requested
- [ ] Cover `itemref` has `linear="no"` where applicable
- [ ] `idref` values match manifest `id` values
- [ ] Chapter endnotes / rearnotes: `linear="no"` on corresponding itemref when epub:type="rearnotes bodymatter"

---

## Navigation Document (nav.xhtml)

File name must contain the string "nav" (e.g. nav.xhtml). Required: `<nav epub:type="toc">` and `<nav epub:type="page-list">` when paginated. **role="doc-toc"** and **role="doc-pagelist"** are **recommended** for 2020-1 (not required as in 2025-1).

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
- [ ] **`role="doc-toc"`** recommended for 2020-1 (not required)
- [ ] Heading and list structure; links to sections or pagebreak targets as per 2020-1 (see Content TOC below)

### Page List (if paginated)

```html
<nav epub:type="page-list">
  <h1>[Language-specific heading]</h1>
  <ol>...</ol>
</nav>
```

- [ ] **`epub:type="page-list"`** present when work is paginated (required)
- [ ] **`role="doc-pagelist"`** recommended for 2020-1 (not required)
- [ ] Page list entries match page breaks in content; unnumbered pages indicated (e.g. hyphen-minus in list item) per 2020-1

### Landmarks

- [ ] Optional; `epub:type="landmarks"` if present

---

## Content Documents

### XHTML Requirements

- [ ] XML declaration: `<?xml version="1.0" encoding="utf-8"?>`
- [ ] DOCTYPE: `<!DOCTYPE html>`
- [ ] File extension .xhtml
- [ ] `<html>` has required attributes: xmlns, xmlns:epub, epub:prefix, xml:lang, lang
- [ ] Namespaces: xmlns=<http://www.w3.org/1999/xhtml>, xmlns:epub=<http://www.idpf.org/2007/ops>
- [ ] **epub:prefix** includes `z3998: <http://www.daisy.org/z3998/2012/vocab/structure/#>` when using z3998 semantics
- [ ] `<head>` children in required order: (1) `<meta charset="UTF-8">`, (2) `<title>` (must match dc:title in package or document heading), (3) `<meta name="dc:identifier" content="[production UID]">` (must match package), (4) `<meta name="viewport" content="width=device-width">`
- [ ] `xml:lang` and `lang` same value per content file

### File Naming

Pattern: `[pid]-[XXX].xhtml` or `[pid]-XXX-[epub-type].xhtml`. Consult 2020-1 spec for allowed suffixes.

- [ ] Prefix (pid) identical to `dc:identifier` value
- [ ] XXX = unique numeric string (zero-padded) corresponding to spine order
- [ ] When applicable, epub-type suffix: cover, titlepage, colophon, toc, part, index, appendix, glossary, footnotes, rearnotes (per 2020-1 spec)

### Body and structural divisions

- [ ] `<body>` has **epub:type** on every content file; value one of: frontmatter, bodymatter, backmatter (exception: cover uses epub:type="cover")
- [ ] epub:type may have multiple values where required (e.g. "frontmatter toc", "part bodymatter", "backmatter index")
- [ ] Section subdivisions use `<section>`; headings with appropriate `<h1>`–`<h6>`; role/epub:type as per 2020-1 spec
- [ ] No nested section as first child of body when heading structure does not warrant it

### Headings

- [ ] Heading hierarchy present; no skipped levels
- [ ] `<h1>` for top-level content heading; when pagebreak div is first, `<h1>` follows it
- [ ] Section headings in `<h2>`–`<h6>` as appropriate
- [ ] Typographic emphasis and line breaks in headings removed unless otherwise indicated by ordering agency

### Pagination (epub:type="pagebreak")

Block (with page number per 2020-1 spec – consult for title vs aria-label):

```html
<div epub:type="pagebreak" class="page-normal" id="page-38" aria-label="38"/>
```

Inline:

```html
<span epub:type="pagebreak" class="page-normal" id="page-38" aria-label="38"/>
```

- [ ] **`epub:type="pagebreak"`** present on all page break elements (required)
- [ ] **`role="doc-pagebreak"`** recommended for 2020-1 (not required)
- [ ] Page number attribute: consult 2020-1 spec for **`title`** vs **`aria-label`**
- [ ] **`class`** is one of: `page-normal`, `page-front` (frontmatter when numbering not continued in bodymatter), `page-special` (e.g. appendix numbering, unnumbered inserts)
- [ ] **`id`** present and unique across the EPUB
- [ ] Empty pages marked with pagebreak element; unnumbered pages: class="page-special", id with supplier-generated value; page number attribute omitted per 2020-1
- [ ] Pagebreak placement at verso-recto change; block `<div>` at section start when page begins with section; inline `<span>` where appropriate
- [ ] Tables: pagebreak in first `<td>` or `<th>` of new page when page change occurs in table

### Images

- [ ] Each image and its caption in `<figure class="image">`; one image per figure
- [ ] `<img>` has **alt** attribute (required). Per 2020-1: use standard values or meaningful description; empty alt only where appropriate (e.g. decorative)
- [ ] `<figcaption>` directly after `<img>`; for image series, series caption in figcaption first in parent figure
- [ ] Image series: parent `<figure class="image-series">` containing child figures
- [ ] Images stored in folder named **images** (or as per 2020-1 spec); format and naming per 2020-1
- [ ] Cover image named cover.jpg (or cover.png); manifest item has `properties="cover-image"`
- [ ] Max dimensions per 2020-1 spec (e.g. longest side); aspect ratio maintained

### Tables

- [ ] `<caption>` for table title (first child of `<table>` where applicable)
- [ ] `<thead>` for header rows; `<th>` for column/row headings; colspan/rowspan when cell spans
- [ ] `<tbody>` for main content; `<tfoot>` when footer within table; otherwise `<p>`
- [ ] `<tr>`, `<td>` for rows and cells
- [ ] Table notes: `<aside>` with epub:type="note", class="notebody", id; not moved from position; reference in table data

### Notes (footnotes, rearnotes, noteref)

- [ ] Note references: `<a href="..." epub:type="noteref" class="noteref">`; href value matches id of associated note
- [ ] Footnotes in separate file [pid]-XXX-footnotes.xhtml; body epub:type="footnotes backmatter"; `<ol>`, each `<li epub:type="footnote" class="notebody" id="...">`
- [ ] Rearnotes / chapter endnotes: file [pid]-XXX-rearnotes.xhtml; body epub:type="rearnotes backmatter" or "rearnotes bodymatter"; `<ol>`, each `<li epub:type="rearnote" class="notebody" id="...">`
- [ ] id on note matches href in noteref; one-to-one relationship

### Lists

- [ ] `<ol>` for ordered lists; manual numbers/formatting removed unless specified
- [ ] `<ul>` for unordered lists; bullets/dashes removed from text unless specified
- [ ] `<li>` for each list item
- [ ] `<dl>`, `<dt>`, `<dd>` for definition lists
- [ ] Nested lists: use `<p>` inside `<li>` when mixing text and nested list to avoid flow issues
- [ ] TOC in content: `<span class="lic">` for heading text and page number when TOC is in content file (per 2020-1)

### Poetry / verse

- [ ] Container: `<section epub:type="z3998:poem" class="poem">` or as per 2020-1 (z3998:poem or z3998:verse)
- [ ] `<div class="linegroup">` and `<p class="line">` as required children
- [ ] Line numbering when present: `<span class="linenum">`

### Table of contents in content (when TOC is in a content file)

- [ ] Body has epub:type="frontmatter toc" or "backmatter toc"
- [ ] `<ol>` for TOC entries; each entry linked to the corresponding epub:type="pagebreak" or section per 2020-1
- [ ] `<span class="lic">` for chapter title and page number in each list item
- [ ] Whitespace between items normalized to single space between spans

### Language tagging

- [ ] Block-level: `xml:lang` (and `lang` where applicable) for longer extracts (at least one full sentence) in a different language
- [ ] IETF RFC 5646; ISO 639-1 two-letter code when applicable
- [ ] Inline language markup not required unless specified by ordering agency

### Thematic breaks

- [ ] `<hr class="emptyline"/>` for thematic break represented by empty line
- [ ] `<hr class="separator"/>` for thematic break represented by visual marker (e.g. asterisk, rule); marker not rendered in content

### Computer code

- [ ] Inline: `<code>`
- [ ] Block: `<pre>` containing `<code>`; whitespace preserved

### Text emphasis

- [ ] `<strong>` for bold
- [ ] `<em>` for italics
- [ ] Underline / other styling: `<span class="...">` when requested via optional markup

### Empty elements

- [ ] No empty elements except: `<img/>`, `<br/>`, `<meta/>`, `<link/>`, `<td/>`, `<div epub:type="pagebreak"/>`, `<span epub:type="pagebreak"/>`

---

## Special Content

### Cover

- [ ] File named [pid]-[XXX]-cover.xhtml
- [ ] `<body epub:type="cover">`
- [ ] `<h1>` first in body
- [ ] Sections where applicable: `<section class="frontcover">`, `rearcover`, `leftflap`, `rightflap`
- [ ] Front cover image: cover.jpg (or cover.png), `properties="cover-image"` in manifest
- [ ] Cover itemref in spine has `linear="no"`

### Title page

- [ ] File named [pid]-[XXX]-titlepage.xhtml
- [ ] Body has epub:type="titlepage"
- [ ] Full title in `<h1 epub:type="fulltitle" class="title">`
- [ ] Subtitle when present: `<span epub:type="title">` / `<span epub:type="subtitle">`
- [ ] Author: `<p epub:type="z3998:author" class="docauthor">`

### Colophon

- [ ] File [pid]-[XXX]-colophon.xhtml; body epub:type="frontmatter colophon" or "backmatter colophon"
- [ ] ISBN when present in `<p class="isbn">`

### Part, index, appendix, glossary

- [ ] Part: [pid]-XXX-part.xhtml, body epub:type="part bodymatter", part heading in `<h1>`
- [ ] Index: [pid]-XXX-index.xhtml, body epub:type="backmatter index"
- [ ] Appendix: [pid]-XXX-appendix.xhtml, body epub:type="appendix backmatter"
- [ ] Glossary: [pid]-XXX-glossary.xhtml, body epub:type="glossary backmatter"

---

## Reference

- Nordic EPUB Guidelines 2020-1: <https://format.mtm.se/nordic_epub/2020-1/>
- Comparison with 2025-1: [nordic-epub-checklist.md](nordic-epub-checklist.md)
