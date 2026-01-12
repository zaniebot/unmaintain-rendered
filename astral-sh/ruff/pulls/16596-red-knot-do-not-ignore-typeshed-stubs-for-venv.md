```yaml
number: 16596
title: "[red-knot] Do not ignore typeshed stubs for 'venv' module"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/typeshed-venv-stubs
created_at: 2025-03-10T07:56:54Z
updated_at: 2025-03-10T08:07:50Z
url: https://github.com/astral-sh/ruff/pull/16596
synced_at: 2026-01-12T15:55:55Z
```

# [red-knot] Do not ignore typeshed stubs for 'venv' module

---

_@sharkdp_

## Summary

We currently fail to add the stubs for the [`venv` stdlib module](https://github.com/python/typeshed/blob/main/stdlib/venv/__init__.pyi) because there is a [`venv/` ignore pattern](https://github.com/astral-sh/ruff/blob/b6c7ba4f8ee53fa5831ed5f94fc3ebf441353800/.gitignore#L181) in the top-level `.gitignore` file.

Another solution would be to remove the top-level `.gitignore` pattern. I'm personally not a big fan of such tool-specific ignore patterns, and I am even less of a fan of broad patterns like "venv", ["dist", "build", "downloads", "lib" or "parts"](https://github.com/astral-sh/ruff/blob/b6c7ba4f8ee53fa5831ed5f94fc3ebf441353800/.gitignore#L63-L81). I have seen them cause annoying-to-debug problems way too often. But I wasn't sure if anyone relied on these.

## Test Plan

Ran the typeshed sync workflow manually once to see if the `venv/` folder is now correctly added.


---

_Label `red-knot` added by @sharkdp on 2025-03-10 07:56_

---

_Review requested from @carljm by @sharkdp on 2025-03-10 07:56_

---

_Review requested from @MichaReiser by @sharkdp on 2025-03-10 07:56_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-03-10 07:56_

---

_@AlexWaygood approved on 2025-03-10 08:00_

Great catch! Reminds me of https://github.com/python/typeshed/pull/8607 😃

---

_Merged by @sharkdp on 2025-03-10 08:07_

---

_Closed by @sharkdp on 2025-03-10 08:07_

---

_Branch deleted on 2025-03-10 08:07_

---
