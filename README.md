# Accessible Handouts (LaTeX Template)

This repo contains a single LaTeX template for creating accessible course handouts and assignments with PDF/UA metadata.

## Files

- `accesible_assignment_template.tex`: Main template. Edit the placeholders (title, instructor, course, term, sections) and compile.

## Requirements

- **LuaLaTeX** (required for the accessibility features in this template).
- Standard LaTeX packages used in the template (e.g., `tagpdf`, `hyperref`, `geometry`).

### Optional but recommended (verification)

- `verapdf` (PDF/UA-2 + Tagged PDF validation)
- `qpdf` (basic PDF structural checks)
- `poppler` tools (`pdfinfo`, `pdffonts`, `pdftotext`)
- `mupdf-tools` (`mutool` for inspecting the PDF catalog/tag tree pointers)

## Quick start

1. Copy or rename `accesible_assignment_template.tex`.
2. Replace placeholder values (e.g., `ASSIGNMENT TITLE`, `INSTRUCTOR NAME`, `COURSE CODE -- COURSE TITLE`).
3. Uncomment the logo block if you want to include an image.
4. Compile with LuaLaTeX.

Example (LuaLaTeX):

```sh
lualatex accesible_assignment_template.tex
```

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

If you want additional templates or fields, open a PR with a short rationale and sample output.
