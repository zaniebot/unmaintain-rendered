```yaml
number: 20199
title: "[ty] chore: `ContextManagerError` message update"
type: pull_request
state: open
author: Y3K
labels:
  - ty
assignees: []
draft: true
base: main
head: chore/ContextManageError_message_update
created_at: 2025-09-01T19:28:22Z
updated_at: 2025-09-03T17:34:15Z
url: https://github.com/astral-sh/ruff/pull/20199
synced_at: 2026-01-10T17:46:21Z
```

# [ty] chore: `ContextManagerError` message update

---

_Pull request opened by @Y3K on 2025-09-01 19:28_

## Summary

Fixes https://github.com/astral-sh/ty/issues/940

## Test Plan

```python
class Bound:
    def __enter__(self): ...

    def __exit__(self, *args): ...


class EnterUnbound:
    def __exit__(self): ...


class ExitUnbound:
    def __enter__(self): ...


class Unbound: ...


def bound(x: Bound):
    with x: ...


def enter_unbound(x: EnterUnbound):
    with x: ...


def exit_unbound(x: ExitUnbound):
    with x: ...


def unbound(x: Unbound):
    with x: ...


def union_enter_unbound(x: Bound | EnterUnbound):
    with x: ...


def union_exit_unbound(x: Bound | ExitUnbound):
    with x: ...


def union_unbound(x: Bound | Unbound):
    with x: ...


def multiple(x: Bound | EnterUnbound | ExitUnbound):
    with x: ...
```

```bash
$ cargo run --bin ty -- check --project ../pydummy

error[invalid-context-manager]: Object of type `EnterUnbound` cannot be used with `with` because it does not implement `__enter__`, and it does not correctly implement `__exit__`
  --> ../pydummy/main.py:23:10
   |
22 | def enter_unbound(x: EnterUnbound):
23 |     with x: ...
   |          ^
   |
info: rule `invalid-context-manager` is enabled by default

error[invalid-context-manager]: Object of type `ExitUnbound` cannot be used with `with` because it does not implement `__exit__`
  --> ../pydummy/main.py:27:10
   |
26 | def exit_unbound(x: ExitUnbound):
27 |     with x: ...
   |          ^
   |
info: rule `invalid-context-manager` is enabled by default

error[invalid-context-manager]: Object of type `Unbound` cannot be used with `with` because it does not implement `__enter__` and `__exit__`
  --> ../pydummy/main.py:31:10
   |
30 | def unbound(x: Unbound):
31 |     with x: ...
   |          ^
   |
info: rule `invalid-context-manager` is enabled by default

error[invalid-context-manager]: Object of type `Bound | EnterUnbound` cannot be used with `with` because the method `__enter__` of `EnterUnbound` is possibly unbound
  --> ../pydummy/main.py:35:10
   |
34 | def union_enter_unbound(x: Bound | EnterUnbound):
35 |     with x: ...
   |          ^
   |
info: rule `invalid-context-manager` is enabled by default

error[invalid-context-manager]: Object of type `Bound | ExitUnbound` cannot be used with `with` because the method `__exit__` of `ExitUnbound` is possibly unbound
  --> ../pydummy/main.py:39:10
   |
38 | def union_exit_unbound(x: Bound | ExitUnbound):
39 |     with x: ...
   |          ^
   |
info: rule `invalid-context-manager` is enabled by default

error[invalid-context-manager]: Object of type `Bound | Unbound` cannot be used with `with` because the methods `__enter__` and `__exit__` of `Unbound` are possibly unbound
  --> ../pydummy/main.py:43:10
   |
42 | def union_unbound(x: Bound | Unbound):
43 |     with x: ...
   |          ^
   |
info: rule `invalid-context-manager` is enabled by default

error[invalid-context-manager]: Object of type `Bound | EnterUnbound | ExitUnbound` cannot be used with `with` because the method `__enter__` of `EnterUnbound` is possibly unbound, and the method `__exit__` of `ExitUnbound` is possibly unbound
  --> ../pydummy/main.py:47:10
   |
46 | def multiple(x: Bound | EnterUnbound | ExitUnbound):
47 |     with x: ...
   |          ^
   |
info: rule `invalid-context-manager` is enabled by default
```

---

_Review requested from @carljm by @Y3K on 2025-09-01 19:28_

---

_Review requested from @AlexWaygood by @Y3K on 2025-09-01 19:28_

---

_Review requested from @sharkdp by @Y3K on 2025-09-01 19:28_

---

_Review requested from @dcreager by @Y3K on 2025-09-01 19:28_

---

_Comment by @github-actions[bot] on 2025-09-01 19:41_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-01 19:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/artifacts.py:675:16: error[invalid-context-manager] Object of type `nullcontext[PrefectClient & ~AlwaysFalsy] | PrefectClient` cannot be used with `async with` because the methods `__aenter__` and `__aexit__` are possibly unbound
+ src/prefect/artifacts.py:675:16: error[invalid-context-manager] Object of type `nullcontext[PrefectClient & ~AlwaysFalsy] | PrefectClient` cannot be used with `async with` because the methods `__aenter__` and `__aexit__` of `nullcontext[PrefectClient & ~AlwaysFalsy]` are possibly unbound

xarray (https://github.com/pydata/xarray)
- xarray/tests/test_distributed.py:316:14: error[invalid-context-manager] Object of type `Unknown | None` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly unbound
+ xarray/tests/test_distributed.py:316:14: error[invalid-context-manager] Object of type `Unknown | None` cannot be used with `with` because the methods `__enter__` and `__exit__` of `None` are possibly unbound

pandas (https://github.com/pandas-dev/pandas)
- pandas/tests/io/test_stata.py:1302:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly unbound
+ pandas/tests/io/test_stata.py:1302:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` of `DataFrame` are possibly unbound
- pandas/tests/io/test_stata.py:1347:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly unbound
+ pandas/tests/io/test_stata.py:1347:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` of `DataFrame` are possibly unbound
- pandas/tests/io/test_stata.py:1351:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly unbound
+ pandas/tests/io/test_stata.py:1351:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` of `DataFrame` are possibly unbound
- pandas/tests/io/test_stata.py:1355:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly unbound
+ pandas/tests/io/test_stata.py:1355:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` of `DataFrame` are possibly unbound
- pandas/tests/io/test_stata.py:1359:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly unbound
+ pandas/tests/io/test_stata.py:1359:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` of `DataFrame` are possibly unbound
- pandas/tests/io/test_stata.py:1364:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly unbound
+ pandas/tests/io/test_stata.py:1364:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` of `DataFrame` are possibly unbound
- pandas/tests/io/test_stata.py:1400:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly unbound
+ pandas/tests/io/test_stata.py:1400:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` of `DataFrame` are possibly unbound
- pandas/tests/io/test_stata.py:1427:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly unbound
+ pandas/tests/io/test_stata.py:1427:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` of `DataFrame` are possibly unbound
- pandas/tests/io/test_stata.py:1692:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly unbound
+ pandas/tests/io/test_stata.py:1692:14: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` of `DataFrame` are possibly unbound
- pandas/tests/io/test_stata.py:2351:10: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly unbound
+ pandas/tests/io/test_stata.py:2351:10: error[invalid-context-manager] Object of type `DataFrame | StataReader` cannot be used with `with` because the methods `__enter__` and `__exit__` of `DataFrame` are possibly unbound

```
</details>
No memory usage changes detected ✅


---

_Renamed from "[ty] chore: ContextManagerError message update" to "[ty] chore: `ContextManagerError` message update" by @Y3K on 2025-09-01 21:30_

---

_Label `ty` added by @ntBre on 2025-09-02 02:48_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7920 on 2025-09-02 23:39_

It seems we never use the second element of this return type, and I'm not sure why we ever would use it. Maybe just remove it?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7903 on 2025-09-02 23:42_

Maybe have this method take a `UnionType` (the struct wrapped by `Type::Union`) instead of a `Type`, since it's only useful for union types, and only called for union types? Then we wouldn't need the "empty return" case below. Every call site below is already guarded by `if let Type::Union(_) = context_expression_type {` which could just as easily be `if let Type::Union(union) = context_expression_type {`.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7916 on 2025-09-02 23:44_

We could distinguish bound vs possibly-unbound here, because we shouldn't give the wishy-washy "possibly unbound" below if we know it's definitely not bound on a type.

We should only report "possibly unbound" on a type like this:

```py
class PossiblyUnbound:
    if random.randint(0, 1):
        def __enter__(self): pass
```

But this kind of type should be uncommon, so I also think it's fine to only give the extra details for union elements where it's definitely unbound.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7939 on 2025-09-02 23:45_

At this point we've verified that for these specific elements of the union, the method is definitely not bound.

```suggestion
                                "the method `{name}` of `{}` {} unbound",
```

(If we did also record the union elements where the method is possibly-unbound, then we'd need to report both cases here, but we should still be clear about which union element is which.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7978 on 2025-09-02 23:48_

```suggestion
                                "the methods `{name_a}` and `{name_b}` of `{}` are unbound",
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7967 on 2025-09-02 23:52_

I think in this case we'd want to report on any union element that is missing _either_ method. We could potentially reach this case when there is no union element missing both methods.

One thing that could simplify the code here (and in other cases in this PR) is that we don't have to squeeze all the information into a single sentence for a single diagnostic message. It would require a bit more restructuring in this `report_diagnostic` method, but we can report multiple additional info sub-diagnostics. So e.g. each union element that is missing a method could just be its own `info` sub-diagnostic, and then we wouldn't have to worry so much about joining with " and ", is vs are, etc.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8005 on 2025-09-02 23:52_

```suggestion
                                "the method `{name_a}` of `{}` {} unbound",
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8021 on 2025-09-02 23:52_

```suggestion
                                "the method `{name_b}` of `{}` {} unbound",
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/with/async.md`:88 on 2025-09-02 23:54_

We can snapshot diagnostics for these tests, which is an easier way to see the full resulting diagnostic, instead of putting full error messages directly in the mdtest. This is usually preferred for tests focused on diagnostic rendering, and is especially useful when the diagnostic is not all crammed into a single sentence, as I suggest below.
```suggestion

<!-- snapshot-diagnostics -->

```

Then you use `cargo insta test -p ty_python_semantic` to generate new snapshots to review with `cargo insta review` and then commit.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/with/async.md`:104 on 2025-09-02 23:55_

E.g. if we snapshot diagnostics, then we can just do this here:
```suggestion
    # error: [invalid-context-manager]
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7907 on 2025-09-03 00:04_

I don't think this PR really addresses most of this TODO. We still don't surface the full underlying call error for wrongly-implemented dunders, like we do for e.g. `IterationError::report_diagnostic`. We don't need to do all of that in this PR, but I think we should leave this TODO in place.

---

_@carljm requested changes on 2025-09-03 00:06_

Thanks for the pull request! This is definitely an improvement.

My idea when I filed the issue was that we would bubble up the information about which types had the dunder unbound or possibly-unbound as part of the `CallDunderError`, but looking at it now I see this would probably require special-casing `Type::Union` in `try_call_dunder` (because `member_lookup_with_policy` squashes union information into a single `PlaceAndQualifiers`). I still think this might make sense, but I also think it's OK to just re-create the information once we know there's a problem.

---

_Review request for @sharkdp removed by @sharkdp on 2025-09-03 07:22_

---

_Comment by @Y3K on 2025-09-03 14:44_

Thank you for your review @carljm!

I'm quite rookie with Rust and `ty` therefore the changes requested still are a bit confussing to me.

I'll read each suggestion carefully and work on a new approach, will ping you once it's ready.

Thanks!

---

_Comment by @carljm on 2025-09-03 14:56_

@Y3K happy to offer clarification on any comments. I think for purposes of this PR you can ignore the comment about "bubbling up the information through `CallDunderError`", and you can ignore one other comment I'll mention inline. The other ones I think don't require an entirely "new approach" to address, just some updates to the current approach.

---

_@carljm reviewed on 2025-09-03 14:57_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:7916 on 2025-09-03 14:57_

I think for this PR you can ignore this comment; it could be a later improvement if it turns out to be useful.

---

_Comment by @Y3K on 2025-09-03 15:04_

Thank you again, @carljm !

I don't want to waste a lot of your time, but I do accept any extra help you want and can provide.

Will address all your feedback within this PR 🙌🏼 

---

_@Y3K reviewed on 2025-09-03 15:13_

---

_Review comment by @Y3K on `crates/ty_python_semantic/src/types.rs`:7920 on 2025-09-03 15:13_

Updated the function and it's usage (not pushed yet).

---

_@Y3K reviewed on 2025-09-03 15:50_

---

_Review comment by @Y3K on `crates/ty_python_semantic/src/types.rs`:7903 on 2025-09-03 15:50_

It is so clear now that you mention it! Sorry for such a dumb mistake.

I've updated the code (not pushed yet).

---

_@Y3K reviewed on 2025-09-03 15:51_

---

_Review comment by @Y3K on `crates/ty_python_semantic/src/types.rs`:7916 on 2025-09-03 15:51_

Marking as `Resolved` for now 🙆🏼‍♂️ 

---

_@Y3K reviewed on 2025-09-03 15:53_

---

_Review comment by @Y3K on `crates/ty_python_semantic/src/types.rs`:7939 on 2025-09-03 15:53_

Updated 👌🏼 (not pushed yet)

---

_Review comment by @Y3K on `crates/ty_python_semantic/src/types.rs`:7978 on 2025-09-03 15:54_

♻️ ditto

---

_@Y3K reviewed on 2025-09-03 15:54_

---

_@Y3K reviewed on 2025-09-03 16:04_

---

_Review comment by @Y3K on `crates/ty_python_semantic/src/types.rs`:8005 on 2025-09-03 16:04_

♻️ ditto

---

_@Y3K reviewed on 2025-09-03 16:05_

---

_Review comment by @Y3K on `crates/ty_python_semantic/src/types.rs`:8021 on 2025-09-03 16:05_

♻️ ditto

---

_@Y3K reviewed on 2025-09-03 16:40_

---

_Review comment by @Y3K on `crates/ty_python_semantic/src/types.rs`:7907 on 2025-09-03 16:40_

Added it back (not pushed)

---

_@Y3K reviewed on 2025-09-03 16:43_

---

_Review comment by @Y3K on `crates/ty_python_semantic/resources/mdtest/with/async.md`:88 on 2025-09-03 16:43_

Sorry for the rookie question.

Can I get a really brief example of how snapshots work?

e.g. should be snapshotted in main or post changes? What if the message changes do I need to update snaps? Are all the snaps run when I do `cargo nextest run`?

---

_Converted to draft by @Y3K on 2025-09-03 17:33_

---
