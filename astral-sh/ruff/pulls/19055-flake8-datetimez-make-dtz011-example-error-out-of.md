```yaml
number: 19055
title: "[`flake8-datetimez`] Make `DTZ011` example error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-7
created_at: 2025-06-30T20:18:12Z
updated_at: 2025-06-30T20:57:30Z
url: https://github.com/astral-sh/ruff/pull/19055
synced_at: 2026-01-12T15:56:30Z
```

# [`flake8-datetimez`] Make `DTZ011` example error out-of-the-box

---

_@MeGaGiGaGon_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

This PR makes [call-date-today (DTZ011)](https://docs.astral.sh/ruff/rules/call-date-today/#call-date-today-dtz011)'s example error out-of-the-box

[Old example](https://play.ruff.rs/b42d6aef-7777-4b3b-9f96-19132000b765)
```py
import datetime

datetime.datetime.today()
```

[New example](https://play.ruff.rs/8577c3c1-cfa8-425b-b1e1-4c53b2a48375)
```py
import datetime

datetime.date.today()
```

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-06-30 20:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-06-30 20:53_

Thank you!

---

_Merged by @dylwil3 on 2025-06-30 20:54_

---

_Closed by @dylwil3 on 2025-06-30 20:54_

---

_Label `documentation` added by @dylwil3 on 2025-06-30 20:54_

---

_Branch deleted on 2025-06-30 20:57_

---
