```yaml
number: 22775
title: "[ty] Improve completion rankings for raise-from/except contexts"
type: pull_request
state: open
author: RasmusNygren
labels: []
assignees: []
base: main
head: completions-exception-context
created_at: 2026-01-20T20:34:23Z
updated_at: 2026-01-20T20:41:42Z
url: https://github.com/astral-sh/ruff/pull/22775
synced_at: 2026-01-20T20:55:43Z
```

# [ty] Improve completion rankings for raise-from/except contexts

---

_@RasmusNygren_

Completions of the correct types are now favoured in `except <CURSOR>` and `raise <EXPR> from <CURSOR>` contexts

Fixes https://github.com/astral-sh/ty/issues/1779

This does regress the completions when no character is typed after the raise keyword `raise <CURSOR>` but I think that's a reasonable trade-off to greatly simplify the implementation with AST-traversal rather than extending the token-based approach.

## Test Plan
New completion evaluation tasks with correct rankings (that produces incorrect rankings on main)


---

_Comment by @astral-sh-bot[bot] on 2026-01-20 20:36_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-20 20:37_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`

core (https://github.com/home-assistant/core)
+ homeassistant/util/variance.py:47:12: error[invalid-return-type] Return type does not match returned value: expected `(**_P@ignore_variance) -> _R@ignore_variance`, found `_Wrapped[_P@ignore_variance, _R@ignore_variance | int | float | datetime, _P@ignore_variance, _R@ignore_variance | int | float | datetime]`
- Found 14464 diagnostics
+ Found 14465 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @RasmusNygren on 2026-01-20 20:41_

---

_Review requested from @carljm by @RasmusNygren on 2026-01-20 20:41_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2026-01-20 20:41_

---

_Review requested from @sharkdp by @RasmusNygren on 2026-01-20 20:41_

---

_Review requested from @dcreager by @RasmusNygren on 2026-01-20 20:41_

---

_Review requested from @MichaReiser by @RasmusNygren on 2026-01-20 20:41_

---

_Review requested from @Gankra by @RasmusNygren on 2026-01-20 20:41_

---
