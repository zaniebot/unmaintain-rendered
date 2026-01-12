```yaml
number: 19673
title: "[`refurb`] Make example error out-of-the-box (`FURB164`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-07-31T21:37:14Z
updated_at: 2025-08-01T17:01:14Z
url: https://github.com/astral-sh/ruff/pull/19673
synced_at: 2026-01-12T15:56:44Z
```

# [`refurb`] Make example error out-of-the-box (`FURB164`)

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

This PR makes [unnecessary-from-float (FURB164)](https://docs.astral.sh/ruff/rules/unnecessary-from-float/#unnecessary-from-float-furb164)'s example error out-of-the-box.

[Old example](https://play.ruff.rs/807ef72f-9671-408d-87ab-8b8bad65b33f)
```py
Decimal.from_float(4.2)
Decimal.from_float(float("inf"))
Fraction.from_float(4.2)
Fraction.from_decimal(Decimal("4.2"))
```

[New example](https://play.ruff.rs/303680d1-8a68-4b6c-a5fd-d79c56eb0f88)
```py
from decimal import Decimal
from fractions import Fraction

Decimal.from_float(4.2)
Decimal.from_float(float("inf"))
Fraction.from_float(4.2)
Fraction.from_decimal(Decimal("4.2"))
```

The "Use instead" section also had imports added, and one of the fixed examples was slightly wrong and needed modification.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-31 21:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-08-01 12:49_

Thank you!

---

_Label `documentation` added by @dylwil3 on 2025-08-01 12:49_

---

_Merged by @dylwil3 on 2025-08-01 12:49_

---

_Closed by @dylwil3 on 2025-08-01 12:49_

---

_Branch deleted on 2025-08-01 17:01_

---
