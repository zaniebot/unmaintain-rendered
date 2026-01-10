```yaml
number: 22438
title: "[ty] Use `Pool` from `regex_automata` to reuse the `matches` allocations"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/include-match-pool
created_at: 2026-01-07T15:37:14Z
updated_at: 2026-01-07T16:22:36Z
url: https://github.com/astral-sh/ruff/pull/22438
synced_at: 2026-01-10T16:30:32Z
```

# [ty] Use `Pool` from `regex_automata` to reuse the `matches` allocations

---

_Pull request opened by @MichaReiser on 2026-01-07 15:37_

## Summary
We alreaedy do this for `exclude`, so it feels like we should do for `include` and it's easy enough.

## Test Plan

`cargo nextest`


---

_Review requested from @carljm by @MichaReiser on 2026-01-07 15:37_

---

_Label `internal` added by @MichaReiser on 2026-01-07 15:37_

---

_Review requested from @sharkdp by @MichaReiser on 2026-01-07 15:37_

---

_Label `ty` added by @MichaReiser on 2026-01-07 15:37_

---

_Review requested from @dcreager by @MichaReiser on 2026-01-07 15:37_

---

_Review requested from @Gankra by @MichaReiser on 2026-01-07 15:37_

---

_Comment by @astral-sh-bot[bot] on 2026-01-07 15:38_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2026-01-07 15:40_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | Bottom[Series[Any, Any]] | ... omitted 6 union elements, object_ | Self@iloc]`
+ static_frame/core/bus.py:675:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Bus[Any], object_]`, found `InterGetItemILocReduces[Bus[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_ | Self@iloc]`
- static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Any, TVDtype@Index]`
+ static_frame/core/index.py:580:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@loc, TVDtype@Index]`, found `InterGetItemLocReduces[Any | Bottom[Series[Any, Any]], TVDtype@Index]`
- static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Unknown | Bottom[Series[Any, Any]], Any]`
+ static_frame/core/node_selector.py:526:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemLocReduces[TVContainer_co@InterfaceSelectQuartet, Any]`, found `InterGetItemLocReduces[Bottom[Series[Any, Any]] | Unknown, Any]`
+ static_frame/core/yarn.py:418:16: error[invalid-return-type] Return type does not match returned value: expected `InterGetItemILocReduces[Yarn[Any], object_]`, found `InterGetItemILocReduces[Yarn[Any] | Bottom[Index[Any]] | TypeBlocks | ... omitted 6 union elements, object_]`
- Found 1837 diagnostics
+ Found 1838 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review request for @dcreager removed by @MichaReiser on 2026-01-07 15:44_

---

_Review request for @carljm removed by @MichaReiser on 2026-01-07 15:44_

---

_Review request for @Gankra removed by @MichaReiser on 2026-01-07 15:44_

---

_Review request for @sharkdp removed by @MichaReiser on 2026-01-07 15:44_

---

_Review requested from @BurntSushi by @MichaReiser on 2026-01-07 15:44_

---

_@BurntSushi approved on 2026-01-07 15:55_

---

_Merged by @MichaReiser on 2026-01-07 16:22_

---

_Closed by @MichaReiser on 2026-01-07 16:22_

---

_Branch deleted on 2026-01-07 16:22_

---
