---
number: 17217
title: "`uv venv`: flag to disable activate/pydoc script generation"
type: issue
state: open
author: paveldikov
labels:
  - enhancement
assignees: []
created_at: 2025-12-22T17:43:23Z
updated_at: 2025-12-22T17:52:15Z
url: https://github.com/astral-sh/uv/issues/17217
synced_at: 2026-01-10T01:26:15Z
---

# `uv venv`: flag to disable activate/pydoc script generation

---

_Issue opened by @paveldikov on 2025-12-22 17:43_

### Summary

I have a use case in which I want a venv that is not intended to be activatable, and these scripts just add noise/confusion/fragility (think Hyrum's law).

I can of course clean up these files post-hoc (and I do!), but it would be even nicer to stop these files from being created in the first place.

I think this might be worthwhile as an internal optimisation for the ephemeral/non-user-facing venvs that are generated various other areas of `uv` (or even stuff like `tox-uv`)

### Example

_No response_

---

_Label `enhancement` added by @paveldikov on 2025-12-22 17:43_

---

_Renamed from "uv venv: flag to disable activate/pydoc script generation" to "`uv venv`: flag to disable activate/pydoc script generation" by @paveldikov on 2025-12-22 17:43_

---

_Comment by @paveldikov on 2025-12-22 17:52_

P.S. `virtualenv` precedent: `--activators`; I can use `--activators ""` to achieve the same behaviour, though it's a bit clunky and fraught on Windows

---
