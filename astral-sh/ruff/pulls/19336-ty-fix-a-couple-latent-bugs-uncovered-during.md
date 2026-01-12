```yaml
number: 19336
title: "[ty] fix a couple latent bugs uncovered during `nonlocal` work"
type: pull_request
state: merged
author: oconnor663
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: jack/read_declaration_only
created_at: 2025-07-14T17:24:24Z
updated_at: 2025-07-16T15:30:43Z
url: https://github.com/astral-sh/ruff/pull/19336
synced_at: 2026-01-12T15:56:37Z
```

# [ty] fix a couple latent bugs uncovered during `nonlocal` work

---

_@oconnor663_

https://github.com/astral-sh/ty/issues/793

This PR includes two commits for two different bugs (I could separate them, but there would be a merge conflict, because they touch the same line):

- Free reads should terminate at an annotation-only declaration in an enclosing scope, even if that scope doesn't have a binding (in part because a sibling inner function could provide a binding).
- Free reads should terminate at a `global` declaration in an enclosing scope, even if that scope doesn't have a binding (a type annotation would be a syntax error). Usually the global scope will have a definition, and we currently require that it does, although CPython doesn't require that at runtime. (Also add a test case for that.)

---

_Review requested from @carljm by @oconnor663 on 2025-07-14 17:24_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-07-14 17:24_

---

_Review requested from @sharkdp by @oconnor663 on 2025-07-14 17:24_

---

_Review requested from @dcreager by @oconnor663 on 2025-07-14 17:24_

---

_@oconnor663 reviewed on 2025-07-14 17:25_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/scopes/global.md`:251 on 2025-07-14 17:25_

This is a test for existing behavior, not new in this PR.

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-07-14 17:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/scopes/global.md`:240 on 2025-07-14 17:27_

```suggestion
    # TODO: we should emit an error on the assignment to `z` because we're adding a variable
    # to an external scope when it does not have any declarations or bindings in that scope
    # (similar conceptually to monkey-patching a new attribute onto a class/instance)
    x, y, z = 1, 2, 3
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/scopes/nonlocal.md`:41 on 2025-07-14 17:28_

👍

---

_Comment by @github-actions[bot] on 2025-07-14 17:29_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
porcupine (https://github.com/Akuli/porcupine)
- error[unresolved-reference] porcupine/utils.py:794:37: Name `value` used when not defined
- Found 34 diagnostics
+ Found 33 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
+ error[unresolved-reference] Pythonwin/pywin/Demos/ocx/ocxtest.py:63:42: Name `calendarParentModule` used when not defined
- Found 2013 diagnostics
+ Found 2014 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ warning[possibly-unbound-attribute] openlibrary/plugins/openlibrary/sentry.py:17:45: Attribute `capture_exception_webpy` on type `Sentry | None` is possibly unbound
- Found 680 diagnostics
+ Found 681 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/scopes/nonlocal.md`:61 on 2025-07-14 17:30_

```suggestion
            y: int = x  # allowed: this loads the global `x` variable
                        # due to the `global` declaration in the immediate enclosing scope
```

---

_@AlexWaygood approved on 2025-07-14 17:31_

Brilliant! Could you double-check the mypy_primer hits are as expected?

---

_Label `ty` added by @AlexWaygood on 2025-07-14 17:31_

---

_Renamed from "fix a couple latent bugs uncovered during `nonlocal` work" to "[ty] fix a couple latent bugs uncovered during `nonlocal` work" by @AlexWaygood on 2025-07-14 17:31_

---

_Label `ecosystem-analyzer` removed by @oconnor663 on 2025-07-14 19:46_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-07-14 19:47_

---

_Comment by @github-actions[bot] on 2025-07-14 19:55_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-reference` | 1 | 1 | 0 |
| `possibly-unbound-attribute` | 1 | 0 | 0 |
| **Total** | **2** | **1** | **0** |

**[Full report with detailed diff](https://jack-read-declaration-only.ecosystem-663.pages.dev/diff)**


---

_Comment by @oconnor663 on 2025-07-14 20:02_

The first `mypy_primer` change [removes a false positive](https://github.com/Akuli/porcupine/blob/b6ea2acb9b4786f226dbaec6d234130ffd5788af/porcupine/utils.py#L776-L794). :tada: 

The second change is [a new lint that I'm not sure about](https://github.com/mhammond/pywin32/blob/62821250e83cc1c1c2b4a613f4eb9f93db1c8fe9/Pythonwin/pywin/Demos/ocx/ocxtest.py#L52-L63). It boils down to an example like this. Should `print(x)` be an unresolved reference error here? Talking to @AlexWaygood, it sounds like we do want this example to error (or at least we're ok with it erroring), but showing an error _only_ on the print is probably the most confusing way to do it. I need to look into exactly where the `x=5` binding is going.
```py
def f():
    global x
    x = 5
    def g():
        print(x)
```

The third change is [a new lint that I think is a true positive](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/openlibrary/sentry.py#L9-L17), but I'm not certain. It boils down to this:
```py
x: int | None = None

def f():
    global x
    x = 5
    def g():
        y: int = x
```
Previously the `infer_place_load` for `x` in `g` stopped at `f`'s scope, and I think it concluded that `x` was `Literal[5]`. But under this PR it sees that `x` is `global` in `f` and skips straight to the global scope, where it sees `int | None` and concludes that that's not assignable to `int`. I think the new behavior is better?

---

_Comment by @AlexWaygood on 2025-07-14 20:04_

> The third change is [a new lint that I think is a true positive](https://github.com/internetarchive/openlibrary/blob/master/openlibrary/plugins/openlibrary/sentry.py?rgh-link-date=2025-07-14T20%3A02%3A13Z#L9-L17), but I'm not certain. It boils down to this:
> 
> ```python
> x: int | None = None
> 
> def f():
>     global x
>     x = 5
>     def g():
>         y: int = x
> ```
> 
> Previously the `infer_place_load` for `x` in `g` stopped at `f`'s scope, and I think it concluded that `x` was `Literal[5]`. But under this PR it sees that `x` is `global` in `f` and skips straight to the global scope, where it sees `int | None` and concludes that that's not assignable to `int`. I think the new behavior is better?

this one feels related to https://github.com/astral-sh/ty/issues/311: since we don't narrow globals in other contexts yet, I think it's fine if we don't do that narrowing here either!

---

_Comment by @oconnor663 on 2025-07-14 20:12_

(`ecosystem-analyzer` reports the same 3 cases as `mypy_primer`.)

---

_Converted to draft by @oconnor663 on 2025-07-14 20:47_

---

_Comment by @oconnor663 on 2025-07-15 00:09_

I've split out the question of how we want to lint these "`global` statement doesn't correspond to any explicitly defined global variable" case into https://github.com/astral-sh/ruff/pull/19344. I'll come back and rebase this once we decide what to do with that.

---

_Comment by @oconnor663 on 2025-07-15 19:12_

I've moved this PR to be on top of https://github.com/astral-sh/ruff/pull/19344, which makes it an error to use the `global` keyword when there's no matching definition in the global scope. ~~Both of [the tricky examples above](https://github.com/astral-sh/ruff/pull/19336#issuecomment-3070801379) fall into that category, so I'm not too worried about whether there are secondary errors below the `global` keyword. Marking this ready for review again.~~

Edit: Ah wait, that was wrong. Gonna think about this some more.

---

_Marked ready for review by @oconnor663 on 2025-07-15 19:12_

---

_Converted to draft by @oconnor663 on 2025-07-15 19:14_

---

_Label `ecosystem-analyzer` removed by @oconnor663 on 2025-07-16 00:46_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-07-16 00:46_

---

_Comment by @oconnor663 on 2025-07-16 01:10_

Ok, yes, the [first new lint](https://github.com/mhammond/pywin32/blob/8b328dffac71b7afaf2d72f47c4048f27a32f6c8/Pythonwin/pywin/Demos/ocx/ocxtest.py#L63) follows an invalid `global` keyword that we also lint on (now that https://github.com/astral-sh/ruff/pull/19344 is in), and I think it's fine.

The [second new lint](https://github.com/internetarchive/openlibrary/blob/fb3a5561bd912ee8a3bb3d94b278d7dcc8ad8100/openlibrary/plugins/openlibrary/sentry.py#L17) is because the program is relying on narrowing globals across function scopes, which as @AlexWaygood pointed out we don't currently do, and which is probably unsound in general. (It's clear to me in this case that the value will never become `None` again after it's stopped being `None`, but I don't think Ty can see that. Maybe the global variable should be declared `sentry: Sentry` rather than `sentry: Sentry | None = None`?)

So yes, I'm ok with these lints and I think this is ready to land. I'll wait until tomorrow in case anyone else wants to review, since it's toggled back and forth a couple times.

---

_Marked ready for review by @oconnor663 on 2025-07-16 01:10_

---

_Review requested from @MichaReiser by @oconnor663 on 2025-07-16 01:10_

---

_@MichaReiser approved on 2025-07-16 08:25_

---

_Merged by @oconnor663 on 2025-07-16 15:30_

---

_Closed by @oconnor663 on 2025-07-16 15:30_

---

_Branch deleted on 2025-07-16 15:30_

---
