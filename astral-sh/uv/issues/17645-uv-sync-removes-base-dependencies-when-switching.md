```yaml
number: 17645
title: "`uv sync` removes base dependencies when switching extras (at least impacts typer/typer-slim)"
type: issue
state: open
author: pimlock
labels:
  - bug
assignees: []
created_at: 2026-01-21T19:40:22Z
updated_at: 2026-01-21T19:40:22Z
url: https://github.com/astral-sh/uv/issues/17645
synced_at: 2026-01-21T20:04:17Z
```

# `uv sync` removes base dependencies when switching extras (at least impacts typer/typer-slim)

---

_@pimlock_

### Summary

# Summary

When switching extras with `uv sync` (or `uv tool install`), packages that provide conflicting files (like `typer` and `typer-slim`) can corrupt the venv. Installing `typer-slim` overwrites `typer`'s files, and uninstalling `typer-slim` deletes them—leaving uv's metadata incorrectly showing typer as installed while the actual files are gone.

# Reproduction

I created a repo with the repro example: https://github.com/pimlock/uv-bug-repro

When this happened in my workflow, the dependency tree was more complex, the base package depended on `typer` and one of deps defined in extra had `typer-slim` as a dep few levels deep.

Here's the verbose output when running all the commands with the spot where the "issue" happens being highlighted
  - https://github.com/pimlock/uv-bug-repro/blob/main/repro-output.txt#L180-L192

Commands:
```
git clone https://github.com/pimlock/uv-bug-repro
cd uv-bug-repro

uv sync                     # ✓ works
uv sync --extra extra1      # ✓ works
uv sync --extra extra2      # ✗ ModuleNotFoundError: No module named 'typer'
```

The package has:
- Base dependency: typer>=0.20.0
- extra1: typer-slim>=0.20.0
- extra2: rich>=13.0.0

## What happens

1. `uv sync` installs typer (which depends on typer-slim)
2. `uv sync --extra extra1` adds typer-slim explicitly
3. `uv sync --extra extra2` removes typer-slim, but also removes typer even though it's a base dependency

This issue is likely caused by how `typer` and `typer-slim` are set up (i.e. `typer` doesn't depend on `typer-slim`), but it may be potentially causing similar issues with other packages (AFAIK pydantic uses the `-slim` pattern, but in a different way).

- typer requires: click, typing-extensions, shellingham, rich
- typer-slim requires: click, typing-extensions

They conflict because they both install files to the same typer/ directory. So here's what's happening:

1. Step 1: `typer` installed → typer/ directory created
2. Step 2: `typer-slim` installed → overwrites typer/ directory
3. Step 3: `typer-slim` uninstalled → deletes typer/ directory

# Expected

Base dependency typer>=0.20.0 should always be installed regardless of which extras are selected.

# Workaround

`uv sync --extra extra2 --reinstall` works correctly.



### Platform

Darwin 25.2.0 arm64

### Version

uv 0.9.26 (ee4f003 2026-01-15)

### Python version

CPython 3.14.0

---

_Label `bug` added by @pimlock on 2026-01-21 19:40_

---
