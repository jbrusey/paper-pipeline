# paper-pipeline

A skill for reproducible research-paper pipelines: generated values, tables, figures, and Markdown-to-PDF builds.

## Install

### pi

```bash
pi install https://github.com/jbrusey/paper-pipeline
```

Project-local install:

```bash
pi install -l https://github.com/jbrusey/paper-pipeline
```

Pinned install:

```bash
pi install https://github.com/jbrusey/paper-pipeline@v0.1.0
```

### Any Agent Skills-compatible tool

Copy or symlink the skill directory into that tool's skills folder:

```bash
git clone https://github.com/jbrusey/paper-pipeline.git
cp -R paper-pipeline/skills/paper-pipeline ~/.claude/skills/
# or
cp -R paper-pipeline/skills/paper-pipeline ~/.codex/skills/
# or
cp -R paper-pipeline/skills/paper-pipeline ~/.agents/skills/
```

The important file is:

```text
skills/paper-pipeline/SKILL.md
```

## Layout

```text
skills/
  paper-pipeline/
    SKILL.md
```
