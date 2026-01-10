```yaml
number: 20128
title: "[ty] No boundness analysis for implicit instance attributes"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/no-reachability-for-implicit-attributes
created_at: 2025-08-28T09:08:41Z
updated_at: 2025-08-28T14:49:18Z
url: https://github.com/astral-sh/ruff/pull/20128
synced_at: 2026-01-10T17:46:21Z
```

# [ty] No boundness analysis for implicit instance attributes

---

_Pull request opened by @sharkdp on 2025-08-28 09:08_

## Summary

With this PR, we stop performing boundness analysis for implicit instance attributes:

```py
class C:
    def __init__(self):
        if False:   
            self.x = 1

C().x  # would previously show an error, with this PR we pretend the attribute exists
```

This PR is potentially just a temporary measure until we find a better fix. But I have already invested a lot of time trying to find the root cause of https://github.com/astral-sh/ty/issues/758 (and [this example](https://github.com/astral-sh/ty/issues/758#issuecomment-3206108262), which I'm not entirely sure is related) and I still don't understand what is going on. This PR fixes the performance problems in both of these problems (in a rather crude way).

The impact of the proposed change on the ecosystem is small, and the three new diagnostics are arguably true positives (previously hidden because we considered the code unreachable, based on e.g. `assert`ions that depended on implicit instance attributes). So this seems like a reasonable fix for now.

Note that we still support cases like these:

```py
class D:
    if False:  # or any other expression that statically evaluates to `False`
        x: int = 1

D().x  # still an error


class E:
    if False:  # or any other expression that statically evaluates to `False`
        def f(self):
            self.x = 1

E().x  # still an error
```

closes https://github.com/astral-sh/ty/issues/758

## Test Plan

Updated tests, benchmark results


---

_Label `ty` added by @sharkdp on 2025-08-28 09:08_

---

_Comment by @github-actions[bot] on 2025-08-28 09:10_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-28 09:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
+ src/trio/_tests/test_ssl.py:932:15: error[call-non-callable] Object of type `None` is not callable
- Found 675 diagnostics
+ Found 676 diagnostics

ignite (https://github.com/pytorch/ignite)
+ tests/ignite/metrics/test_precision.py:377:17: warning[possibly-unbound-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ tests/ignite/metrics/test_recall.py:380:17: warning[possibly-unbound-attribute] Attribute `cpu` on type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- Found 2107 diagnostics
+ Found 2109 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Renamed from "[ty] No reachability analysis for implicit instance attributes" to "[ty] No boundness analysis for implicit instance attributes" by @sharkdp on 2025-08-28 13:21_

---

_@sharkdp reviewed on 2025-08-28 13:28_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/reachability_constraints.rs`:823 on 2025-08-28 13:28_

I have added and removed this so many times... it's obviously useful, so I guess there's no harm in leaving it.

---

_Comment by @codspeed-hq[bot] on 2025-08-28 13:52_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fno-reachability-for-implicit-attributes?runnerMode=Instrumentation)

### Merging #20128 will **improve performances by ×3.9**

<sub>Comparing <code>david/no-reachability-for-implicit-attributes</code> (ceab895) with <code>main</code> (e586f6d)</sub>



### Summary

`⚡ 2` improvements  
`✅ 41` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` ty_micro[complex_constrained_attributes_2] `` | 96.3 ms | 63 ms | +52.86% |
| ⚡ | `` ty_micro[complex_constrained_attributes_3] `` | 261 ms | 66.8 ms | ×3.9 |


---

_Marked ready for review by @sharkdp on 2025-08-28 13:59_

---

_Review requested from @carljm by @sharkdp on 2025-08-28 13:59_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-08-28 13:59_

---

_Review requested from @dcreager by @sharkdp on 2025-08-28 13:59_

---

_@AlexWaygood approved on 2025-08-28 14:19_

Branch-dependent attributes are much more common in stub files than in runtime code. I wonder if it would make sense to add a diagnostic that _enforces_ that you should never have any implicit instance attributes in a stub file. I.e., since we will now have different behaviour for the two different snippets, we could add a diagnostic that says that in a stub file you should never do this:

```py
import sys

class F:
    def __init__(self) -> None:
        if sys.version_info >= (3, 13):
            self.x: int
```

and that instead you should always do this:

```py
import sys

class Foo:
    if sys.version_info >= (3, 13):
        x: int
```

(but the new diagnostic could probably trigger for any implicit instance attribute in a stub file, not just ones inside branches -- it'll be simpler to implement that way, and there's no real reason to ever have any implicit instance attributes in a stub file)

Then again, now that I think about it, that's all already covered by https://docs.astral.sh/ruff/rules/non-empty-stub-body/, so maybe we should just recommend that users should enable that Ruff rule on their stub files

---

_Comment by @sharkdp on 2025-08-28 14:24_

> Branch-dependent attributes are much more common in stub files than in runtime code. I wonder if it would make sense to add a diagnostic that _enforces_ that you should never have any implicit instance attributes in a stub file. I.e., since we will now have different behaviour for the two different snippets […]

I don't see any reason why someone would declare the type of instance attributes the complicated way *in a stub*, if there is a much easier way to just declare it on the body of the class. The implicit syntax wouldn't enable any use cases that wouldn't be covered by a simple `attribute: type` declaration on the body, I think? So I would argue that this inconsistency is nothing we need to immediately worry about, especially if there is a Ruff lint that would "catch" it.

---

_Merged by @sharkdp on 2025-08-28 14:25_

---

_Closed by @sharkdp on 2025-08-28 14:25_

---

_Branch deleted on 2025-08-28 14:25_

---

_Comment by @AlexWaygood on 2025-08-28 14:49_

> I don't see any reason why someone would declare the type of instance attributes the complicated way _in a stub_, if there is a much easier way to just declare it on the body of the class.

there's no reason, no! But, folks do strange things sometimes -- I'd wager money that there's _a_ stub file out there _somewhere_ that does something like that 😆

---
