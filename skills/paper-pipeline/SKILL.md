---
name: paper-pipeline
description: Use this skill when developing a research paper where results, tables, figures, and manuscript text are generated reproducibly from scripts.
---

## When active

Maintain a reproducible paper pipeline: every reported value, table, figure, and final manuscript artefact should be generated from source data, configuration, scripts, or manuscript text.

Use the existing project structure and build system. Do not restructure the repository unless asked. If no build system exists, add the smallest `Makefile` or equivalent script that builds the needed artefacts.

## Agent workflow

1. Inspect the build system first (`Makefile`, Quarto, RMarkdown, Snakemake, npm scripts, shell scripts, etc.).
2. Classify files before editing: source or generated.
3. Edit source files only: data, config, scripts, manuscript, bibliography, build rules.
4. If a displayed value is wrong, trace it back to the script, data, or config that produced it; fix that source, not the rendered output.
5. Rebuild the smallest relevant target after changes.
6. Run the full paper build before final completion when feasible.
7. Report source files changed, generated files rebuilt, commands run, and whether the build passed.

## Hard rules

- Do not hand-edit generated values, tables, figures, LaTeX, or PDFs unless explicitly asked.
- Do not hard-code numerical results in prose.
- Generate reported values from scripts, preferably as LaTeX macros or included files.
- Keep raw data, processed data, generated artefacts, scripts, and manuscript text distinct.
- Keep generated artefacts out of version control unless the project explicitly requires them.
- Fix random seeds where results depend on randomness.
- Preserve reproducibility over convenience.

## New project default layout

For a new paper repo, prefer this minimal shape:

```text
paper/
  Makefile
  paper.md
  references.bib
  config/
  data/raw/
  data/processed/
  scripts/
  build/
```

## Generated values

Values used in prose, such as sample counts, error rates, confidence intervals, or percentage improvements, should be generated into a file such as `build/values.tex`:

```latex
\newcommand{\AbstractImprovement}{\SI{12.4}{\percent}}
\newcommand{\BestRMSE}{\num{0.83}}
\newcommand{\NumParticipants}{\num{42}}
```

The manuscript should reference the generated value:

```latex
Our method improved performance by \AbstractImprovement{} compared with the baseline.
```

## Tables and figures

- Generate tables from scripts or notebooks invoked by the build system.
- Format numerical table values consistently, e.g. with `siunitx` for LaTeX papers.
- Generate figures from source data/config and write them to the build directory.
- Avoid manual post-processing; encode styling and filtering in the generation script.

## Venue / journal style files

When a venue provides LaTeX style requirements, keep the style inputs as source files and invoke them from the build rule rather than editing generated LaTeX.

Use the smallest Pandoc mechanism that fits:

- `-V documentclass=...` and `-V classoption=...` for venue `.cls` files.
- `--include-in-header config/header.tex` for packages, macros, or `.sty` setup.
- `--template config/template.tex` only when the venue requires a specific LaTeX document skeleton.

Example:

```make
build/paper.pdf: paper.md references.bib config/template.tex config/header.tex
	pandoc paper.md \
	  --output build/paper.pdf \
	  --bibliography references.bib \
	  --citeproc \
	  --template config/template.tex \
	  --include-in-header config/header.tex
```

Do not hand-edit generated `.tex` to satisfy venue formatting; update the template, header, metadata, or build rule instead.

## Build pattern

Each generated artefact should have an explicit dependency path from source to output. Example:

```make
all: build/paper.pdf

build/values.tex: scripts/generate_metrics.py data/processed/results.csv
	python scripts/generate_metrics.py

build/paper.pdf: paper.md build/values.tex references.bib
	pandoc paper.md --output build/paper.pdf --bibliography references.bib --citeproc --include-in-header build/values.tex

clean:
	rm -rf build/*
```

Before calling the work complete, prefer `make clean && make all` or the project’s equivalent full rebuild.
