---
name: nordic-epub-evaluation
description: Evaluate unzipped EPUB files against the Nordic Accessible EPUB Guidelines (2015-1, 2020-1, or 2025-1), WCAG 2.2 and Nordic MathML Guidelines; includes validation towards Daisy Pipeline (nordic-epub3-validate). Use when reviewing EPUB accessibility, checking EPUB compliance, or working with unzipped EPUB directories.
metadata:
  version: "1.0.4"
---

# Nordic EPUB Evaluation

Evaluate unzipped EPUBs against Nordic accessibility standards. **Validate only against the guideline version the EPUB declares**; do not apply requirements from other versions. Evaluation can include **validation against the Daisy Pipeline** (nordic-epub3-validate); this skill covers **manual** evaluation and reporting against Nordic/WCAG/MathML, and can be used to interpret or supplement automated Pipeline reports.

## Before Starting

**Important**: This skill assumes the EPUB is unzipped. Ask the user for the path to the unzipped EPUB directory before proceeding.

Evaluation can include **validation against the Daisy Pipeline** (the nordic-epub3-validate job). This skill covers **manual** evaluation and reporting against Nordic/WCAG/MathML, and can be used to interpret or supplement reports from automated validation.

Typical EPUB structure:

```text
epub-directory/
├── mimetype
├── META-INF/
│   └── container.xml
└── EPUB/
    ├── package.opf
    ├── nav.xhtml
    ├── content files (.xhtml)
    ├── css/
    ├── images/
    └── fonts/ (optional)
```

## Validation against the Daisy Pipeline

EPUB files can be validated against the **Daisy Pipeline job "nordic-epub3-validate"** (official validator: nordic-epub3-dtbook-migrator). The job checks Nordic EPUB 3 guidelines; the report is typically delivered as `report.xhtml` (e.g. under `html-report/report.xhtml` in the Pipeline output). Manual evaluation in this skill should be consistent with and supplement what the Daisy Pipeline reports.

## Evaluation Workflow

### Phase 0: Identify declared guideline (do this first)

1. **Locate package document**: Read `META-INF/container.xml` to find `package.opf` path.
2. **Read package.opf metadata** and determine which Nordic guideline set the EPUB declares:
   - `<meta property="nordic:guidelines">2025-1</meta>` or `<link rel="dcterms:conformsTo" href=".../2025-1"/>` → **validate against 2025-1 only**
   - `<meta property="nordic:guidelines">2020-1</meta>` or `<link rel="dcterms:conformsTo" href=".../2020-1"/>` → **validate against 2020-1 only**
   - `<meta property="nordic:guidelines">2015-1</meta>` or `<link rel="dcterms:conformsTo" href=".../2015-1"/>` → **validate against 2015-1 only**
   - If none of the above is present: state in the report that no guideline was declared, and **default to 2025-1** for the evaluation.
3. **Record the target version** and use only the requirement set for that version in Phases 1–3. Do not report as failures requirements that apply only to a different version (e.g. do not require 2025-1 nav roles when the EPUB declares 2015-1).

### Phase 1: Structural Analysis

1. **Analyze package.opf**: Check metadata, manifest, spine (against the identified guideline).
2. **Check navigation**: Review `nav.xhtml` structure (see version-specific requirements below).
3. **Sample content files**: Examine representative XHTML files.

### Phase 2: Compliance Checks (version-specific)

Run checks in this order, applying **only** the requirements for the declared guideline version:

1. **Package metadata** – Dublin Core and any accessibility metadata required by that version.
2. **Navigation document** – TOC, page-list, landmarks per version (see table below).
3. **Content structure** – Semantic HTML, heading hierarchy.
4. **Images** – Alt text, figure markup (all versions).
5. **Tables** – Headers, captions, scope (all versions).
6. **MathML** (if present) – Run validation against the [mathml-checklist-nlb-production.md](mathml-checklist-nlb-production.md) (NLB MathML Guidelines, 2022). See the addendum below for changes required to upgrade to the newer MathML guideline version.
7. **Page breaks** – Markers and page-list alignment per version (see table below).

### Phase 3: Report Generation

Generate the report using the template below. **Guidelines** in the report must list only the identified version (e.g. "Nordic EPUB 2015-1") and WCAG 2.2. When MathML is present, include results for the MathML checklist ([mathml-checklist-nlb-production.md](mathml-checklist-nlb-production.md)); if relevant, add a note on what would need to change for the newer MathML guideline version (see addendum). References section should include the URL for that Nordic version plus WCAG and MathML.

## Quick Reference

### Version-specific requirements (validate only the row for the declared version)

| Requirement | 2015-1 | 2020-1 | 2025-1 |
|-------------|--------|--------|--------|
| **Nav TOC** | `nav[epub:type="toc"]` required | `nav[epub:type="toc"]`; role="doc-toc" recommended | `nav[role="doc-toc"][epub:type="toc"]` required |
| **Nav page-list** | If paginated: `nav[epub:type="page-list"]` | Same + role="doc-pagelist" recommended | `nav[role="doc-pagelist"][epub:type="page-list"]` required when paginated |
| **Landmarks** | Optional | Optional | Optional |
| **Page breaks in content** | `epub:type="pagebreak"` | Same; role="doc-pagebreak" recommended | `epub:type="pagebreak"` and `role="doc-pagebreak"` required |
| **Accessibility metadata in package** | Check 2015-1 spec for required set | Check 2020-1 spec for required set | schema:accessMode, accessModeSufficient, accessibilityFeature, accessibilityHazard, dcterms:conformsTo, a11y:certifiedBy (see below) |

When the EPUB declares 2015-1 or 2020-1, **do not** report missing 2025-1-only requirements (e.g. nav roles, doc-pagebreak role) as failures; you may mention them as optional improvements if desired.

### Required Package Metadata (all versions)

```xml
<dc:title id="title">[title]</dc:title>
<dc:language>[language code]</dc:language>
<dc:identifier id="pub-identifier">[production number]</dc:identifier>
<dc:source>[ISBN]</dc:source>
<dc:creator>[author]</dc:creator>
<dc:publisher>[ordering agency]</dc:publisher>
<dc:date>[completion date]</dc:date>
<meta property="dcterms:modified">[completion date]</meta>
<meta property="nordic:supplier">[supplier name]</meta>
<meta property="nordic:guidelines">[2015-1|2020-1|2025-1]</meta>
```

### Required Accessibility Metadata (2025-1; check 2015-1/2020-1 spec for older versions)

For EPUBs declaring 2025-1, minimum for simple book:

```xml
<meta property="schema:accessMode">textual</meta>
<meta property="schema:accessModeSufficient">textual</meta>
<meta property="schema:accessibilityFeature">displayTransformability</meta>
<meta property="schema:accessibilityFeature">structuralNavigation</meta>
<meta property="schema:accessibilityFeature">tableOfContents</meta>
<meta property="schema:accessibilityFeature">readingOrder</meta>
<meta property="schema:accessibilityHazard">none</meta>
<link rel="dcterms:conformsTo" href="https://format.mtm.se/nordic_epub/2025-1"/>
<meta property="a11y:certifiedBy">[ordering agency]</meta>
```

For 2015-1 or 2020-1, consult the official guideline document for the required accessibility metadata set; do not require the full 2025-1 set unless the EPUB declares 2025-1.

### Navigation and content (apply only the row for the declared version)

Navigation: see version-specific table above. Content document checks below apply to all versions except page breaks (role="doc-pagebreak" required only for 2025-1).

| Element | Requirement (all versions unless noted) |
|---------|----------------------------------------|
| `<html>` | `xmlns`, `xmlns:epub`, `xml:lang`, `lang` attributes |
| `<section>` | `role` and/or `epub:type` for semantic sections |
| `<h1>`-`<h6>` | Correct hierarchy, no skipped levels |
| `<img>` | `alt` attribute always present |
| `<figure>` | Contains `<img>` and optionally `<figcaption>` |
| `<table>` | `<caption>`, `<thead>`, `<th scope>` where appropriate |
| Page breaks | `epub:type="pagebreak"` (all); **2025-1 only**: also `role="doc-pagebreak"` |

### Image Alt Text Values

Use these default values when meaningful alt text is not provided:

| Norwegian | English | Use for |
|-----------|---------|---------|
| Foto. | Photo. | Photographs |
| Illustrasjon. | Illustration. | Illustrations |
| Figur. | Figure. | Diagrams, charts |
| Symbol. | Symbol. | Icons, signs |
| Kart. | Map. | Maps |
| Tegning. | Drawing. | Drawings |
| Tegneserie. | Comic. | Comics |
| Logotyp. | Logo. | Logos |

### MathML Quick Checks

When MathML is present, validate against the **NLB MathML Guidelines checklist** ([mathml-checklist-nlb-production.md](mathml-checklist-nlb-production.md), 2022):

- `<math xmlns="http://www.w3.org/1998/Math/MathML">`; no `<mfenced>` (deprecated); no `<mlabeledtr>`; invisible operators `&#x2062;` (multiplication), `&#x2061;` (function application); units with `mathvariant="normal"` for single letters. See the full checklist for all rules.

**Addendum — upgrading to the new MathML guideline version:** To evaluate against the newer MathML guidelines (or to report what would need to change for an EPUB to comply), use [mathml-guidelines-differences.md](mathml-guidelines-differences.md), which describes the differences between the 2022 checklist and the newer requirements (structure/semantics, display, mfenced vs mo, invisible operators, alttext/altimg, etc.).

## Report Template

```markdown
# EPUB Accessibility Evaluation Report

**File**: [EPUB directory path]
**Evaluated**: [date]
**Declared guideline**: [version from package.opf, or "none declared"]
**Guidelines used for validation**: Nordic EPUB [2015-1 | 2020-1 | 2025-1] only, WCAG 2.2

(If no guideline was declared, state "No nordic:guidelines/dcterms:conformsTo found; validated against 2025-1 by default.")

## Summary

| Category | Status | Issues |
|----------|--------|--------|
| Package Metadata | ✅/⚠️/❌ | [count] |
| Accessibility Metadata | ✅/⚠️/❌ | [count] |
| Navigation | ✅/⚠️/❌ | [count] |
| Content Structure | ✅/⚠️/❌ | [count] |
| Images | ✅/⚠️/❌ | [count] |
| Tables | ✅/⚠️/❌ | [count] |
| MathML | ✅/⚠️/❌/N/A | [count] |
| Page Breaks | ✅/⚠️/❌/N/A | [count] |

**Overall**: [PASS/NEEDS WORK/FAIL]

## Detailed Findings

### 🔴 Critical Issues (must fix)

[List critical issues with file paths and line references.]

### 🟡 Warnings (should fix)

[List warnings with recommendations.]

### 🟢 Recommendations (nice to have)

[List optional improvements]

### MathML: Addendum for upgrading to the new guideline version

(Include when MathML is present and relevant.) Validation was run against the Nordic MathML checklist (2022). For changes required to upgrade this EPUB to the newer MathML guideline version, see [mathml-guidelines-differences.md](mathml-guidelines-differences.md).

## Files Examined

- package.opf
- nav.xhtml
- [list of content files checked]

## References

- Nordic EPUB Guidelines [version used]: https://format.mtm.se/nordic_epub/[2015-1|2020-1|2025-1]
- Nordic MathML Guidelines: https://github.com/nlbdev/mathml-guidelines
- WCAG 2.2: https://www.w3.org/TR/WCAG22/
- DAISY Knowledge Base: https://kb.daisy.org/publishing/docs/
```

## Severity Classification

Apply severity **only for requirements of the declared guideline version**. Do not treat "missing 2025-1 requirement" as critical when the EPUB declares 2015-1.

| Level | Criteria |
|-------|----------|
| 🔴 Critical | Breaks accessibility for the declared version; missing required elements or invalid structure per that version |
| 🟡 Warning | Suboptimal but functional; missing recommended elements per that version |
| 🟢 Recommendation | Enhancements, or requirements from a newer guideline version (mention only if useful) |

## Additional Resources

- **Daisy Pipeline**: Nordic validation uses the job nordic-epub3-validate; the official validator is nordic-epub3-dtbook-migrator.
- For detailed structural requirements, use the checklist for the **declared version only**:
  - 2025-1: [nordic-epub-checklist.md](nordic-epub-checklist.md) (2025-1)
  - 2020-1: [nordic-epub-checklist-2020-1.md](nordic-epub-checklist-2020-1.md)
  - 2015-1: [nordic-epub-checklist-2015-1.md](nordic-epub-checklist-2015-1.md)
- For MathML validation: [mathml-checklist-nlb-production.md](mathml-checklist-nlb-production.md) (NLB MathML Guidelines, 2022). For changes required to upgrade to the newer MathML guideline version: [mathml-guidelines-differences.md](mathml-guidelines-differences.md).
- For WCAG mapping: [wcag-mapping.md](wcag-mapping.md)
