```yaml
number: 339
title: "`[invalid-return-type]` should not be emitted on function definitions inside `if TYPE_CHECKING` blocks"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-05-12T23:45:18Z
updated_at: 2025-07-21T16:46:40Z
url: https://github.com/astral-sh/ty/issues/339
synced_at: 2026-01-10T02:06:24Z
```

# `[invalid-return-type]` should not be emitted on function definitions inside `if TYPE_CHECKING` blocks

---

_Issue opened by @AlexWaygood on 2025-05-12 23:45_

### Summary

It's fairly common for `.py` files to have "stub-like sections" of code that are added just for a type checker's benefit. These "stub-like sections" generally do not have any function bodies (just `...`), and are generally guarded by `if TYPE_CHECKING`. As an example, see this code in the `trio` library: https://github.com/python-trio/trio/blob/ac7b93f05796d49f324b231b3b2751facd34bc3a/src/trio/_file_io.py#L300-L344.

We shouldn't emit `invalid-return-type` for functions like this that are inside `if TYPE_CHECKING` blocks; we should omit checking these functions for that lint, just like we do for actual stub files.

---

_Label `help wanted` added by @AlexWaygood on 2025-05-12 23:45_

---

_Label `bug` added by @dhruvmanila on 2025-05-13 13:58_

---

_Comment by @MatthewMckee4 on 2025-05-13 23:16_

Shouldn't we also not emit `invalid-return-type` for `foo` in the else clause here? (which we do right now)

```py
if TYPE_CHECKING:
    def foo() -> int: ...
else:
    def foo() -> int: ...
```
(maybe not the best example)
Both mypy and pyright do not emit anything here.

I don't think we should so this is another issue we currently have.

Maybe a place for a warning though?

---

_Comment by @AlexWaygood on 2025-05-13 23:23_

> Shouldn't we also not emit `invalid-return-type` for `foo` in the else clause here? (which we do right now)

I think we should emit the diagnostic for the `else`-clause definition here. The `else` clause here is a definition that will actually be executed at runtime. And at runtime, it won't actually return an `int`. Allowing the definition to go without a diagnostic would lead to unsound behaviour.

>  Both mypy and pyright do not emit anything here.

Yes, both mypy and pyright have some heuristics that mean that they ignore all functions that only have `...` as their body when they apply this lint. We'd like to try doing something a bit more principled (but a bit more complex!), which is what's suggested in this issue.

---

_Comment by @InSyncWithFoo on 2025-05-16 15:24_

I think special-casing `TYPE_CHECKING` as a type-checking-time/runtime differentiator is a bad way to go about this. It will only cause more problems in the future, especially when ty adds support for user-defined constants.

Plus, defining functions with a `...`-only body (and optionally a docstring) is such a common pattern that, I think, it deserves a dedicated lint (say, `invalid-return-type-ellipsis`) that is disabled by default. This lint will then be used for such functions instead of the existing `invalid-return-type`, excluding known special cases like overloads and abstract methods.

---

_Comment by @JelleZijlstra on 2025-05-16 16:12_

> I think we should emit the diagnostic for the `else`-clause definition here. The `else` clause here is a definition that will actually be executed at runtime. And at runtime, it won't actually return an `int`. Allowing the definition to go without a diagnostic would lead to unsound behaviour.

Isn't the point of `if TYPE_CHECKING` that the code in the else branch is not checked by the type checker? You shouldn't emit any diagnostics for the else block.

---

_Comment by @AlexWaygood on 2025-05-16 16:20_

> Isn't the point of `if TYPE_CHECKING` that the code in the else branch is not checked by the type checker? You shouldn't emit any diagnostics for the else block.

Good point, thanks!

---

_Comment by @evanrittenhouse on 2025-05-26 15:15_

I'll pick this up

---

_Assigned to @evanrittenhouse by @AlexWaygood on 2025-05-26 17:50_

---

_Comment by @AlexWaygood on 2025-06-02 11:13_

@evanrittenhouse -- LMK if you need any pointers about how to tackle this!

---

_Comment by @MatthewMckee4 on 2025-07-10 20:01_

@AlexWaygood I'd be interested in picking this up if Evan isn't working on it anymore. I tried to have a go at it previously.

I had made a variable in `InferContext` `in_type_checking_block: bool`. But it seemed like i was setting that in the wrong pass through of a file. So, any pointers are appreciated

---

_Assigned to @MatthewMckee4 by @MichaReiser on 2025-07-11 07:06_

---

_Unassigned @evanrittenhouse by @MichaReiser on 2025-07-11 07:06_

---

_Comment by @carljm on 2025-07-15 00:25_

@MatthewMckee4 I think we can recognize an `if TYPE_CHECKING:` block in semantic indexing (that is, in `SemanticIndexBuilder`), and it will be better to implement this there, because that's where we always do a full AST walk with context. In type inference, we may visit the AST out of order or "helicopter in" to a specific function definition without even visiting the surrounding AST, so I don't think this can be implemented purely in type inference. I think we will need to record in the semantic index in some way which function definitions are in a `TYPE_CHECKING` block, and then refer to this information when deciding whether to emit the invalid-return-type diagnostic in type inference.

---

_Closed by @carljm on 2025-07-16 20:48_

---

_Comment by @karlicoss on 2025-07-20 21:10_

Reading this thread and I'm a bit confused (hope you don't mind asking here since seems relevant). Should the code under `if TYPE_CHECKING`  be type checked at all?

Or the idea is that `ty` will omit/relax some checks but not other checks? I.e. it might still make sense to check "insides" of TYPE_CHECKING clause, but shadow their type context from the outside typing context?

I.e. this results in `a: Literal[123]`  (whereas if the condition was "normal" bool, it would have been `a: Literal[123, "abc"]`)
```
from typing import TYPE_CHECKING, reveal_type

if TYPE_CHECKING:
    a = 123
else:
    a = "abc"

reveal_type(a)
```
However, if you make a type error, e.g. like this: `a: str = 123`, it would still be flagged by ty? 

---

_Comment by @sharkdp on 2025-07-21 06:49_

> Should the code under `if TYPE_CHECKING` be type checked at all

> Or the idea is that ty will omit/relax some checks but not other checks?

Code inside these blocks is written specifically for type checkers, so yes, it should be checked. What's special about these blocks is that we know that they will never be executed at runtime, so *some* rules can be relaxed — correct.

> I.e. it might still make sense to check "insides" of TYPE_CHECKING clause, but shadow their type context from the outside typing context?

I don't know exactly what you mean by that, but I don't think there should be any sort of boundary there. Code in `if TYPE_CHECKING` sections is often the type *setup* for what comes after. So code inside these branches should interact with code outside these branches in the usual way.

> I.e. this results in a: Literal[123] (whereas if the condition was "normal" bool, it would have been a: Literal[123, "abc"])

That's working as intended, yes. `if TYPE_CHECKING` blocks and their counterparts (the `else` branch here) allow you to selectively show code to the type checker and to hide code from the type checker.

> However, if you make a type error, e.g. like this: `a: str = 123`, it would still be flagged by ty?

Yes. Code like this will even be flagged inside the `else` branch. It's definitely wrong. If you really want to convince the type checker that `123` is of type `str`, you need to use `typing.cast`.

---

_Comment by @karlicoss on 2025-07-21 09:52_

Aah sorry 🤦 I made a typo, what I wanted to ask  was "Should the code under **not TYPE_CHECKING** be type checked at all?".

I.e. the following snippet is rejected by ty with "Object of type `Literal[123]` is not assignable to `str` (invalid-assignment)"
```
from typing import TYPE_CHECKING

if not TYPE_CHECKING:
    a: str = 123
else:
    a = "abc"
```

Whereas for instance mypy just ignores things inside `not TYPE_CHECKING` completely.

Hopefully this question makes more sense now if you account for the typo:

> I.e. it might still make sense to check "insides" of TYPE_CHECKING clause, but shadow their type context from the outside typing context

But I think you answered it here: "Yes. Code like this will even be flagged inside the else branch. It's definitely wrong." -- thanks!

---

_Comment by @carljm on 2025-07-21 16:46_

We consider code under `if not TYPE_CHECKING` as unreachable, and apply the same rules to it that we apply to any unreachable code. This silences many diagnostics, but not all -- some simple self-contained cases that don't rely on anything outside the unreachable block and are definitely wrong will still be flagged, even in unreachable code.

---
