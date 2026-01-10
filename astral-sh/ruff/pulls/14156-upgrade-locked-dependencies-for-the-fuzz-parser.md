```yaml
number: 14156
title: "Upgrade locked dependencies for the `fuzz-parser` script"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: bump-pycodegen
created_at: 2024-11-07T15:05:23Z
updated_at: 2024-11-07T15:10:18Z
url: https://github.com/astral-sh/ruff/pull/14156
synced_at: 2026-01-10T20:50:57Z
```

# Upgrade locked dependencies for the `fuzz-parser` script

---

_Pull request opened by @AlexWaygood on 2024-11-07 15:05_

## Summary

The new version of `pysource-codegen` is able to generate more variants of syntactically valid Python source files.

## Test Plan

I ran `python scripts/fuzz-parser/fuzz.py $(shuf -i 0-100000 -n 10000)` locally, and no new bugs were uncovered by the script.


---

_Label `ci` added by @AlexWaygood on 2024-11-07 15:05_

---

_@MichaReiser approved on 2024-11-07 15:07_

---

_Merged by @AlexWaygood on 2024-11-07 15:10_

---

_Closed by @AlexWaygood on 2024-11-07 15:10_

---

_Branch deleted on 2024-11-07 15:10_

---
