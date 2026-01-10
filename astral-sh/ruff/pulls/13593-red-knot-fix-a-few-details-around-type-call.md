```yaml
number: 13593
title: "[red-knot] Fix a few details around `Type::call`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/redknot-calls
created_at: 2024-10-01T16:44:40Z
updated_at: 2024-10-01T16:58:16Z
url: https://github.com/astral-sh/ruff/pull/13593
synced_at: 2026-01-10T20:59:36Z
```

# [red-knot] Fix a few details around `Type::call`

---

_Pull request opened by @AlexWaygood on 2024-10-01 16:44_

## Summary

Most method calls of the form `x.y()` desugar to `y = x.y; y()`. However, this is not true for dunder methods when they are implicitly called. `iter(x)` does not desugar to `x.__iter__()`, it desugars to `type(x).__iter__(x)`. For several dunders, we're looking up the method on the type of the object (correct!) but then we're not passing `self` into the dunder when it's called (incorrect!).

This PR fixes that. It doesn't have any user-visible change right now (and it's not really possible to test right now) because we don't validate the types of arguments passed to functions yet. But it's good to get things right now; it will make things less confusing when somebody comes to implementing validation of argument types.

I also reworked the `CallOutcome::union` constructor method so that it accepts an `IntoIterator<Item=Type<'db>>` rather than a `Box<[Type<'db>]>`. This makes the signature slightly more flexible and ergonomic.

## Test Plan

`cargo test -p red_knot_python_semantic --lib`


---

_Label `red-knot` added by @AlexWaygood on 2024-10-01 16:44_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-01 16:44_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-01 16:44_

---

_@carljm approved on 2024-10-01 16:47_

Nice, thank you!

---

_Merged by @AlexWaygood on 2024-10-01 16:49_

---

_Closed by @AlexWaygood on 2024-10-01 16:49_

---

_Branch deleted on 2024-10-01 16:49_

---

_Comment by @github-actions[bot] on 2024-10-01 16:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/44aa63330b03bdacab731af2333ff9bf70855de3/stubs/tensorflow/tensorflow/saved_model/__init__.pyi#L114'>stubs/tensorflow/tensorflow/saved_model/__init__.pyi:114:1:</a> F821 Undefined name `PREDI`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F821 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---
