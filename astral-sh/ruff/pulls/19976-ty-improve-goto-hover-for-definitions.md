```yaml
number: 19976
title: "[ty] improve goto/hover for definitions"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/goto-def-def
created_at: 2025-08-18T20:49:09Z
updated_at: 2025-08-19T15:26:57Z
url: https://github.com/astral-sh/ruff/pull/19976
synced_at: 2026-01-10T17:52:17Z
```

# [ty] improve goto/hover for definitions

---

_Pull request opened by @Gankra on 2025-08-18 20:49_

By computing the actual Definition for, well, definitions, we unlock a bunch of richer machinery in the goto/hover subsystems for free. 

Fixes https://github.com/astral-sh/ty/issues/1001
Fixes https://github.com/astral-sh/ty/issues/1004

---

_Review requested from @carljm by @Gankra on 2025-08-18 20:49_

---

_Review requested from @AlexWaygood by @Gankra on 2025-08-18 20:49_

---

_Review requested from @sharkdp by @Gankra on 2025-08-18 20:49_

---

_Review requested from @dcreager by @Gankra on 2025-08-18 20:49_

---

_Review requested from @MichaReiser by @Gankra on 2025-08-18 20:49_

---

_Comment by @Gankra on 2025-08-18 20:50_

mea culpa: this does not fix https://github.com/astral-sh/ty/issues/1004 *for vendored typeshed definitions* (possibly stdlib-specific?). It seems like file_to_module doesn't understand opened-in-your-ide vendored typeshed files? Something to look into more in a followup.

---

_Comment by @github-actions[bot] on 2025-08-18 20:52_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-18 20:53_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review comment by @Gankra on `crates/ty_ide/src/goto_references.rs`:409 on 2025-08-18 20:53_

Love when a fix suddenly enriches an existing test case :D

---

_@Gankra reviewed on 2025-08-18 20:53_

---

_Label `server` added by @AlexWaygood on 2025-08-18 21:53_

---

_Label `ty` added by @AlexWaygood on 2025-08-18 21:53_

---

_@carljm approved on 2025-08-19 01:03_

Love it.

---

_Merged by @Gankra on 2025-08-19 01:42_

---

_Closed by @Gankra on 2025-08-19 01:42_

---

_Branch deleted on 2025-08-19 01:42_

---

_@AlexWaygood reviewed on 2025-08-19 06:42_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/goto_references.rs`:446 on 2025-08-19 06:42_

Hmm, is this one correct? It looks like a reference to a different variable with the same name to me 

---

_@AlexWaygood reviewed on 2025-08-19 06:44_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/goto_references.rs`:446 on 2025-08-19 06:44_

I doubt your PR introduced a bug here, just wondering if we should have a TODO comment or open an issue!

---

_@carljm reviewed on 2025-08-19 06:59_

---

_Review comment by @carljm on `crates/ty_ide/src/goto_references.rs`:446 on 2025-08-19 06:59_

Same variable, different definitions. Except blocks don't create subscopes, they just overwrite whatever was previously in that variable. So I think this is the expected behavior. 

---

_@MichaReiser reviewed on 2025-08-19 07:25_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto_references.rs`:446 on 2025-08-19 07:25_

This also matches pycharm's behavior

---

_@AlexWaygood reviewed on 2025-08-19 09:03_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/goto_references.rs`:446 on 2025-08-19 09:03_

> Except blocks don't create subscopes, they just overwrite whatever was previously in that variable.

You know that I know that ;) I'm not sure it's relevant, though.

Determining the set of references to a given symbol is important for code actions such as being able to right-click on a symbol in an IDE and rename it. If I right clicked on `err` here in the `except ValueError as err` branch, I'm not sure I'd expect the `err` symbol in the second `except` branch to be renamed as well? It has a wholly independent definition from the `err` symbol in the first branch

```py
try:
    foo
except ValueError as err:
    print(err)
except TypeError as err:
    print(err)
```

---

_@AlexWaygood reviewed on 2025-08-19 09:04_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/goto_references.rs`:446 on 2025-08-19 09:04_

Obviously not a critical debate to have right now if our current behaviour matches other mainstream language servers, though

---

_@MichaReiser reviewed on 2025-08-19 09:08_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto_references.rs`:446 on 2025-08-19 09:08_

That does make sense to me but I think it might get tricky if there are any `err` usages after the `except` block. It then is important that they all have the same name or renaming only some of them changes program semantics. 

This sort of lookahead is probably just complicated enough that renaming all instances is a reasonable default (it reduces the risk of breaking anything or, at least, can avoid any heavy machinary to figure out if it would)

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/goto_references.rs`:446 on 2025-08-19 09:13_

> That does make sense to me but I think it might get tricky if there are any `err` usages after the `except` block.

For `except` definitions specifically, that can't happen because although the `except` branch doesn't introduce a new scope, the interpreter inserts an implicit `del` of the variable bound by the `except` statement at the end of the block:

```pycon
>>> try:
...     1 / 0
... except ZeroDivisionError as e:
...     print(e)
...     
division by zero
>>> print(e)
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    print(e)
          ^
NameError: name 'e' is not defined
```

but yes, I see your point!

---

_@AlexWaygood reviewed on 2025-08-19 09:13_

---

_@carljm reviewed on 2025-08-19 15:16_

---

_Review comment by @carljm on `crates/ty_ide/src/goto_references.rs`:446 on 2025-08-19 15:16_

I don't think we should treat `except` defined variables differently due to their implicit `del` behavior; that _is_ going down the route of sort of treating them as a separate sub-scope. And in general, I think Micha's comment accurately describes why we shouldn't / can't try to treat separate definitions of the same variable in the same scope as separate variables for purposes of goto-references, rename, etc.

---

_@AlexWaygood reviewed on 2025-08-19 15:26_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/goto_references.rs`:446 on 2025-08-19 15:26_

makes sense -- thanks!

---
