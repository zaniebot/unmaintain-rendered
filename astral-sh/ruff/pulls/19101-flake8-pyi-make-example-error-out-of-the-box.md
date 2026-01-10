```yaml
number: 19101
title: "[`flake8-pyi`] Make example error out-of-the-box (`PYI042`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-5
created_at: 2025-07-02T20:02:10Z
updated_at: 2025-07-02T21:36:27Z
url: https://github.com/astral-sh/ruff/pull/19101
synced_at: 2026-01-10T18:33:12Z
```

# [`flake8-pyi`] Make example error out-of-the-box (`PYI042`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-02 20:02_

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

This PR makes [snake-case-type-alias (PYI042)](https://docs.astral.sh/ruff/rules/snake-case-type-alias/#snake-case-type-alias-pyi042)'s example error out-of-the-box

[Old example](https://play.ruff.rs/8fafec81-2228-4ffe-81e8-1989b724cb47)
```py
type_alias_name: TypeAlias = int
```

[New example](https://play.ruff.rs/b396746c-e6d2-423c-bc13-01a533bb0747)
```py
from typing import TypeAlias

type_alias_name: TypeAlias = int
```

Imports were also added to the "use instead" section.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Review requested from @AlexWaygood by @MeGaGiGaGon on 2025-07-02 20:02_

---

_Comment by @github-actions[bot] on 2025-07-02 20:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2025-07-02 21:30_

---

_Label `documentation` added by @AlexWaygood on 2025-07-02 21:31_

---

_Merged by @AlexWaygood on 2025-07-02 21:31_

---

_Closed by @AlexWaygood on 2025-07-02 21:31_

---

_Branch deleted on 2025-07-02 21:36_

---
