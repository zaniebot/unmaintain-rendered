```yaml
number: 19192
title: "[ty] Fix `ClassLiteral.into_callable` for dataclasses"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-into-callable-class-literal
created_at: 2025-07-07T23:48:25Z
updated_at: 2025-07-16T21:13:31Z
url: https://github.com/astral-sh/ruff/pull/19192
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Fix `ClassLiteral.into_callable` for dataclasses

---

_Pull request opened by @MatthewMckee4 on 2025-07-07 23:48_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Slightly refactor ClassLiteral.into_callable to also look for `__init__` functions of type Type::Callable to fix the issue with dataclasses, as the synthensized __init__ created by dataclass isn't covered as it isn't FunctionLiteral

Fixes https://github.com/astral-sh/ty/issues/760

## Test Plan

Add subtype test from the issue


---

_Review requested from @carljm by @MatthewMckee4 on 2025-07-07 23:48_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-07-07 23:48_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-07-07 23:48_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-07-07 23:48_

---

_Renamed from "Allow callable type for __init__ functions in into_callable for Class…" to "[ty] Fix `ClassLiteral.into_callable` for dataclasses" by @MatthewMckee4 on 2025-07-07 23:49_

---

_Comment by @github-actions[bot] on 2025-07-07 23:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
strawberry (https://github.com/strawberry-graphql/strawberry)
- error[invalid-assignment] strawberry/codegen/query_codegen.py:648:13: Object of type `<class 'GraphQLOptional'> | ((t) -> Unknown)` is not assignable to `((Unknown, /) -> Unknown) | None`
- error[invalid-assignment] strawberry/codegen/query_codegen.py:656:13: Object of type `<class 'GraphQLList'> | ((t) -> Unknown)` is not assignable to `((Unknown, /) -> Unknown) | None`
- error[invalid-parameter-default] strawberry/codegen/query_codegen.py:760:9: Default value of type `<class 'GraphQLObjectType'>` is not assignable to annotated parameter type `(str, /) -> GraphQLObjectType`
- Found 359 diagnostics
+ Found 356 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[invalid-argument-type] src/bokeh/embed/bundle.py:186:29: Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `<class 'URL'>`
- error[invalid-argument-type] src/bokeh/embed/bundle.py:189:30: Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `<class 'URL'>`
- Found 858 diagnostics
+ Found 856 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ty` added by @MichaReiser on 2025-07-08 07:12_

---

_@sharkdp approved on 2025-07-09 07:50_

Thank you very much!

Made a minor change to avoid one unnecessary clone.

---

_Merged by @sharkdp on 2025-07-09 08:04_

---

_Closed by @sharkdp on 2025-07-09 08:04_

---

_Branch deleted on 2025-07-16 21:13_

---
