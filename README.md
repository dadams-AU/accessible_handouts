# Accessible Handouts (LaTeX Template)

This repo contains templates for creating accessible course handouts and assignments with PDF/UA metadata. There is a raw LaTeX template and a Pandoc template that wraps the same accessibility settings.

## Files

- `accessible_assignment_template.tex`: Raw LaTeX template. Edit the placeholders (title, instructor, course, term, sections) and compile.
- `accessible-handout.latex`: Pandoc template. Write your content in Markdown and compile to an accessible PDF via Pandoc + LuaLaTeX.

## Requirements

- **LuaLaTeX** (required for the accessibility features in both templates).
- Standard LaTeX packages used in the templates (e.g., `tagpdf`, `hyperref`, `geometry`, `tabularray`, `fontspec`).

### For the Pandoc template

- **Pandoc** (any recent version)

### Optional but recommended (verification)

- `verapdf` (PDF/UA-2 + Tagged PDF validation)
- `qpdf` (basic PDF structural checks)
- `poppler` tools (`pdfinfo`, `pdffonts`, `pdftotext`)
- `mupdf-tools` (`mutool` for inspecting the PDF catalog/tag tree pointers)

## Quick start

### Raw LaTeX template

1. Copy or rename `accessible_assignment_template.tex`.
2. Replace placeholder values (e.g., `ASSIGNMENT TITLE`, `INSTRUCTOR NAME`, `COURSE CODE -- COURSE TITLE`).
3. Uncomment the logo block if you want to include an image.
4. Compile with LuaLaTeX.

```sh
lualatex accessible_assignment_template.tex
```

### Pandoc template

1. Write your handout content in a Markdown file with a YAML front matter block:

```markdown
---
title: "Assignment 1"
author: "Instructor Name"
date: "Spring 2026"
course-code: "EDUC 101"
course-title: "Introduction to Education"
lang: en-US
---

## Instructions

Your content here...
```

1. Compile with Pandoc using LuaLaTeX and the template:

```sh
pandoc handout.md \
  --template=accessible-handout.latex \
  --pdf-engine=lualatex \
  -o handout.pdf
```

#### Supported front matter variables

| Variable | Description |
| --- | --- |
| `title` | Document title (also sets `pdftitle`) |
| `author` | Author name(s) — accepts a list |
| `date` | Date string shown in the title block |
| `course-code` | Course code (e.g., `EDUC 101`) |
| `course-title` | Course title appended to the code |
| `lang` | BCP 47 language tag (default: `en-US`) |
| `logo` | Path to a logo image file |
| `logo-alt` | Alt text for the logo (default: `Institution logo`) |
| `keywords` | List of PDF keywords |
| `bibliography` | BibTeX file(s) for `natbib` references |

## Accessibility verification (recommended)

This template is designed to generate a **tagged PDF** with **PDF/UA-2**-oriented metadata. The most reliable way to verify is to run a PDF/UA validator on the generated PDF.

### Arch Linux install commands

```sh
sudo pacman -S --needed qpdf poppler mupdf-tools
```

For `verapdf`, use your AUR helper (e.g., `yay` or `paru`) or install the official release bundle:

```sh
# example if you use yay
yay -S --needed verapdf
```

### Run checks

```sh
# build
lualatex -interaction=nonstopmode -halt-on-error accesible_assignment_template.tex

# verify PDF/UA-2 + tagged PDF
verapdf accesible_assignment_template.pdf

# optional basic checks
qpdf --check accesible_assignment_template.pdf
pdfinfo -meta accesible_assignment_template.pdf | head
mutool show accesible_assignment_template.pdf 83
```

If `verapdf` reports non-compliance, treat it as the source of truth and fix the template/content until it passes.


## Notes

- The template sets PDF/UA settings and language in `\DocumentMetadata`.
- Descriptive metadata (title/author/subject/keywords) is set via `\hypersetup`.
- `pdfdisplaydoctitle=true` is enabled so the PDF catalog includes `ViewerPreferences/DisplayDocTitle=true` (required by common PDF/UA validation profiles).
- If `tagpdf` is available, it enables tagging for accessibility (LuaLaTeX required).
- The font defaults to TeX Gyre Heros/TeX Gyre Cursor if available, otherwise Latin Modern.

## License

This repository is licensed under **Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)**.

See [LICENSE](LICENSE).

## Contributing

Contributions are welcome! Please open issues or submit pull requests for improvements or bug fixes.