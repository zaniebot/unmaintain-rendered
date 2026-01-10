```yaml
number: 17351
title: "[red-knot] Specialize `str.startswith` for string literals"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/str-startswith
created_at: 2025-04-11T13:12:54Z
updated_at: 2025-04-11T18:30:38Z
url: https://github.com/astral-sh/ruff/pull/17351
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Specialize `str.startswith` for string literals

---

_Pull request opened by @sharkdp on 2025-04-11 13:12_

## Summary

Infer precise Boolean literal types for `str.startswith` calls where the instance and the prefix are both string literals. This allows us to understand `sys.platform.startswith(…)` branches.

## Test Plan

New Markdown tests

---

_Label `red-knot` added by @sharkdp on 2025-04-11 13:12_

---

_Comment by @github-actions[bot] on 2025-04-11 13:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyinstrument (https://github.com/joerick/pyinstrument)
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/appdirs.py:99:33: Name `_get_win_folder` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/appdirs.py:152:33: Name `_get_win_folder` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/appdirs.py:319:33: Name `_get_win_folder` used when possibly not defined
- error[lint:unresolved-import] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/appdirs.py:559:28: Module `ctypes` has no member `windll`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/appdirs.py:562:20: Cannot resolve import `com.sun.jna`
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/appdirs.py:99:33: Name `_get_win_folder` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/appdirs.py:152:33: Name `_get_win_folder` used when not defined
+ warning[lint:unresolved-reference] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/vendor/appdirs.py:319:33: Name `_get_win_folder` used when not defined
- Found 292 diagnostics
+ Found 290 diagnostics

```
</details>


---

_Comment by @sharkdp on 2025-04-11 13:24_

The pyinstrument changes show that we now understand the [`sys.platform.startswith('java')`](https://github.com/joerick/pyinstrument/blob/main/pyinstrument/vendor/appdirs.py#L46) branch in that file. We still report some diagnostics, because the reachable `else` branch [sets `system = sys.platform`](https://github.com/joerick/pyinstrument/blob/99fab9d9e3d13c7c913c1db5607f7bb1a3ead94b/pyinstrument/vendor/appdirs.py#L59) and then uses *that* symbol inside nested scopes for [further platform checks](https://github.com/joerick/pyinstrument/blob/99fab9d9e3d13c7c913c1db5607f7bb1a3ead94b/pyinstrument/vendor/appdirs.py#L95). We still emit errors in those branches, because the public type of `system` is `Literal["linux"] | Unknown`, so we don't infer those platform check branches as unreachable.

So I think those diagnostics are true positives according to our model, where `system` could be reassigned (not that we would understand that yet, but we would require that symbol to be qualified with `Final`).

Two false positives further down in the file are now gone, because they use `system` for platform checks inside the scope it was defined in.

---

_Marked ready for review by @sharkdp on 2025-04-11 13:24_

---

_Review requested from @carljm by @sharkdp on 2025-04-11 13:24_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-11 13:24_

---

_Review requested from @dcreager by @sharkdp on 2025-04-11 13:24_

---

_Comment by @AlexWaygood on 2025-04-11 13:40_

It might be confusing for users if we infer precise types for `str.startswith` but not `str.endswith` — should we do that method too?

Overall I'm okay with this, but note that this goes beyond what the spec requires of us when it comes to understanding `sys.platform` branches

---

_Converted to draft by @sharkdp on 2025-04-11 13:40_

---

_Marked ready for review by @sharkdp on 2025-04-11 13:53_

---

_Comment by @sharkdp on 2025-04-11 13:59_

> It might be confusing for users if we infer precise types for `str.startswith` but not `str.endswith` — should we do that method too?

It's going to be extremely rare that someone actually calls `x.startswith(y)` where both `x` and `y` are string literal types. I don't think that we have to worry about similarly precise `endswith` inference being a required feature for anyone.

> Overall I'm okay with this, but note that this goes beyond what the spec requires of us when it comes to understanding `sys.platform` branches



> Overall I'm okay with this, but note that this goes beyond what the spec requires of us when it comes to understanding `sys.platform` branches

Yes, but [the spec is vague](https://typing.python.org/en/latest/spec/directives.html#version-and-platform-checking). And there is [this issue](https://github.com/python/typing/issues/1732) which aims to improve the spec. It includes this comment:

> Maybe it would also make sense to support platform checks using startswith(), as that is officially recommended [in the Python docs](https://docs.python.org/3/library/sys.html#sys.platform), and is required to properly support some Unix variants, like FreeBSD, that include the version number in the platform string. See also https://github.com/python/typeshed/pull/11876.

And this comment:

> mypy has actually always supported sys.platform.startswith, since 2016 (in addition to == and !=)

And this:

> sys.platform.startswith probably makes sense to add since it helps FreeBSD (a Tier 3 platform), is already supported by mypy, and doesn't seem difficult to add to other type checkers.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:6191 on 2025-04-11 14:03_

Maybe add a comment saying why we special-case this method specifically?

---

_Renamed from "[red-knot] Type inference for str.startswith" to "[red-knot] Specialize `str.startswith` for string literals" by @sharkdp on 2025-04-11 14:04_

---

_@MichaReiser approved on 2025-04-11 14:24_

---

_Merged by @sharkdp on 2025-04-11 14:26_

---

_Closed by @sharkdp on 2025-04-11 14:26_

---

_Branch deleted on 2025-04-11 14:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:3135 on 2025-04-11 14:27_

does it cause false positives if you use `SupportsIndex` here? I thought this branch meant we already avoided false positives for nearly all uses of `SupportsIndex`:

https://github.com/astral-sh/ruff/blob/ffef71d1067aa6d185bc177b502ff9dc4c906260/crates/red_knot_python_semantic/src/types.rs#L1226-L1243

---

_@AlexWaygood approved on 2025-04-11 14:28_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3135 on 2025-04-11 18:30_

You're right. For some reason I didn't realized I could just do `KnownClass::SupportsIndex.to_instance(db)` => https://github.com/astral-sh/ruff/pull/17362

---

_@sharkdp reviewed on 2025-04-11 18:30_

---
