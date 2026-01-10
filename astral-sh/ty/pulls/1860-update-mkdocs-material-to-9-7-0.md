```yaml
number: 1860
title: Update mkdocs-material to 9.7.0
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - documentation
  - internal
assignees: []
merged: true
base: main
head: update-mkdocs-material
created_at: 2025-12-12T00:14:12Z
updated_at: 2025-12-12T05:03:55Z
url: https://github.com/astral-sh/ty/pull/1860
synced_at: 2026-01-10T02:34:11Z
```

# Update mkdocs-material to 9.7.0

---

_Pull request opened by @MatthewMckee4 on 2025-12-12 00:14_

<!--
Thank you for contributing to ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Mostly copied https://github.com/astral-sh/ruff/pull/21797.

## Test Plan

From contributing.md, ran:

```bash
uv pip compile docs/requirements.in -o docs/requirements.txt --universal -p 3.12
uvx --with-requirements docs/requirements.txt -- mkdocs serve -f mkdocs.yml
```


---

_Label `documentation` added by @AlexWaygood on 2025-12-12 00:20_

---

_Label `internal` added by @AlexWaygood on 2025-12-12 00:20_

---

_@dhruvmanila approved on 2025-12-12 05:03_

Thank you!

---

_Renamed from "Update mkdocs-material" to "Update mkdocs-material to 9.7.0" by @dhruvmanila on 2025-12-12 05:03_

---

_Merged by @dhruvmanila on 2025-12-12 05:03_

---

_Closed by @dhruvmanila on 2025-12-12 05:03_

---
