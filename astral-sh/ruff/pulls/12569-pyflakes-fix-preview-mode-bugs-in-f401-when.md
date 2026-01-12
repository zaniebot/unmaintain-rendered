```yaml
number: 12569
title: "[`pyflakes`] Fix preview-mode bugs in `F401` when attempting to autofix unused first-party submodule imports in an `__init__.py` file"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: alex/f401-bug
created_at: 2024-07-29T16:39:37Z
updated_at: 2024-08-02T16:34:46Z
url: https://github.com/astral-sh/ruff/pull/12569
synced_at: 2026-01-12T15:55:41Z
```

# [`pyflakes`] Fix preview-mode bugs in `F401` when attempting to autofix unused first-party submodule imports in an `__init__.py` file

---

_@AlexWaygood_

## Summary

Fixes #12513.

To summarise, if you have an unused first-party submodule import (e.g. `import submodule.a`) in an `__init__.py` file:
- Without a preexisting `__all__` definition:
  - We used to produce invalid syntax in our autofix: `import submodule.a as submodule.a`.
  - We now offer no autofix
- With a preexisting `__all__` definition:
  - We used to enter an infinite loop when attempting to provide an autofix
  - We now don't enter an infinite loop, and add `"submodule"` to `__all__` in our autofix

The error message has also changed slightly if you have an unused import in an `__init__.py` file that isn't a first-party import. This was a side effect of the refactoring, but I think it's for the better. For `import sys` in an `__init__.py` file:
- The error message used to be:
  > `sys` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- It is now:
  > `sys` imported but unused

I think that's an improvement. If it's not a first-party import, there's very little chance that the user wants it to be reexported, either by adding it to `__all__` or by using a redundant alias, so the more concise error message feels like an improvement to me.

## Test Plan

`cargo test -p ruff_linter --lib`


---

_Label `fixes` added by @AlexWaygood on 2024-07-29 16:39_

---

_Label `preview` added by @AlexWaygood on 2024-07-29 16:39_

---

_Renamed from "[`pyflakes`] Fix preview-mode infinite loop when attempting to autofix unused first-party submodule import in an `__init__.py` file that has an `__all__` definition" to "[`pyflakes`] Fix preview-mode infinite loop in `F401` when attempting to autofix unused first-party submodule import in an `__init__.py` file that has an `__all__` definition" by @AlexWaygood on 2024-07-29 16:45_

---

_Comment by @github-actions[bot] on 2024-07-29 16:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @qarmin on 2024-07-29 17:41_

Will this fix https://github.com/astral-sh/ruff/issues/12570 ?
Also F401 causes infinite loop, but without `__all__`

---

_Comment by @AlexWaygood on 2024-07-29 17:45_

> Will this fix #12570 ?

Doesn't look like it (not as the PR currently stands, anyway). I can still repro that infinite loop even with this PR branch.

---

_Comment by @AlexWaygood on 2024-07-29 17:59_

I'm still looking into it, but it looks like https://github.com/astral-sh/ruff/issues/12570 is a distinct bug from the one this PR is fixing. So we'll fix it, but not in this PR 👍

---

_@MichaReiser approved on 2024-07-30 06:26_

LGTM. 

What's the reason that you decided not to add sub imports (by adding `a`) to `__ALL__`? Is it because that's not what sub imports are commonly used for?

---

_Comment by @AlexWaygood on 2024-07-30 10:06_

> What's the reason that you decided not to add sub imports (by adding `a`) to `__ALL__`? Is it because that's not what sub imports are commonly used for?

Submodule imports have... odd semantics in Python. Here's what happens if I import a regular module: the `email` module is loaded and assigned to an `email` symbol in the global namespace:

```pycon
>>> globals().keys()
dict_keys(['__name__', '__doc__', '__package__', '__loader__', '__spec__', '__annotations__', '__builtins__'])
>>> import email
>>> globals().keys()
dict_keys(['__name__', '__doc__', '__package__', '__loader__', '__spec__', '__annotations__', '__builtins__', 'email'])
<module 'email' from '/Users/alexw/.pyenv/versions/3.12.4/lib/python3.12/email/__init__.py'>
```

That's _not_ what happens with a submodule import, however:

```pycon
Python 3.12.4 (main, Jun 12 2024, 09:54:41) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import email.utils
>>> globals().keys()
dict_keys(['__name__', '__doc__', '__package__', '__loader__', '__spec__', '__annotations__', '__builtins__', 'email'])
>>> email
<module 'email' from '/Users/alexw/.pyenv/versions/3.12.4/lib/python3.12/email/__init__.py'>
>>> email.utils
<module 'email.utils' from '/Users/alexw/.pyenv/versions/3.12.4/lib/python3.12/email/utils.py'>
```

In the REPL snippet above, the `import email.utils` statement does the following things:
1. Loads the `email` module
2. Assigns the `email` module to an `email` symbol in the global namespace
3. Loads the `email.utils` submodule
4. Assigns the `email.utils` submodule as an `email` attribute on the `email` module that's now in the global namespace.

`__all__` is a list of symbols in the global namespace that are publicly exported from a given module. If `__all__` is validly defined, it should only contain strings which are valid identifiers, and all the identifiers listed in `__all__` should refer to symbols that exist in the global namespace. Therefore, we shouldn't add `submodule.a` or `a` to `__all__`: the first isn't a valid identifier, and neither of them refers to a symbol that exists in the global namespace as a result of the statement `import submodule.a`.

Possibly we _could_ autofix this by adding `submodule` to `__all__`, however... possibly that would be better than just removing the import if we know that the `import submodule.a` statement is a first-party import. @carljm, do you have any opinion here on what the better autofix would be?

---

_Comment by @carljm on 2024-07-30 14:03_

I might be missing some context here, but if we autofix this:

```
import mod
__all__ = ["FOO"]
FOO = 42
```

to this:

```
import mod
__all__ = ["FOO", "mod"]
FOO = 42
```

Then I think we should also autofix this:

```
import mod.sub
__all__ = ["FOO"]
FOO = 42
```

to this:

```
import mod.sub
__all__ = ["FOO", "mod"]
FOO = 42
```

It doesn't make sense to me that we would remove `import mod.sub`, but if its `import mod` we would add `"mod"` to `__all__`. Both `import mod` and `import mod.sub` define the symbol `mod`, and we should handle them consistently; either remove the import as autofix in both cases, or add to `__all__` in both cases.

---

_Comment by @AlexWaygood on 2024-07-30 14:07_

I don't think you're missing something! I think I just spent too long tracking down the source of the bug, and didn't think hard enough about what the correct autofix should be. I'll make that change.

---

_Comment by @AlexWaygood on 2024-07-31 11:15_

I found another bug! If you have `import submodule.a` in an `__init__.py` file that _doesn't_ have an `__all__` definition, our autofix currently produces invalid syntax:

```
__init__.py:1:8: F401 [*] `submodule.a` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
  |
1 | import submodule.a
  |        ^^^^^^^^^^^ F401
  |
  = help: Use an explicit re-export: `submodule.a as submodule.a`

ℹ Safe fix
1   |-import submodule.a
  1 |+import submodule.a as submodule.a
```

---

_Comment by @AlexWaygood on 2024-07-31 12:28_

I pushed some updates. They required a bit of refactoring as the code for this rule is quite complex.

To summarise, if you have an unused first-party submodule import (e.g. `import submodule.a`) in an `__init__.py` file:
- Without a preexisting `__all__` definition:
  - We used to produce invalid syntax in our autofix: `import submodule.a as submodule.a`.
  - We now offer no autofix
- With a preexisting `__all__` definition:
  - We used to enter an infinite loop when attempting to provide an autofix
  - We now don't enter an infinite loop, and add `"submodule"` to `__all__` in our autofix

The error message has also changed slightly if you have an unused import in an `__init__.py` file that isn't a first-party import. This was a side effect of the refactoring, but I think it's for the better. For `import sys` in an `__init__.py` file:
- The error message used to be:
  > `sys` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
- It is now:
  > `sys` imported but unused

I think that's an improvement. If it's not a first-party import, there's very little chance that the user wants it to be reexported, either by adding it to `__all__` or by using a redundant alias, so the more concise error message feels like an improvement to me.

---

_Renamed from "[`pyflakes`] Fix preview-mode infinite loop in `F401` when attempting to autofix unused first-party submodule import in an `__init__.py` file that has an `__all__` definition" to "[`pyflakes`] Fix preview-mode bugs in `F401` when attempting to autofix unused first-party submodule imports in an `__init__.py` file" by @AlexWaygood on 2024-07-31 12:32_

---

_Merged by @AlexWaygood on 2024-07-31 12:34_

---

_Closed by @AlexWaygood on 2024-07-31 12:34_

---

_Branch deleted on 2024-07-31 12:34_

---
