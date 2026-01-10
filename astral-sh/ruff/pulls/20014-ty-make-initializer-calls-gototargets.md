```yaml
number: 20014
title: "[ty] Make initializer calls GotoTargets"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/goto-init
created_at: 2025-08-21T00:19:37Z
updated_at: 2025-09-02T18:49:16Z
url: https://github.com/astral-sh/ruff/pull/20014
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Make initializer calls GotoTargets

---

_Pull request opened by @Gankra on 2025-08-21 00:19_

This introduces `GotoTarget::Call` that represents the kind of ambiguous/overloaded click of a callable-being-called:

```py
x = mymodule.MyClass(1, 2)
             ^^^^^^^
```

This is equivalent to `GotoTarget::Expression` for the same span but enriched
with information about the actual callable implementation.

That is, if you click on `MyClass` in `MyClass()` it is *both* a
reference to the class and to the initializer of the class. Therefore
it would be ideal for goto-* and docstrings to be some intelligent
merging of both the class and the initializer.

In particular the callable-implementation (initializer) is prioritized over the callable-itself (class) so when showing docstrings we will preferentially show the docs of the initializer if it exists, and then fallback to the docs of the class.

For goto-definition/goto-declaration we will yield both the class and the initializer, requiring you to pick which you want (this is perhaps needlessly pedantic but...).

Fixes https://github.com/astral-sh/ty/issues/898
Fixes https://github.com/astral-sh/ty/issues/1010

---

_Label `server` added by @Gankra on 2025-08-21 00:19_

---

_Review requested from @carljm by @Gankra on 2025-08-21 00:19_

---

_Review requested from @MichaReiser by @Gankra on 2025-08-21 00:19_

---

_Review requested from @AlexWaygood by @Gankra on 2025-08-21 00:19_

---

_Label `ty` added by @Gankra on 2025-08-21 00:19_

---

_Review requested from @sharkdp by @Gankra on 2025-08-21 00:19_

---

_Review requested from @dcreager by @Gankra on 2025-08-21 00:19_

---

_Comment by @github-actions[bot] on 2025-08-21 00:22_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-21 00:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@MichaReiser approved on 2025-08-21 08:30_

---

_Comment by @MichaReiser on 2025-08-21 08:31_

> For goto-definition/goto-declaration we will yield both the class and the initializer, requiring you to pick which you want (this is perhaps needlessly pedantic but...).

Pyright jumps to the definition of the class. I'm leaning towards aligning the behavior for now (although it is somewhat unexpected, so maybe what you have is best)

---

_Comment by @Gankra on 2025-08-21 15:14_

Something I've vaguely been considering is another variant of DefinitionsOrTargets that has two lists of definitions: one is the "right" answer and one is "fallback". So docstrings would consider both but goto-def would be opinionated and just pick one?

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-21 18:13_

---

_Comment by @Gankra on 2025-08-21 20:39_

Surprisingly a lot of reports of different IDEs actually making the `(` of the call separately clickable as the ctor call!

---

_Comment by @MichaReiser on 2025-08-22 06:51_

> Surprisingly a lot of reports of different IDEs actually making the `(` of the call separately clickable as the ctor call!

This sounds nice

---

_Comment by @Gankra on 2025-09-02 18:40_

Ok I've rebased this on top of #20148 and minimized its intelligence, making this a reasonably safe change. I'm going to land this now (feedback is the best way to iterate on exact UX of this I think).

---

_Merged by @Gankra on 2025-09-02 18:49_

---

_Closed by @Gankra on 2025-09-02 18:49_

---

_Branch deleted on 2025-09-02 18:49_

---
