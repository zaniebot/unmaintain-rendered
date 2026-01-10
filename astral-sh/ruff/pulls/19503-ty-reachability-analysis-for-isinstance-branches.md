```yaml
number: 19503
title: "[ty] Reachability analysis for `isinstance(…)` branches"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/isinstance-inference
created_at: 2025-07-23T08:52:02Z
updated_at: 2025-07-23T12:42:18Z
url: https://github.com/astral-sh/ruff/pull/19503
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Reachability analysis for `isinstance(…)` branches

---

_Pull request opened by @sharkdp on 2025-07-23 08:52_

## Summary

Add more precise type inference for a limited set of `isinstance(…)` calls, i.e. return `Literal[True]` if we can be sure that this is the correct result. This improves exhaustiveness checking / reachability analysis for if-elif-else chains with `isinstance` checks. For example:

```py
def is_number(x: int | str) -> bool:  # no "can implicitly return `None` error here anymore
    if isinstance(x, int):
        return True
    elif isinstance(x, str):
        return False

    # code here is now detected as being unreachable
```

This PR also adds a new test suite for exhaustiveness checking.

## Test Plan

New Markdown tests

### Ecosystem analysis

The removed diagnostics look good. There's [one case](https://github.com/pytorch/vision/blob/f52c4f1afd7dec25cbe7b98bcf1cbc840298e8da/torchvision/io/video_reader.py#L125-L143) where a "true positive" is removed in unreachable code. `src` is annotated as being of type `str`, but there is an `elif isinstance(src, bytes)` branch, which we now detect as unreachable. And so the diagnostic inside that branch is silenced. I don't think this is a problem, especially once we have a "graying out" feature, or a lint that warns about unreachable code.

---

_Label `ty` added by @sharkdp on 2025-07-23 08:52_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-23 08:52_

---

_@sharkdp reviewed on 2025-07-23 08:54_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/exhaustiveness_checking.md`:179 on 2025-07-23 08:54_

On `main`, we emit a diagnostic here, i.e. we do not detect this branch as unreachable. On this branch, we do detect this as unreachable.

---

_Comment by @github-actions[bot] on 2025-07-23 08:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
kornia (https://github.com/kornia/kornia)
- error[invalid-return-type] kornia/geometry/liegroup/so3.py:77:38: Function can implicitly return `None`, which is not assignable to return type `So3`
- error[invalid-return-type] kornia/geometry/liegroup/so3.py:98:24: Return type does not match returned value: expected `So3`, found `Vector3`
- Found 777 diagnostics
+ Found 775 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- error[invalid-return-type] github/Repository.py:3599:45: Function can implicitly return `None`, which is not assignable to return type `GitRelease`
- error[invalid-assignment] github/Team.py:305:13: Object of type `str` is not assignable to `Repository`
- Found 307 diagnostics
+ Found 305 diagnostics

apprise (https://github.com/caronc/apprise)
- error[unresolved-attribute] test/helpers/rest.py:291:26: Type `NotifyBase` has no attribute `url`
- error[unresolved-attribute] test/test_plugin_email.py:388:30: Type `NotifyBase` has no attribute `url`
- error[unresolved-attribute] test/test_plugin_growl.py:298:30: Type `NotifyBase` has no attribute `url`
- Found 4315 diagnostics
+ Found 4312 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- warning[possibly-unresolved-reference] pwndbg/dbg/gdb/__init__.py:817:13: Name `bp` used when possibly not defined
- warning[possibly-unresolved-reference] pwndbg/dbg/gdb/__init__.py:819:27: Name `bp` used when possibly not defined
- warning[possibly-unresolved-reference] pwndbg/dbg/gdb/__init__.py:833:9: Name `bp` used when possibly not defined
- warning[possibly-unresolved-reference] pwndbg/dbg/lldb/__init__.py:632:16: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] pwndbg/dbg/lldb/__init__.py:634:55: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] pwndbg/dbg/lldb/__init__.py:637:26: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] pwndbg/dbg/lldb/__init__.py:1551:16: Name `bp` used when possibly not defined
- warning[possibly-unresolved-reference] pwndbg/dbg/lldb/__init__.py:1553:60: Name `e` used when possibly not defined
- warning[possibly-unresolved-reference] pwndbg/dbg/lldb/__init__.py:1553:77: Name `e` used when possibly not defined
- warning[possibly-unresolved-reference] pwndbg/dbg/lldb/__init__.py:1576:28: Name `bp` used when possibly not defined
- warning[possibly-unresolved-reference] pwndbg/dbg/lldb/__init__.py:1595:27: Name `bp` used when possibly not defined
- warning[possibly-unresolved-reference] pwndbg/dbg/lldb/__init__.py:1596:88: Name `bp` used when possibly not defined
- warning[possibly-unresolved-reference] pwndbg/dbg/lldb/__init__.py:1597:29: Name `bp` used when possibly not defined
- warning[possibly-unresolved-reference] pwndbg/dbg/lldb/__init__.py:1598:88: Name `bp` used when possibly not defined
- Found 2260 diagnostics
+ Found 2246 diagnostics

vision (https://github.com/pytorch/vision)
- error[invalid-assignment] torchvision/io/video_reader.py:143:17: Object of type `BytesIO` is not assignable to `str`
- Found 1468 diagnostics
+ Found 1467 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- error[invalid-return-type] src/prefect/server/schemas/schedules.py:565:27: Function can implicitly return `None`, which is not assignable to return type `rrule`
- Found 3688 diagnostics
+ Found 3687 diagnostics

sympy (https://github.com/sympy/sympy)
- warning[possibly-unresolved-reference] sympy/solvers/ode/ode.py:1001:32: Name `deriv` used when possibly not defined
- warning[possibly-unresolved-reference] sympy/solvers/ode/ode.py:1001:66: Name `deriv` used when possibly not defined
- warning[possibly-unresolved-reference] sympy/solvers/ode/ode.py:1002:39: Name `deriv` used when possibly not defined
- warning[possibly-unresolved-reference] sympy/solvers/ode/ode.py:1003:25: Name `deriv` used when possibly not defined
- warning[possibly-unresolved-reference] sympy/solvers/ode/ode.py:1003:54: Name `old` used when possibly not defined
- warning[possibly-unresolved-reference] sympy/solvers/ode/ode.py:1004:21: Name `new` used when possibly not defined
- warning[possibly-unresolved-reference] sympy/solvers/ode/ode.py:1004:45: Name `deriv` used when possibly not defined
- warning[possibly-unresolved-reference] sympy/solvers/ode/ode.py:1005:21: Name `deriv` used when possibly not defined
- warning[possibly-unresolved-reference] sympy/solvers/ode/ode.py:1007:40: Name `deriv` used when possibly not defined
- warning[possibly-unresolved-reference] sympy/solvers/ode/ode.py:1009:44: Name `new` used when possibly not defined
- Found 12963 diagnostics
+ Found 12953 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@sharkdp reviewed on 2025-07-23 08:55_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/exhaustiveness_checking.md`:1 on 2025-07-23 08:55_

I added tests for `match` statements as well, but they don't work yet (see https://github.com/astral-sh/ty/issues/99#issuecomment-2983054488)

---

_Comment by @github-actions[bot] on 2025-07-23 09:00_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `possibly-unresolved-reference` | 0 | 24 | 0 |
| `invalid-return-type` | 0 | 4 | 0 |
| `unresolved-attribute` | 0 | 3 | 0 |
| `invalid-assignment` | 0 | 2 | 0 |
| **Total** | **0** | **33** | **0** |

**[Full report with detailed diff](https://david-isinstance-inference.ecosystem-663.pages.dev/diff)**


---

_Renamed from "[ty] Type inference for `isinstance(…)` calls" to "[ty] Reachability analysis for `isinstance(…)` branches" by @sharkdp on 2025-07-23 09:15_

---

_Marked ready for review by @sharkdp on 2025-07-23 09:19_

---

_Review requested from @carljm by @sharkdp on 2025-07-23 09:19_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-23 09:19_

---

_Review requested from @dcreager by @sharkdp on 2025-07-23 09:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1272 on 2025-07-23 09:28_

we could possibly also set the return type to `Literal[False]` if `ClassType::could_coexist_in_mro_with()` returns `false` for the two classes (indicating that it would be impossible to create a class that subclasses both classes simultaneously):

https://github.com/astral-sh/ruff/blob/b605c3e2327fbc7c16f7e26cdc1f53c47ce60344/crates/ty_python_semantic/src/types/class.rs#L492-L497

doesn't have to be done in this PR, though

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1321 on 2025-07-23 09:29_

it might make sense to apply similar handling for `issubclass()` too, just for symmetry if for nothing else. But again, that doesn't have to be done in this PR

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1268 on 2025-07-23 09:38_

there are a number of other variants where we _could_ plausibly return `true` here. Any inhabitant of a `Tuple` type is an instance of `tuple`; any inhabitant of a `ClassLiteral` type is an instance of `type`; any inhabitant of a `FunctionLiteral` type is an instance of `types.FunctionType`

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/exhaustiveness_checking.md`:159 on 2025-07-23 09:43_

```suggestion
            # this diagnostic is correct: the inferred type of `x` is `Literal[1]`:
            assert_never(x)  # error: [type-assertion-failure]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/exhaustiveness_checking.md`:42 on 2025-07-23 09:44_

this might help distinguish this from some of the undesirable diangostics in this file (having a deliberate `type-assertion-failure` diagnostic in an mdtest is a bit unusual!)

```suggestion
        # this diagnostic is correct: the inferred type of `x` is `Literal[1]`
        assert_never(x)  # error: [type-assertion-failure]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/exhaustiveness_checking.md`:200 on 2025-07-23 09:45_

```suggestion
        # this diagnostic is correct: inferred type of `x` is `C`:
        assert_never(x)  # error: [type-assertion-failure]
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/exhaustiveness_checking.md`:237 on 2025-07-23 09:45_

```suggestion
            # this diagnostic is correct: the inferred type of `x` is `B`:
            assert_never(x)  # error: [type-assertion-failure]
```

---

_@AlexWaygood approved on 2025-07-23 09:46_

Nice!

---

_@sharkdp reviewed on 2025-07-23 10:09_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/function.rs`:1272 on 2025-07-23 10:09_

Ah, interesting! I'm somehow assuming that negative answers will not yield a lot of benefit, but I might be wrong. Will evaluate this separately.

---

_@sharkdp reviewed on 2025-07-23 10:10_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/function.rs`:1321 on 2025-07-23 10:10_

I did sort of a 80:20 thing here. We could do many sophisticated things, but I'm not sure if it's worth it? Might be quick to try out though, will evaluate separately.

---

_@sharkdp reviewed on 2025-07-23 10:12_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/function.rs`:1268 on 2025-07-23 10:12_

Yes, I thought about it. It's a similar "bang for the buck" argument here, I guess. Can try to add a few more things, though.

---

_@AlexWaygood reviewed on 2025-07-23 10:18_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:1268 on 2025-07-23 10:18_

we already have the "this variant delegates to _this_ nominal-instance type" logic fully spelled out in `Type::has_relation_to()`. But we obviously can't easily reuse that delegation logic here, at least not without some (possibly painful) refactoring

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-23 10:39_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-23 10:39_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-23 10:41_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-23 10:41_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:946 on 2025-07-23 10:44_

I think we can optimize the same way for function-literals as you did for tuples?

```suggestion
        Type::Tuple(..) => always_true_if(class.is_known(db, KnownClass::Tuple)),

        Type::FunctionLiteral(..) => {
            always_true_if(class.is_known(db, KnownClass::FunctionType))
        }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:970 on 2025-07-23 10:46_

I _do_ think the fact that class-literals are all instances of `type` comes up fairly often, so I think that would be worth reflecting. (But the same is [not true](https://github.com/astral-sh/ty/issues/116) for generic aliases, and thus we also can't apply the same logic to `SubclassOf` types either.)

---

_@AlexWaygood reviewed on 2025-07-23 10:46_

---

_@sharkdp reviewed on 2025-07-23 10:51_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/function.rs`:946 on 2025-07-23 10:51_

I think the existing check for function literals is more general. It would also work for `isinstance(some_function, object)`. I had to rewrite the one for tuples because it didn't work the other way around (due to the representation of tuple instances, I think)

---

_@AlexWaygood reviewed on 2025-07-23 10:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:946 on 2025-07-23 10:53_

Aha, yet another reason for me to remove `Type::Tuple` entirely!

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-23 10:55_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-23 10:55_

---

_@sharkdp reviewed on 2025-07-23 10:59_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/function.rs`:970 on 2025-07-23 10:59_

Didn't yield any new ecosystem results, but doesn't hurt :smile: — thanks.

---

_Merged by @sharkdp on 2025-07-23 11:06_

---

_Closed by @sharkdp on 2025-07-23 11:06_

---

_Branch deleted on 2025-07-23 11:06_

---

_@AlexWaygood reviewed on 2025-07-23 12:22_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/function.rs`:970 on 2025-07-23 12:22_

For the record -- I experimented in https://github.com/astral-sh/ruff/pull/19507 with what it would look like to do a more exhaustive match here. No ecosystem hits at all, so I think you were right to choose a simpler route here :-)

---

_@sharkdp reviewed on 2025-07-23 12:42_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/function.rs`:970 on 2025-07-23 12:42_

Thanks!

---
