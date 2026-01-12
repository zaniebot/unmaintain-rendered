```yaml
number: 19695
title: "[`refurb`] Make example error out-of-the-box (`FURB157`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-08-01T17:30:06Z
updated_at: 2025-08-01T18:10:09Z
url: https://github.com/astral-sh/ruff/pull/19695
synced_at: 2026-01-12T15:56:45Z
```

# [`refurb`] Make example error out-of-the-box (`FURB157`)

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

This PR makes [verbose-decimal-constructor (FURB157)](https://docs.astral.sh/ruff/rules/verbose-decimal-constructor/#verbose-decimal-constructor-furb157)'s example error out-of-the-box.

[Old example](https://play.ruff.rs/0930015c-ad45-4490-800e-66ed057bfe34)
```py
Decimal("0")
Decimal(float("Infinity"))
```

[New example](https://play.ruff.rs/516e5992-322f-4203-afe7-46d8cad53368)
```py
from decimal import Decimal

Decimal("0")
Decimal(float("Infinity"))
```

Imports were also added to the "Use Instead" section.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-08-01 17:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-08-01 17:55_

---

_Label `documentation` added by @dylwil3 on 2025-08-01 17:55_

---

_Merged by @dylwil3 on 2025-08-01 17:55_

---

_Closed by @dylwil3 on 2025-08-01 17:55_

---

_Branch deleted on 2025-08-01 18:10_

---
