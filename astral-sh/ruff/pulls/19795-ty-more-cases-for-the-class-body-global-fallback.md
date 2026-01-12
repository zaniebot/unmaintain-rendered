```yaml
number: 19795
title: "[ty] more cases for the class body global fallback"
type: pull_request
state: merged
author: oconnor663
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: jack/class_body_corner_cases
created_at: 2025-08-06T22:09:59Z
updated_at: 2025-08-08T00:30:29Z
url: https://github.com/astral-sh/ruff/pull/19795
synced_at: 2026-01-12T15:56:47Z
```

# [ty] more cases for the class body global fallback

---

_@oconnor663_

I had been tracking https://github.com/astral-sh/ty/issues/875, and I took a look at @mtshiba's https://github.com/astral-sh/ruff/pull/19743 after it closed that bug. It looks like there are some obscure corner cases that don't match CPython's behavior, and some of the refactoring I've been doing for https://github.com/astral-sh/ruff/pull/19703 (particularly a `Symbol::is_local` method) might be helpful. I expect that all these cases are extremely rare, so this PR is more for my own understanding and to kick the tires on the refactorings I'm already considering. ~~I don't expect to get any ecosystem hits but we'll see.~~ (lol what is scipy doing :sweat_smile:)

This PR adds three new cases to the class body global fallback tests in `global.md`:

```py
x: str = "a"

def f(x: int, y: int):
    ...
    # Declarations count as definitions, even if there's no binding.
    class F:
        reveal_type(x)  # revealed: str
        x: int
        reveal_type(x)  # revealed: str

    # Explicitly `nonlocal` variables don't count, even if they're bound.
    class G:
        nonlocal x
        reveal_type(x)  # revealed: int
        x = 42
        reveal_type(x)  # revealed: Literal[42]

    # Possibly-unbound variables get unioned with the fallback lookup.
    class H:
        if secrets.randbelow(2):
            x = None
        reveal_type(x)  # revealed: None | str
```

These cases all fail in `main` and pass with this PR.

---

_Review requested from @carljm by @oconnor663 on 2025-08-06 22:10_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-08-06 22:10_

---

_Review requested from @sharkdp by @oconnor663 on 2025-08-06 22:10_

---

_Review requested from @dcreager by @oconnor663 on 2025-08-06 22:10_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/types/infer.rs`:6688 on 2025-08-06 22:11_

Removing `local_is_unbound` from the logic here lets us fall back both the `Unbound` case (as before) and also in the `PossiblyUnbound` case (new in this PR). The `Bound` case never gets here, because we're inside of `or_fall_back_to`.

---

_@oconnor663 reviewed on 2025-08-06 22:11_

---

_Comment by @github-actions[bot] on 2025-08-06 22:11_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-06 22:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scipy (https://github.com/scipy/scipy)
- scipy/optimize/_minpack_py.py:521:13: error[unresolved-attribute] Unresolved attribute `skip_lookup` on type `def _memoized_func(params) -> Unknown`.
- scipy/optimize/tests/test_chandrupatla.py:358:41: error[unresolved-attribute] Type `def callback(res) -> Unknown` has no attribute `xl`
- scipy/optimize/tests/test_chandrupatla.py:358:67: error[unresolved-attribute] Type `def callback(res) -> Unknown` has no attribute `xr`
- scipy/optimize/tests/test_chandrupatla.py:359:41: error[unresolved-attribute] Type `def callback(res) -> Unknown` has no attribute `xl`
- scipy/optimize/tests/test_chandrupatla.py:359:67: error[unresolved-attribute] Type `def callback(res) -> Unknown` has no attribute `xr`
- scipy/optimize/tests/test_chandrupatla.py:756:48: error[unresolved-attribute] Type `def callback(res) -> Unknown` has no attribute `bracket`
- scipy/optimize/tests/test_chandrupatla.py:757:50: error[unresolved-attribute] Type `def callback(res) -> Unknown` has no attribute `bracket`
- scipy/optimize/tests/test_chandrupatla.py:758:50: error[unresolved-attribute] Type `def callback(res) -> Unknown` has no attribute `bracket`
- scipy/optimize/tests/test_chandrupatla.py:759:52: error[unresolved-attribute] Type `def callback(res) -> Unknown` has no attribute `bracket`
- Found 6796 diagnostics
+ Found 6787 diagnostics

```
</details>
No memory usage changes detected ✅


---

_@oconnor663 reviewed on 2025-08-06 22:17_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:78 on 2025-08-06 22:17_

This is the one part of this PR I'm really not sure about. Previously we weren't applying the global fallback (because `a` is possibly bound), but we were still walking enclosing scopes (because `scope.is_function_like(db)` used to be part of the `Unbound` short-circuit in `infer_place_load`). I _think_ that caused us to look up `a.x` here from an enclosing snapshot, and now we don't?

---

_Label `ty` added by @oconnor663 on 2025-08-06 22:17_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-08-06 22:17_

---

_Label `ecosystem-analyzer` removed by @oconnor663 on 2025-08-06 22:47_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-08-06 22:47_

---

_Comment by @carljm on 2025-08-06 22:48_

Ecosystem hits: it looks like even in non-class scopes, this is now causing us to find an attribute write in an enclosing scope in some cases where previously we did not. That seems like a positive change.

---

_Comment by @github-actions[bot] on 2025-08-06 22:57_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 0 | 9 | 0 |
| **Total** | **0** | **9** | **0** |

**[Full report with detailed diff](https://jack-class-body-corner-cases.ecosystem-663.pages.dev/diff)**


---

_Comment by @oconnor663 on 2025-08-06 23:01_

I'm finding it pretty hard to pin down exactly what's changed there. I think I can minimize those scipy examples down to something like this:

```py
def f():
    def g():
        if g.x is not None:
            g.x = 42
    g.x = None
```

This is _weird_ code, and it has plenty of diagnostics both before (3) and after (2) this PR. But the one removed diagnostic is on the assignment `g.x = 42`. I _think_ `ty` is now concluding that that line is unreachable? Previously we used to skip walking local scopes if `has_bindings_in_this_scope` was true, and now we skip it if `symbol_resolves_locally` is true, and the subtle difference is that the former can be true for attribute/subscript places but the latter can't be. This wasn't exactly intended, but it does seem like "skip walking enclosing scopes" should only apply to bare symbols. Consider this other example I just made up:

```py
def f():
    x = [1, 2, 3]
    x[0] = 42
    def g():
        reveal_type(x[0])  # previously Unknown, now 42
        x[0] = 99
        reveal_type(x[0])  # 99
```

That seems like an improvement? But I don't claim to understand our non-symbol place logic very well.

---

_Comment by @oconnor663 on 2025-08-07 00:05_

(copying a question [from chat](https://discord.com/channels/1039017663004942429/1343690517745111081/1402792898377945149) to make it easier for me to find later)

Speaking of non-symbol places, it seems like our handling of those is kind of unsound. For example (both before and after this PR):

```py
def f():
    x = ["a"]
    x[0] = 42
    def g():
        x = ["b"]
        def h():
            reveal_type(x[0])  # 42
            print(x[0])        # "b"
```

Is that a known issue?

---

_Comment by @carljm on 2025-08-07 23:16_

> Is that a known issue?

It is now! And I just merged @mtshiba 's fix for it :)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/narrow/assignment.md`:78 on 2025-08-07 23:41_

Yes, that looks likely, and we apply the Unknown-widening to the global `A` but not the function-local one.

---

_@carljm approved on 2025-08-07 23:56_

Looks great, thank you!

---

_Merged by @oconnor663 on 2025-08-08 00:30_

---

_Closed by @oconnor663 on 2025-08-08 00:30_

---

_Branch deleted on 2025-08-08 00:30_

---
