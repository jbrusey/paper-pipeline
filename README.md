# paper-pipeline

A skill for reproducible research-paper pipelines: generated values, tables, figures, and Markdown-to-PDF builds.

## Install

In Codex or Claude Code, enter:

```text
Install https://github.com/jbrusey/paper-pipeline as a skill.
```

The coding agent should clone the repository and install the `paper-pipeline` skill into the appropriate skills directory for the current tool.

After installation, ask the agent to use it:

```text
Use the paper-pipeline skill to set up this paper repository.
```

The skill itself is located at:

```text
skills/paper-pipeline/SKILL.md
```

### pi

```bash
pi install git:github.com/jbrusey/paper-pipeline
```

Project-local install:

```bash
pi install -l git:github.com/jbrusey/paper-pipeline
```

Pinned install:

```bash
pi install git:github.com/jbrusey/paper-pipeline@v0.1.0
```

`https://github.com/jbrusey/paper-pipeline` also works; the `git:` form is just shorter.

### Any Agent Skills-compatible tool

Copy or symlink the skill directory into that tool's skills folder:

```bash
git clone https://github.com/jbrusey/paper-pipeline.git
cp -R paper-pipeline/skills/paper-pipeline ~/.claude/skills/
# or
cp -R paper-pipeline/skills/paper-pipeline ~/.agents/skills/
```

The important file is:

```text
skills/paper-pipeline/SKILL.md
```
