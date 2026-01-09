---
number: 14587
title: Display the new entry points (uv run foo) at the end of installing a project or package
type: issue
state: open
author: oxysoft
labels:
  - enhancement
assignees: []
created_at: 2025-07-13T13:38:03Z
updated_at: 2025-09-08T14:39:23Z
url: https://github.com/astral-sh/uv/issues/14587
synced_at: 2026-01-07T13:12:18-06:00
---

# Display the new entry points (uv run foo) at the end of installing a project or package

---

_Issue opened by @oxysoft on 2025-07-13 13:38_

### Summary

This is a small pet peeve of mine but it took me the longest time to understand the entry point stuff in Python. I will admit that I'm usually far too late to the docs, and that's because I feel that we should instead design the world like a Metroidvania, breadcrumbing the possibilities.

### Example

The idea is simple: at the end of pip install, the output would display a list of any new entry point available to the user, with `uv pip install foo` or `uv pip install -e .` Even better if it says something like `The package X offers Y entry points in its pyproject.toml` or wherever it comes from!

---

_Label `enhancement` added by @oxysoft on 2025-07-13 13:38_

---

_Comment by @griffinmilsap on 2025-09-03 18:12_

I'll +1 this.  I use `uv` across a lot of projects, and I define entrypoints in `[project.scripts]` in many of these projects.  I don't typically remember the names, so when I come back to a project after a while, my first command is typically `cat pyproject.toml` then scroll through to look for the `[project.scripts]` to remember what my entry points are.  Something like a `uv info` would be helpful, to just cat a quick summary of the pyproject.toml with package name, optional dependencies, entrypoints, etc.  If a uv-managed .venv is present, it could output things like python version installed and other venv-relevant info too.  

... Then again, maybe that's just trivial feature bloat when you can just cat the `pyproject.toml` which is already pretty human readable ...  

---

_Comment by @zanieb on 2025-09-03 18:35_

As a note, `uv run` without arguments will list the available entry points

```
❯ uv run
Provide a command or script to invoke with `uv run <command>` or `uv run <script>.py`.

The following commands are available in the environment:

- glom
- jsonschema
- markdown-it
- normalizer
- opentelemetry-bootstrap
- opentelemetry-instrument
- pwiz.py
- pygmentize
- pysemgrep
- python
- python3
- python3.14
- semgrep
```

(This is a bit different than showing them in `pip install` though)

---

_Comment by @griffinmilsap on 2025-09-08 14:39_

Well THATS really cool!  That actually meets my needs!

---
