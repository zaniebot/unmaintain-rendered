```yaml
number: 22214
title: "[`flake8-unused-arguments`] Mark `**kwargs` in `TypeVar` as used (`ARG001`)"
type: pull_request
state: merged
author: ValdonVitija
labels:
  - bug
assignees: []
merged: true
base: main
head: typevar_does_not_recognize_kwargs_for_arg001
created_at: 2025-12-26T22:26:23Z
updated_at: 2025-12-31T16:07:56Z
url: https://github.com/astral-sh/ruff/pull/22214
synced_at: 2026-01-10T16:36:18Z
```

# [`flake8-unused-arguments`] Mark `**kwargs` in `TypeVar` as used (`ARG001`)

---

_Pull request opened by @ValdonVitija on 2025-12-26 22:26_

## Summary

Fixes false positive in ARG001 when `**kwargs` is passed to `typing.TypeVar`

Closes #22178

When `**kwargs` is used in a `typing.TypeVar` call, the checker was not recognizing it as a usage, leading to false positive "unused function argument" warnings.

### Root Cause

In the AST, keyword arguments are represented by the `Keyword` struct with an `arg` field of type `Option<Identifier>`:
- Named keywords like `bound=int` have `arg = Some("bound")`
- Dictionary unpacking like `**kwargs` has `arg = None`

The existing code only handled the `Some(id)` case, never visiting the expression when `arg` was `None`, so `**kwargs` was never marked as used.

### Changes

Added an `else` branch to handle `**kwargs` unpacking by calling `visit_non_type_definition(value)` when `arg` is `None`. This ensures the `kwargs` variable is properly visited and marked as used by the semantic model.

## Test Plan

Tested with the following code:

```python
import typing

def f(
    *args: object,
    default: object = None,
    **kwargs: object,
) -> None:
    typing.TypeVar(*args, **kwargs)
```

Before :

`ARG001 Unused function argument: kwargs
`

After : 

`All checks passed!`

Run the example with the following command(from the root of ruff and please change the path to the module that contains the code example):

`cargo run -p ruff -- check /path/to/file.py --isolated --select=ARG --no-cache`

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 14:27_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/1834c5bb757c215cf9eb8f98f994a5843618ae6b/src/scikit_build_core/_compat/typing.py#L36'>src/scikit_build_core/_compat/typing.py:36:32:</a> RUF100 [*] Unused `noqa` directive (unused: `ARG001`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/1834c5bb757c215cf9eb8f98f994a5843618ae6b/src/scikit_build_core/_compat/typing.py#L36'>src/scikit_build_core/_compat/typing.py:36:32:</a> RUF100 [*] Unused `noqa` directive (unused: `ARG001`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>





---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:1878 on 2025-12-29 15:04_

I'm not sure we need the first part of this comment, but I like the example.


```suggestion
                                // Ex: typing.TypeVar(**kwargs)
```

I just removed the `*args` to make it even more clear which part is relevant.

---

_@ntBre reviewed on 2025-12-29 15:07_

Thank you! This is exactly the fix I had in mind.

Could you add your manual test case from the PR summary as a new ARG001 case just to prevent future regressions?

You could add it at the bottom of this file:

https://github.com/astral-sh/ruff/blob/fbb5c8aa3c19891f2a6d78ccf335ae91c94f98ca/crates/ruff_linter/resources/test/fixtures/flake8_unused_arguments/ARG.py#L254-L258

---

_Label `bug` added by @ntBre on 2025-12-29 15:07_

---

_@ValdonVitija reviewed on 2025-12-29 20:59_

---

_Review comment by @ValdonVitija on `crates/ruff_linter/src/checkers/ast/mod.rs`:1878 on 2025-12-29 20:59_

@ntBre Added the required test case & removed the first part of the comment.

Thank you!

---

_@ntBre approved on 2025-12-31 16:05_

Thank you!

---

_Renamed from "Typevar does not recognize kwargs for arg001" to "[`flake8-unused-arguments`] Mark `**kwargs` in `TypeVar` as used (`ARG001`)" by @ntBre on 2025-12-31 16:07_

---

_Merged by @ntBre on 2025-12-31 16:07_

---

_Closed by @ntBre on 2025-12-31 16:07_

---
