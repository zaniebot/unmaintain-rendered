```yaml
number: 19296
title: "[`pyupgrade`] Make example error out-of-the-box (`UP040`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-07-11T23:00:54Z
updated_at: 2025-07-12T17:44:57Z
url: https://github.com/astral-sh/ruff/pull/19296
synced_at: 2026-01-12T15:56:36Z
```

# [`pyupgrade`] Make example error out-of-the-box (`UP040`)

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

This PR makes [non-pep695-type-alias (UP040)](https://docs.astral.sh/ruff/rules/non-pep695-type-alias/#non-pep695-type-alias-up040)'s example error out-of-the-box.

[Old example](https://play.ruff.rs/6beca1be-45cd-4e5a-aafa-6a0584c10d64)
```py
ListOfInt: TypeAlias = list[int]
PositiveInt = TypeAliasType("PositiveInt", Annotated[int, Gt(0)])
```

[New example](https://play.ruff.rs/bbad34da-bf07-44e6-9f34-53337e8f57d4)
```py
from typing import Annotated, TypeAlias, TypeAliasType
from annotated_types import Gt

ListOfInt: TypeAlias = list[int]
PositiveInt = TypeAliasType("PositiveInt", Annotated[int, Gt(0)])
```

Imports were also added to the "Use instead" section.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Label `documentation` added by @AlexWaygood on 2025-07-11 23:04_

---

_Comment by @github-actions[bot] on 2025-07-11 23:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs`:43 on 2025-07-12 13:56_

Let's add the other missing imports too, even if they're not necessary to trigger UP040 out of the box:

```suggestion
/// from typing import TypeAlias, TypeAliasType, Annotated
/// from annotated_types import Gt
```
 
And could you also add the missing imports of `Annotated` and `Gt` to the `Use instead:` block below?

---

_@AlexWaygood reviewed on 2025-07-12 13:56_

Thanks!

---

_@MeGaGiGaGon reviewed on 2025-07-12 16:29_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_type_alias.rs`:43 on 2025-07-12 16:29_

Added

---

_@AlexWaygood approved on 2025-07-12 17:39_

---

_Merged by @AlexWaygood on 2025-07-12 17:39_

---

_Closed by @AlexWaygood on 2025-07-12 17:39_

---

_Branch deleted on 2025-07-12 17:44_

---
