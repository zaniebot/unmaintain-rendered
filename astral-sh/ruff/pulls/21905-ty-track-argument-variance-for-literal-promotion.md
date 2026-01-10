```yaml
number: 21905
title: "[ty] Track argument variance for literal promotion without relying on `SpecializationBuilder` internals"
type: pull_request
state: closed
author: dcreager
labels:
  - internal
  - ty
  - ecosystem-analyzer
assignees: []
base: main
head: dcreager/param-variance
created_at: 2025-12-10T20:05:39Z
updated_at: 2025-12-15T20:56:50Z
url: https://github.com/astral-sh/ruff/pull/21905
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Track argument variance for literal promotion without relying on `SpecializationBuilder` internals

---

_Pull request opened by @dcreager on 2025-12-10 20:05_

When inferring a specialization for a generic function call, we might want to promote `Literal` types that appear in the inferred specialization. Whether we do so depends (among other things) on the variance of those typevars in the parameter types that each argument is matched to. That means we need to track the variance of each typevar as we process the arguments during inference.

Before, we were piggy-backing on how `SpecializationBuilder` walks through the formal and actual types. Its `infer_map` method takes in a callback that is invoked each time we record a new type mapping for a typevar. Before, this method was provided with the variance of that typevar, according to the formal/actual pair that we just checked.

We are in the process of changing the internals of `SpecializationBuilder` so that this approach is no longer tenable. This PR updates the call inference logic to calculate the typevar variance separately, without making any assumptions about how `SpecializationBuilder` does its work.

---

_Review requested from @carljm by @dcreager on 2025-12-10 20:05_

---

_Review requested from @AlexWaygood by @dcreager on 2025-12-10 20:05_

---

_Review requested from @sharkdp by @dcreager on 2025-12-10 20:05_

---

_Label `internal` added by @dcreager on 2025-12-10 20:05_

---

_Label `ty` added by @dcreager on 2025-12-10 20:05_

---

_Comment by @astral-sh-bot[bot] on 2025-12-10 20:07_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-10 20:09_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 491 diagnostics
+ Found 489 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 42 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/models/annotations/geometry.py:222:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[int]] | Property[int]`, found `<class 'Float'>`
+ src/bokeh/models/annotations/geometry.py:222:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[Literal[0]]] | Property[Literal[0]]`, found `<class 'Float'>`
- src/bokeh/models/annotations/geometry.py:229:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[int]] | Property[int]`, found `<class 'Float'>`
+ src/bokeh/models/annotations/geometry.py:229:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[Literal[0]]] | Property[Literal[0]]`, found `<class 'Float'>`
+ src/bokeh/models/plots.py:779:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[Literal[5]]] | Property[Literal[5]]`, found `<class 'Int'>`
+ src/bokeh/models/plots.py:801:30: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[Literal[2000]]] | Property[Literal[2000]]`, found `<class 'Int'>`
- src/bokeh/models/ranges.py:403:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[int]] | Property[int]`, found `<class 'Float'>`
+ src/bokeh/models/ranges.py:403:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[Literal[0]]] | Property[Literal[0]]`, found `<class 'Float'>`
- src/bokeh/models/ranges.py:413:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[int]] | Property[int]`, found `<class 'Float'>`
+ src/bokeh/models/ranges.py:413:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[Literal[0]]] | Property[Literal[0]]`, found `<class 'Float'>`
+ src/bokeh/models/tools.py:685:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[Literal[0]]] | Property[Literal[0]]`, found `<class 'Int'>`
+ src/bokeh/models/tools.py:1182:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[Literal[0]]] | Property[Literal[0]]`, found `<class 'Int'>`
+ src/bokeh/models/widgets/inputs.py:441:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[Literal[2]]] | Property[Literal[2]]`, found `<class 'Int'>`
+ src/bokeh/models/widgets/inputs.py:592:32: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[Literal[100]]] | Property[Literal[100]]`, found `<class 'Int'>`
+ src/bokeh/models/widgets/inputs.py:601:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[Literal[1]]] | Property[Literal[1]]`, found `<class 'Int'>`
+ src/bokeh/models/widgets/sliders.py:102:22: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[Literal[""]]] | Property[Literal[""]]`, found `<class 'String'>`
+ src/bokeh/models/widgets/tables.py:848:31: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `type[Property[Literal[0]]] | Property[Literal[0]]`, found `<class 'Int'>`
- src/bokeh/plotting/_figure.py:201:97: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `str | BaseText | None`, found `Unknown | Nullable[str]`
+ src/bokeh/plotting/_figure.py:201:97: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `str | BaseText | None`, found `Unknown | Nullable[Literal[""]]`
- src/bokeh/plotting/_figure.py:202:97: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `str | BaseText | None`, found `Unknown | Nullable[str]`
+ src/bokeh/plotting/_figure.py:202:97: error[invalid-argument-type] Argument to function `process_axis_and_grid` is incorrect: Expected `str | BaseText | None`, found `Unknown | Nullable[Literal[""]]`
- src/bokeh/settings.py:747:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((int | None | str, /) -> int | None | str) | None`, found `def convert_logging(value: str | int) -> int | None`
+ src/bokeh/settings.py:747:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `((int | None | str, /) -> int | None | Literal["none", "debug"]) | None`, found `def convert_logging(value: str | int) -> int | None`
- Found 867 diagnostics
+ Found 876 diagnostics

zulip (https://github.com/zulip/zulip)
+ zerver/tests/test_users.py:259:25: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["email"], Unknown].__getitem__(key: Literal["email"], /) -> Unknown` cannot be called with key of type `Literal["is_owner"]` on object of type `dict[Literal["email"], Unknown]`
+ zerver/tests/test_users.py:261:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["email"], Unknown].__getitem__(key: Literal["email"], /) -> Unknown` cannot be called with key of type `Literal["is_owner"]` on object of type `dict[Literal["email"], Unknown]`
+ zerver/tests/test_users.py:318:25: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["email"], Unknown].__getitem__(key: Literal["email"], /) -> Unknown` cannot be called with key of type `Literal["is_admin"]` on object of type `dict[Literal["email"], Unknown]`
+ zerver/tests/test_users.py:320:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["email"], Unknown].__getitem__(key: Literal["email"], /) -> Unknown` cannot be called with key of type `Literal["is_admin"]` on object of type `dict[Literal["email"], Unknown]`
+ zerver/tests/test_users.py:361:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["email"]` on object of type `dict[Literal["user_id"], Unknown]`
+ zerver/tests/test_users.py:362:27: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["avatar_url"]` on object of type `dict[Literal["user_id"], Unknown]`
+ zerver/tests/test_users.py:363:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["delivery_email"]` on object of type `dict[Literal["user_id"], Unknown]`
+ zerver/tests/test_users.py:387:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["email"]` on object of type `dict[Literal["user_id"], Unknown]`
+ zerver/tests/test_users.py:389:13: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["avatar_url"]` on object of type `dict[Literal["user_id"], Unknown]`
+ zerver/tests/test_users.py:402:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["email"]` on object of type `dict[Literal["user_id"], Unknown]`
+ zerver/tests/test_users.py:404:13: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["avatar_url"]` on object of type `dict[Literal["user_id"], Unknown]`
+ zerver/tests/test_users.py:406:26: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[Literal["user_id"], Unknown].__getitem__(key: Literal["user_id"], /) -> Unknown` cannot be called with key of type `Literal["delivery_email"]` on object of type `dict[Literal["user_id"], Unknown]`
- Found 3631 diagnostics
+ Found 3643 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @carljm on 2025-12-10 20:23_

---

_Comment by @carljm on 2025-12-10 20:23_

Code looks fine. Ecosystem changes on zulip suggest that there's an unintentional behavior change here?

---

_Comment by @astral-sh-bot[bot] on 2025-12-10 20:32_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 21 | 0 | 7 |
| `unused-ignore-comment` | 0 | 3 | 0 |
| **Total** | **21** | **3** | **7** |

**[Full report with detailed diff](https://dcreager-param-variance.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-param-variance.ecosystem-663.pages.dev/timing))




---

_Comment by @carljm on 2025-12-10 20:58_

It seems like in the zulip cases we are failing to do literal promotion in a case where previously we did promote.

---

_Comment by @dcreager on 2025-12-11 16:32_

> It seems like in the zulip cases we are failing to do literal promotion in a case where previously we did promote.

Fixed the zulip case and added an mdtest regression for it.

---

_Label `ecosystem-analyzer` removed by @dcreager on 2025-12-11 16:39_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-12-11 16:39_

---

_Comment by @codspeed-hq[bot] on 2025-12-11 16:44_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fparam-variance?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21905 will **degrade performances by 4.25%**

<sub>Comparing <code>dcreager/param-variance</code> (fe25ee2) with <code>main</code> (c8851ec)</sub>



### Summary

`❌ 1` regression  
`✅ 12` untouched  
`⏩ 39` skipped[^skipped]  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fparam-variance?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fparam-variance?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.3 s | 1.4 s | -4.25% |
[^skipped]: 39 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fparam-variance?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @AlexWaygood on 2025-12-11 18:05_

Looks like both the ecosystem CI jobs timed out, which I think is probably _not_ related to the github outage?

---

_Comment by @dcreager on 2025-12-11 18:08_

> Looks like both the ecosystem CI jobs timed out, which I think is probably _not_ related to the github outage?

Yep, I've got a mypy_primer run going locally to confirm

---

_Comment by @dcreager on 2025-12-11 20:26_

> Looks like both the ecosystem CI jobs timed out

Cherry-picked several of performance optimizations commits from #21551 and that seems to have fixed the timeouts. Locally, at least! Waiting for CI to confirm

---

_Comment by @dcreager on 2025-12-11 21:12_

> Locally, at least! Waiting for CI to confirm

Welp, famous last words! Instead of pushing this forward more, I'm going to see if I can update the generic protocol PR to not require this.

---

_Comment by @dcreager on 2025-12-15 20:56_

> I'm going to see if I can update the generic protocol PR to not require this.

This happened, so I'm closing this for now

---

_Closed by @dcreager on 2025-12-15 20:56_

---

_Branch deleted on 2025-12-15 20:56_

---
