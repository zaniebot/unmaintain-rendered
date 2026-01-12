```yaml
number: 17655
title: "[red-knot] Don't ignore hidden files by default"
type: pull_request
state: merged
author: thejchap
labels:
  - ty
assignees: []
merged: true
base: main
head: feature/red-knot-no-ignore-hidden
created_at: 2025-04-27T15:04:27Z
updated_at: 2025-04-28T06:21:12Z
url: https://github.com/astral-sh/ruff/pull/17655
synced_at: 2026-01-12T15:56:03Z
```

# [red-knot] Don't ignore hidden files by default

---

_@thejchap_

## Summary
#17626
Update directory walking configuration to not ignore hidden files by default. Matches Ruff's behavior

## Test Plan
Snapshot test

---

_Comment by @github-actions[bot] on 2025-04-27 15:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
vision (https://github.com/pytorch/vision)
+ error[lint:unresolved-import] /tmp/mypy_primer/projects/vision/.github/process_commit.py:13:8: Cannot resolve import `requests`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/vision/.github/process_commit.py:77:47: Argument to this function is incorrect: Expected `int`, found `int | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/vision/.github/process_commit.py:77:47: Argument to this function is incorrect: Expected `int`, found `int | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/vision/.github/process_commit.py:77:47: Argument to this function is incorrect: Expected `int`, found `int | None`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/vision/.github/scripts/run-clang-format.py:48:5: Object of type `TextIOWrapper` is not assignable to `int`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/vision/.github/scripts/run-clang-format.py:152:17: Attribute `readlines` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/vision/.github/scripts/run-clang-format.py:153:17: Attribute `readlines` on type `@Todo(specialized non-generic class) | None` is possibly unbound
- Found 2249 diagnostics
+ Found 2256 diagnostics

```
</details>


---

_Label `red-knot` added by @AlexWaygood on 2025-04-27 18:05_

---

_Marked ready for review by @thejchap on 2025-04-27 21:14_

---

_Review requested from @carljm by @thejchap on 2025-04-27 21:14_

---

_Review requested from @MichaReiser by @thejchap on 2025-04-27 21:14_

---

_Review requested from @AlexWaygood by @thejchap on 2025-04-27 21:14_

---

_Review requested from @sharkdp by @thejchap on 2025-04-27 21:14_

---

_Review requested from @dcreager by @thejchap on 2025-04-27 21:14_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-27 21:27_

---

_@MichaReiser approved on 2025-04-28 06:21_

Thanks!

---

_Merged by @MichaReiser on 2025-04-28 06:21_

---

_Closed by @MichaReiser on 2025-04-28 06:21_

---
