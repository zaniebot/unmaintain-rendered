```yaml
number: 17342
title: "[red-knot] Silence errors in unreachable type annotations / class bases"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/unreachable-type-annotations
created_at: 2025-04-10T19:50:41Z
updated_at: 2025-05-07T15:19:46Z
url: https://github.com/astral-sh/ruff/pull/17342
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] Silence errors in unreachable type annotations / class bases

---

_@sharkdp_

## Summary

For silencing `invalid-type-form` diagnostics in unreachable code, we use the same approach that we use before and check the reachability that we already record.

For silencing `invalid-bases`, we simply check if the type of the base is `Never`. If so, we silence the diagnostic with the argument that the class construction would never happen.

## Test Plan

Updated Markdown tests.

---

_Label `red-knot` added by @sharkdp on 2025-04-10 19:50_

---

_Comment by @github-actions[bot] on 2025-04-10 19:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
rich (https://github.com/Textualize/rich)
- error[lint:invalid-base] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:33:26: Invalid class base with type `Never` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:44:57: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-base] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:57:34: Invalid class base with type `Never` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-base] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:67:27: Invalid class base with type `Never` (all bases must be a class, `Any`, `Unknown` or `Todo`)
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:78:43: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:95:32: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:129:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:132:12: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:170:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:173:12: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:205:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:205:46: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:229:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:230:6: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:253:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:253:42: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:276:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:276:47: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:300:17: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:300:47: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:377:34: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:387:30: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:405:46: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_win32_console.py:444:44: Variable of type `Never` is not allowed in a type expression
- error[lint:invalid-type-form] /tmp/mypy_primer/projects/rich/rich/_windows.py:40:43: Variable of type `Never` is not allowed in a type expression
- Found 791 diagnostics
+ Found 766 diagnostics

```
</details>


---

_Marked ready for review by @sharkdp on 2025-04-10 19:58_

---

_Review requested from @carljm by @sharkdp on 2025-04-10 19:58_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-10 19:58_

---

_Review requested from @dcreager by @sharkdp on 2025-04-10 19:58_

---

_Comment by @MichaReiser on 2025-04-10 20:12_

I haven't followed all the reachability discussions and I'm sorry if we discussed this before. 

Is the explicit reachability check now required for all diagnostics? Or is it only a small subset? 

---

_Comment by @sharkdp on 2025-04-10 20:18_

> Is the explicit reachability check now required for all diagnostics? Or is it only a small subset?

I hope this is the last one :smile:. I could imagine that we might need to add it to a few more isolated diagnostics, but it will not be required to check reachability for all diagnostics. That's a feature! We will still report division-by-zero, if you do `1 / 0` in an unreachable block.

Whether that approach truly takes us all the way, I don't know. But for now, this is my last PR w.r.t. unreachable code. This solves the last remaining TODOs in that test suite (expect for some TODOs around a new feature that would highlight/report unreachable code).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:603 on 2025-04-10 20:25_

```suggestion
    /// Check if a given AST node is reachable.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:603 on 2025-04-10 20:26_

Also worth noting here that it doesn't work for arbitrary nodes? The node reachability has to have explicitly been recorded in semantic indexing.

---

_@carljm approved on 2025-04-10 20:28_

---

_Merged by @sharkdp on 2025-04-10 20:47_

---

_Closed by @sharkdp on 2025-04-10 20:47_

---

_Branch deleted on 2025-04-10 20:47_

---

_Comment by @MichaReiser on 2025-04-11 06:20_

I'm not opposed to this approach but I've two thoughts:

1. How would I know if I add a new rule if it needs to check/respect reachability? Should we add some documentation somewhere with some guidance on this?
2. This is not a *now* problem but I wonder how this approach will scale if we start adding lint rules. 

---

_Comment by @sharkdp on 2025-04-11 09:45_

@MichaReiser Excellent questions.



> 1. How would I know if I add a new rule if it needs to check/respect reachability?

I don't know the full answer yet. And I find it hard to formulate a general strategy.

If a rule detects a problem with the code that would also be there if the code was *not* unreachable, then it should report the problem in all cases (not respect reachabililty). So the following two diagnostics should be emitted in unreachable code:
```py
def _():
    return
    x: str = 1  # Object of type `Literal[1]` is not assignable to `str`
    1 / 0  # Cannot divide object of type `Literal[1]` by zero
```

The cases where we need to consider reachability are all related to the fact that the rule would otherwise emit a false positive because it depends on *loading a symbol* from a place in the code that is "on the other side" of the "reachability boundary". For example:

```py
def _():
    class SomeClass: ...
    some_variable = 1

    return

    some_variable  # no unreachable-reference error here, even though `some_variable` is not visible
    x: SomeClass = SomeClass()  # no invalid-type-form error here, even though `SomeClass` is not visible
```

> 1. Should we add some documentation somewhere with some guidance on this?

Yes, probably. Where would you look for this information / where should I put it?

> This is not a _now_ problem but I wonder how this approach will scale if we start adding lint rules.

That is a completely reasonable concern. I don't know how this scales. For now, four (out of ~50) rules needed a special "reachability treatment". Three of these rules (unresolved-reference, unresolved-attribute, unresolved-import) are very obviously related to "looking up symbols". So I would argue that there is just one case (invalid-type-form) that was kind of unexpected for me. It is related to the fact that we infer a type of `Never` for symbols that are on the other side of that reachability boundary. `Never` is a very "permissive" type, and we use it for precisely that reason. It doesn't cause any problems in most of the rules, apparently. But invalid-type-form is special because it affects types in type-expression positions. And we do not allow a type of `Never` in a type expression context (not to be confused with a type expression that *represents* `Never`). Something like
```py
from typing import Never

def _(IsThisAValidTypeExpression: Never):
    x: IsThisAValidTypeExpression  # Variable of type `Never` is not allowed in a type expression (lint:invalid-type-form)
```


So to summarize, I hope that this approach *does* scale. But I could also imagine that we find more problems as we go. And it might turn out that we need to silence *all* diagnostics in unreachable code eventually.

---

_Comment by @MichaReiser on 2025-04-11 11:47_

Thanks. This explanation was very useful!

> Yes, probably. Where would you look for this information / where should I put it?

Hmm, that's a good question. Two ideas come to my mind:

1. Into `red_knot/docs/diagnostics.md`. We could combine it with other guidance around how to write/create diagnostics (@BurntSushi)
2. `red_knot_python_semantics::types::diagnostics` 

But I don't think there's any very obvious place. 

---

_Comment by @sharkdp on 2025-04-14 10:59_

After discovering some problems with our current approach which I [documented here](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/resources/mdtest/unreachable.md#limitations-of-the-current-approach), I decided to postpone the documentation of any guidelines here, as it seems likely that we need to pivot to a different strategy of silencing diagnostics in unreachable code. For now, I would suggest to simply not care about unreachable code when introducing new diagnostics (unless there are obvious false positives in the ecosystem checks).

I opened a new ticket so we don't forget about this: https://github.com/astral-sh/ty/issues/127

---
