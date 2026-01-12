```yaml
number: 13192
title: "[`ruff`] Implement post-init-default (`RUF033`)"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: post-init-defaults
created_at: 2024-09-01T11:34:55Z
updated_at: 2024-09-02T12:28:23Z
url: https://github.com/astral-sh/ruff/pull/13192
synced_at: 2026-01-12T15:55:43Z
```

# [`ruff`] Implement post-init-default (`RUF033`)

---

_@tjkuson_

## Summary

Implements the post-init-default (`RUF033`) rule, which reports a diagnostic upon any `__post_init__` method with argument default. Has a sometimes available unsafe autofix. For example,

```diff
 from dataclasses import InitVar, dataclass


 @dataclass
 class Foo:
     """A very helpful docstring."""

+    baz: InitVar[str] = "something"
     bar: InitVar[int] = 1

-    def __post_init__(self, bar: int, baz: str = "something") -> None: ...
+    def __post_init__(self, bar: int, baz: str) -> None: ...
```

Closes #13128

## Test Plan

`cargo nextest run`


---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:90 on 2024-09-01 11:40_

Before emitting the diagnostic, could we also check:
1. That we're inside a class scope (we shouldn't emit for `__post_init__` functions in the global scope)
2. That the class is decorated with `@dataclasses.dataclass` (since `__post_init__` has no special meaning for other classes)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:14 on 2024-09-01 11:40_

```suggestion
/// [documentation]:
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:20 on 2024-09-01 11:40_

```suggestion
/// Default values for `__post_init__` arguments that exist as init-only fields
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:49 on 2024-09-01 11:41_

We may as well keep the type annotations! Otherwise type checkers won't analyze the body of the function :-)

```suggestion
///     def __post_init__(self, bar: int, baz: int) -> None:
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:68 on 2024-09-01 11:44_

Even though there isn't actually a fix yet, I'd use `fix_title()` here for this information, as it keeps the violation message nice and concise, and clearly separates what the problem is from how to fix it.

```suggestion
    #[derive_message_formats]
    fn message(&self) -> String {
        format!("`__post_init__` method with argument defaults")
    }
    
    fn fix_title(&self) -> String {
        format!("Use `dataclasses.InitVar` instead"")
    }
```

---

_Comment by @github-actions[bot] on 2024-09-01 11:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-09-01 11:48_

Sorry for reviewing a draft PR... couldn't resist! Looks great.

---

_@tjkuson reviewed on 2024-09-01 18:09_

---

_Review comment by @tjkuson on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:90 on 2024-09-01 18:09_

Done in https://github.com/astral-sh/ruff/pull/13192/commits/6ca05468a85d7a9266d986205b2ccb57ae3f3301

---

_Comment by @codspeed-hq[bot] on 2024-09-01 18:15_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/tjkuson:post-init-defaults)

### Merging #13192 will **improve performances by 5.89%**

<sub>Comparing <code>tjkuson:post-init-defaults</code> (7b21ade) with <code>main</code> (0f85769)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `tjkuson:post-init-defaults` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 866.2 µs | 818 µs | +5.89% |


---

_Marked ready for review by @tjkuson on 2024-09-01 18:23_

---

_Renamed from "Implement post-init-default (`RUF033`)" to "[`ruff`] Implement post-init-default (`RUF033`)" by @tjkuson on 2024-09-01 18:28_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-09-02 08:45_

---

_@AlexWaygood approved on 2024-09-02 11:52_

Thanks! I pushed a couple of simplifications, but this was overall an excellent PR

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:85 on 2024-09-02 11:54_

I made this run on `StmtFunctionDef` nodes rather than `StmtClassDef` nodes, as this allows us to access `checker.semantic().current_scope()` to see what other symbols have been bound in the class. This enables us to see much more simply and more accurately whether it's safe to add a new `InitVar` field to the class body in the fix

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/post_init_default.rs`:185 on 2024-09-02 11:56_

Rather than iterate through all statements in the class body until we get the first non-docstring statement, I changed it so that we just insert the `InitVar` symbol in the line before the `__post_init__` definition. It seems much simpler and less error-prone as we already know that we're past the docstring in this case

---

_@AlexWaygood reviewed on 2024-09-02 11:56_

---

_Label `rule` added by @MichaReiser on 2024-09-02 11:58_

---

_Label `preview` added by @MichaReiser on 2024-09-02 11:58_

---

_Merged by @AlexWaygood on 2024-09-02 12:10_

---

_Closed by @AlexWaygood on 2024-09-02 12:10_

---

_Branch deleted on 2024-09-02 12:28_

---
