```yaml
number: 17081
title: "[red-knot] Binary operator inference: generalize code for non-instances"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/binary-operators-non-instances
created_at: 2025-03-31T08:04:46Z
updated_at: 2025-03-31T11:01:26Z
url: https://github.com/astral-sh/ruff/pull/17081
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Binary operator inference: generalize code for non-instances

---

_@sharkdp_

## Summary

Generalize the rich-comparison fallback code for binary operator inference. This gets rid of one `todo_type!(…)` and implements the last remaining failing case from https://github.com/astral-sh/ruff/issues/14200.

closes https://github.com/astral-sh/ruff/issues/14200

## Test Plan

New Markdown tests.

---

_Label `red-knot` added by @sharkdp on 2025-03-31 08:04_

---

_Review requested from @carljm by @sharkdp on 2025-03-31 08:04_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-03-31 08:04_

---

_Review requested from @dcreager by @sharkdp on 2025-03-31 08:04_

---

_Comment by @github-actions[bot] on 2025-03-31 08:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
black (https://github.com/psf/black)
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/black/src/blackd/middlewares.py:15:39: Operator `in` is not supported for types `str` and `<bound method `headers` of `Request`>`, in comparing `Literal["Access-Control-Request-Method"]` with `<bound method `headers` of `Request`> | @Todo(instance attribute on class with dynamic base)`
- Found 211 diagnostics
+ Found 212 diagnostics

rich (https://github.com/Textualize/rich)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/control.py:198:27: Object of type `dict | @Todo[crates/red_knot_python_semantic/src/types/infer.rs:3626]` cannot be assigned to parameter 2 (`table`) of bound method `translate`; expected type `_TranslateTable`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/control.py:198:27: Object of type `dict | @Todo[crates/red_knot_python_semantic/src/types/infer.rs:3627]` cannot be assigned to parameter 2 (`table`) of bound method `translate`; expected type `_TranslateTable`

```
</details>


---

_Comment by @sharkdp on 2025-03-31 08:19_

> `mypy_primer` results

The new `black` error comes from a `"Access-Control-Request-Method" in request.headers` comparison between `Literal["…"]` and a `@reify`-decorated method in `aiohttp.web_request.Request`, which we do not understand yet. It's apparently [similar to `@property`](https://github.com/aio-libs/propcache/blob/79088e6c9c5c1b5eae2be83dfe0cfc2315f8cc8e/src/propcache/_helpers_c.pyx#L7-L12) (`reify = propcache.api.under_cached_property`).

Previously, we simply returned `@Todo(Binary comparisons between more types)` from a comparison between `Literal["…"]` and a bound method, so this seems like an improvement. The false positive might go away with #17017.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:5386 on 2025-03-31 10:59_

by calling `.ok()`, we're probably throwing away information here that might be useful in a subdiagnostic, but we can deal with that later (this PR obviously doesn't regress on that in any way)

---

_@AlexWaygood approved on 2025-03-31 11:00_

Very nice!

---

_Merged by @sharkdp on 2025-03-31 11:01_

---

_Closed by @sharkdp on 2025-03-31 11:01_

---

_Branch deleted on 2025-03-31 11:01_

---
