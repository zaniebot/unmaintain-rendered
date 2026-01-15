```yaml
number: 2507
title: "Bug: not deterministic reporting of `invalid-return-type`"
type: issue
state: open
author: jeertmans
labels: []
assignees: []
created_at: 2026-01-15T08:01:59Z
updated_at: 2026-01-15T16:36:57Z
url: https://github.com/astral-sh/ty/issues/2507
synced_at: 2026-01-15T16:50:04Z
```

# Bug: not deterministic reporting of `invalid-return-type`

---

_@jeertmans_

### Summary

Hello!

I am not sure how to report this, but it appears that the output of `ty check` is not deterministic. I discovered this when trying to ignore a specific `invalid-return-type` diagnostic, that I didn't know how to fix. I enabled the `unused-ignore-comment` rule, and noticed that the rule would sometimes be triggered, sometimes not.

```
➜  DiffeRT git:(main) ✗ uv run ty check
All checks passed!
➜  DiffeRT git:(main) ✗ uv run ty check
error[unused-ignore-comment]: Unused `ty: ignore` directive
   --> differt/src/differt/plotting/_utils.py:401:33
    |
399 |             registry[backend] = __wrapper__
400 |
401 |             return __wrapper__  # ty: ignore[invalid-return-type]
    |                                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
402 |
403 |         return _wrapper_
    |
help: Remove the unused suppression comment
398 | 
399 |             registry[backend] = __wrapper__
400 | 
    -             return __wrapper__  # ty: ignore[invalid-return-type]
401 +             return __wrapper__
402 | 
403 |         return _wrapper_
404 | 

Found 1 diagnostic
➜  DiffeRT git:(main) ✗ uv run ty check
error[unused-ignore-comment]: Unused `ty: ignore` directive
   --> differt/src/differt/plotting/_utils.py:401:33
    |
399 |             registry[backend] = __wrapper__
400 |
401 |             return __wrapper__  # ty: ignore[invalid-return-type]
    |                                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
402 |
403 |         return _wrapper_
    |
help: Remove the unused suppression comment
398 | 
399 |             registry[backend] = __wrapper__
400 | 
    -             return __wrapper__  # ty: ignore[invalid-return-type]
401 +             return __wrapper__
402 | 
403 |         return _wrapper_
404 | 

Found 1 diagnostic
➜  DiffeRT git:(main) ✗ uv run ty check
error[unused-ignore-comment]: Unused `ty: ignore` directive
   --> differt/src/differt/plotting/_utils.py:401:33
    |
399 |             registry[backend] = __wrapper__
400 |
401 |             return __wrapper__  # ty: ignore[invalid-return-type]
    |                                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
402 |
403 |         return _wrapper_
    |
help: Remove the unused suppression comment
398 | 
399 |             registry[backend] = __wrapper__
400 | 
    -             return __wrapper__  # ty: ignore[invalid-return-type]
401 +             return __wrapper__
402 | 
403 |         return _wrapper_
404 | 

Found 1 diagnostic
➜  DiffeRT git:(main) ✗ uv run ty check
All checks passed!
```

Removing the `# ty: ignore[invalid-return-type]` comment does the opposite:

```
➜  DiffeRT git:(main) ✗ uv run ty check
error[invalid-return-type]: Return type does not match returned value
   --> differt/src/differt/plotting/_utils.py:401:20
    |
399 |             registry[backend] = __wrapper__
400 |
401 |             return __wrapper__
    |                    ^^^^^^^^^^^ expected `(**P@dispatch) -> T@dispatch`, found `_Wrapped[P@dispatch, T@dispatch | SceneCanvas | matplotlib.figure.Figure | plotly.graph_objs._figure.Figure, P@dispatch, T@dispatch | SceneCanvas | matplotlib.figure.Figure | plotly.graph_objs._figure.Figure]`
402 |
403 |         return _wrapper_
    |
   ::: differt/src/differt/plotting/_utils.py:381:48
    |
379 |             )
380 |
381 |         def _wrapper_(impl: Callable[P, T]) -> Callable[P, T]:
    |                                                -------------- Expected `(**P@dispatch) -> T@dispatch` because of return type
382 |             # ruff: noqa: DOC201
383 |             """Actually register the backend implementation."""
    |
info: rule `invalid-return-type` is enabled by default

Found 1 diagnostic
➜  DiffeRT git:(main) ✗ uv run ty check
All checks passed!
➜  DiffeRT git:(main) ✗ uv run ty check
error[invalid-return-type]: Return type does not match returned value
   --> differt/src/differt/plotting/_utils.py:401:20
    |
399 |             registry[backend] = __wrapper__
400 |
401 |             return __wrapper__
    |                    ^^^^^^^^^^^ expected `(**P@dispatch) -> T@dispatch`, found `_Wrapped[P@dispatch, T@dispatch | SceneCanvas | matplotlib.figure.Figure | plotly.graph_objs._figure.Figure, P@dispatch, T@dispatch | SceneCanvas | matplotlib.figure.Figure | plotly.graph_objs._figure.Figure]`
402 |
403 |         return _wrapper_
    |
   ::: differt/src/differt/plotting/_utils.py:381:48
    |
379 |             )
380 |
381 |         def _wrapper_(impl: Callable[P, T]) -> Callable[P, T]:
    |                                                -------------- Expected `(**P@dispatch) -> T@dispatch` because of return type
382 |             # ruff: noqa: DOC201
383 |             """Actually register the backend implementation."""
    |
info: rule `invalid-return-type` is enabled by default

Found 1 diagnostic
```

Do you have any idea if this is a bug from ty?

# MWE

You can reproduce this bug by cloning my repository:

```bash
git clone https://github.com/jeertmans/DiffeRT.git --depth 1
```

You can avoid building the Rust code by downloading pre-built wheels:

```bash
sed -i 's/members = \["differt", "differt-core"\]/members = ["differt"]/g' pyproject.toml
sed -i 's/^differt_core/# differt_core/g' differt/pyproject.toml
```

### Version

ty 0.0.12

---

_Comment by @carljm on 2026-01-15 16:36_

Thanks for the report! Yes, we have some known non-determinism on some projects that we need to track down and fix; another example is helpful.

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-15 16:36_

---
