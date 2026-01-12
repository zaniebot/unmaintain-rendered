```yaml
number: 16959
title: "[red-knot] Recognize an assignment in a function/class scopes as a global-scope definition for purposes of `*` imports if the assigned symbol is declared `global`"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - ty
assignees: []
base: main
head: alex/global-statements
created_at: 2025-03-24T20:53:12Z
updated_at: 2025-03-28T18:04:17Z
url: https://github.com/astral-sh/ruff/pull/16959
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] Recognize an assignment in a function/class scopes as a global-scope definition for purposes of `*` imports if the assigned symbol is declared `global`

---

_@AlexWaygood_

## Summary

Further work towards https://github.com/astral-sh/ruff/issues/14169. If something like this exists in a `foo.py` module:

```py
def f():
    global a
    a: bool = True
```

and a `bar.py` module has this:

```py
from foo import *

reveal_type(a)
```

then we will now recognize `a` as bound in `bar.py`. We don't _yet_ reveal `bool` as the inferred type, but that depends on fixing https://github.com/astral-sh/ty/issues/220.

## Test Plan

Assertions in existing mdtests are modified, and some new assertions are added


---

_Label `red-knot` added by @AlexWaygood on 2025-03-24 20:53_

---

_Review requested from @carljm by @AlexWaygood on 2025-03-24 20:53_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-03-24 20:53_

---

_Review requested from @dcreager by @AlexWaygood on 2025-03-24 20:53_

---

_Renamed from "[red-knot] Recognize an assignment in a function/class scopes as a global-scope definition if the assigned symbol is declared `global`" to "[red-knot] Recognize an assignment in a function/class scopes as a global-scope definition for purposes of `*` imports if the assigned symbol is declared `global`" by @AlexWaygood on 2025-03-24 20:53_

---

_Comment by @github-actions[bot] on 2025-03-24 20:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:35 on 2025-03-24 21:17_

Nit: I prefer to store the owned hash set on the finder and return it from there. It results in fewer lifetimes
```suggestion
    finder.exports
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:62 on 2025-03-24 21:19_

I don't know how hot this code path is but we could consider using hashbrown's hash table to only compute the hash key once (for `global_declarations_in_scope` and `exports`). But that's probably a premature optimization :)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:35 on 2025-03-24 21:21_

Okay, I think it makes sense now, seeing how you use it as a nested visitor. You could use `std::mem::take` to take the exports and then assign it to the inner visitor but not sure if that's worth getting rid of the lifetime.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:313 on 2025-03-24 21:22_

Do we need the second lifetime here or could we use `'a` in places of both `'a` and `'b` (without the borrow checker getting upset?

---

_@MichaReiser reviewed on 2025-03-24 21:22_

---

_@AlexWaygood reviewed on 2025-03-24 21:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:35 on 2025-03-24 21:35_

Nice, the suggestion of using `std::mem::take()` made things a lot cleaner!

---

_@AlexWaygood reviewed on 2025-03-24 21:37_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:62 on 2025-03-24 21:37_

I'll leave this for now (following our in-person conversation) but happy to look into this later if it shows up in profiling etc.!

---

_Review request for @sharkdp removed by @sharkdp on 2025-03-28 12:48_

---

_@carljm reviewed on 2025-03-28 16:29_

Sorry for not getting to clarity on this sooner, but I feel like we shouldn't support this. While this PR doesn't add a lot of complexity, fully supporting this pattern will, because it means that the types of symbols in an outer scope can depend on an arbitrary nested scope.

I think it is reasonable to require that if you use `global` in a function and assign to that name, the name must explicitly exist in the global scope.

If we do end up deciding we have to support this pattern, I want to add full support for it altogether (that is, not do star-import support for it separately from actual type inference support) so we can see the full picture of what it requires.

---

_Comment by @AlexWaygood on 2025-03-28 18:04_

yes, after discussing this with you the other day, I agree that we probably shouldn't do this right now. Let's add support for `global` statements generally first, and see where we land on that. We can possibly revive this after we do that, but it doesn't seem like a priority in any event.

---

_Closed by @AlexWaygood on 2025-03-28 18:04_

---

_Branch deleted on 2025-03-28 18:04_

---
