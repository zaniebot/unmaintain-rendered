```yaml
number: 22472
title: "[pyflakes] Fix F823 false positive with submodule import shadowing"
type: pull_request
state: closed
author: bxff
labels:
  - bug
  - rule
assignees: []
base: main
head: fix-22467
created_at: 2026-01-09T04:57:09Z
updated_at: 2026-01-12T22:16:41Z
url: https://github.com/astral-sh/ruff/pull/22472
synced_at: 2026-01-12T22:24:50Z
```

# [pyflakes] Fix F823 false positive with submodule import shadowing

---

_@bxff_

## Summary

The undefined_local rule (F823) incorrectly flagged an error for the common pattern where a submodule import shadows a global module import of the same root:

```python
import pwndbg
def myfunc() -> None:
    if pwndbg.dbg.is_gdblib_available():
        import pwndbg.gdblib.functions
```

While Python technically raises an UnboundLocalError in this case, Pyflakes exempts submodule imports that extend an existing import of the same root module. Ruff now follows this behavior.

The fix exempts F823 errors when:
- The local binding is a SubmoduleImport
- The shadowed binding is an import of the same root module

Plain imports shadowing other plain imports (e.g., `import sys` inside a function shadowing a global `import sys`) are still correctly flagged.

## Test Plan

* Added regression test case to `crates/ruff_linter/resources/test/fixtures/pyflakes/F823.py`
* Ran all 454 pyflakes tests - all passed
* Verified the specific exemption works correctly while preserving existing behavior

Fixes https://github.com/astral-sh/ruff/issues/22467

---

_Renamed from "[pyflakes] Fix F823 false positive with submodule import shadowing (#22467)" to "[pyflakes] Fix F823 false positive with submodule import shadowing" by @bxff on 2026-01-09 04:59_

---

_Label `bug` added by @amyreese on 2026-01-09 20:20_

---

_Label `rule` added by @amyreese on 2026-01-09 20:20_

---

_Comment by @astral-sh-bot[bot] on 2026-01-09 20:33_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Comment by @dscorbett on 2026-01-10 13:59_

This seems like a bug in the upstream rule. Given that this always raises `UnboundLocalError`, why shouldn’t the `undefined-local` rule detect it?

---

_Comment by @ntBre on 2026-01-12 22:16_

I'll go ahead and close this for now since Ruff is correctly flagging a runtime error. Thank you for working on this and checking the behavior of the upstream linter!

As I commented on the issue, I think this could be a good use for a [`secondary_annotation`](https://github.com/astral-sh/ruff/blob/e4ba29392bb768965ba16ba86aba7d482e4f2ea8/crates/ruff_linter/src/checkers/ast/mod.rs#L3560) on the diagnostic pointing to the local assignment that shadows the global binding. I'm picturing something like this:

```
F823 Local variable `pwndbg` referenced before assignment
 --> repro.py:7:8
  |
6 | def myfunc() -> None:
7 |     if pwndbg.dbg.is_gdblib_available():
  |        ^^^^^^
8 |         import pwndbg.gdblib.functions
  |                ------ This binding makes `pwndbg` a local variable
9 |         _ = pwndbg.gdblib.functions.functions
  |
```

We could even include a `help` line via the fix title that says something like ``consider declaring `pwndbg` as `global` or `nonlocal` ``, in line with the example in the documentation.

Just mentioning in case you'd be interested in working on that instead :)

---

_Closed by @ntBre on 2026-01-12 22:16_

---
