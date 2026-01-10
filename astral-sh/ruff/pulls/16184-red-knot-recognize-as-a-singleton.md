```yaml
number: 16184
title: "[red-knot] Recognize `...` as a singleton"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/ellipsistype-singleton
created_at: 2025-02-16T14:22:18Z
updated_at: 2025-02-16T22:01:03Z
url: https://github.com/astral-sh/ruff/pull/16184
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Recognize `...` as a singleton

---

_Pull request opened by @AlexWaygood on 2025-02-16 14:22_

## Summary

This PR adds some special-casing for `types.EllipsisType` (and, on Python <=3.9, the fictional `builtins.ellipsis` class) so that red-knot recognises it as a singleton class. Once this is recognised, we are able to narrow unions involving `EllipsisType` using `if x is ...`, which is a pattern supported by both [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=7cc5f1f77f16a34e4dabcb9fc74fb0c6) and [pyright](https://pyright-play.net/?code=GYJw9gtgBALgngBwKYGcoEsILCGUCiANoegiuigCqJIBQtAJksFMABQAeAXAcaeVRpQAPhgB2MAJRdaUORhYcMaAHRqZ8zVBBIAbkgCGhAPrxknSbPlJCKJBq1yd%2Bo6ZoXaQA).

I hoped to add support for `NotImplementedType` as part of the same PR, but this is requires a little more thought due to the fact that, according to typeshed, [`NotImplemented` is not an instance of `types.NotImplementedType` even on Python 3.10+](https://github.com/python/typeshed/blob/654d8c245766663379b69e89631d8ae665b63e48/stdlib/builtins.pyi#L1285-L1289).

## Test Plan

Mdtests added that fail on `main`


---

_Label `red-knot` added by @AlexWaygood on 2025-02-16 14:22_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-16 14:22_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-16 14:22_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-16 14:22_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/is.md`:85 on 2025-02-16 19:41_

Maybe

```suggestion
## `is` for `EllipsisType` (Python 3.9 and below)
```

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/is.md`:1 on 2025-02-16 19:44_

Could we also add tests for `EllipsisType` (and `ellipsis`) to `type_properties/is_singleton.md`? It has a similar version-split test for `NoDefault` as well.

I think I would be in favor of moving the version-dependent behavior there, while keeping this file free of this by making setting the version to 3.10 for the full file? But I'm also okay with having the `<=3.9 / >= 3.10` split in both files.

---

_@sharkdp approved on 2025-02-16 19:44_

Thank you

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/conditionals/is.md`:1 on 2025-02-16 21:47_

I'd weakly prefer to keep tests for both `<=3.9` and `>=3.10` here, since the important behaviour for users is the type narrowing behaviour rather than whether we internally consider `EllipsisType` to be a singleton class, and that's only tested here. But I will definitely add the tests to `is_singleton.md` too!

---

_@AlexWaygood reviewed on 2025-02-16 21:47_

---

_Merged by @AlexWaygood on 2025-02-16 22:01_

---

_Closed by @AlexWaygood on 2025-02-16 22:01_

---

_Branch deleted on 2025-02-16 22:01_

---
