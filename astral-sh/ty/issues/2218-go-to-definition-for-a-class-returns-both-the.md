```yaml
number: 2218
title: "Go-to-definition for a class returns both the `__init__` definition and the class itself"
type: issue
state: open
author: lkhphuc
labels:
  - server
assignees: []
created_at: 2025-12-25T06:59:57Z
updated_at: 2026-01-10T08:51:56Z
url: https://github.com/astral-sh/ty/issues/2218
synced_at: 2026-01-12T15:54:26Z
```

# Go-to-definition for a class returns both the `__init__` definition and the class itself

---

_@lkhphuc_

### Summary

When using the "go to definition" (in neovim) for a class, currently ty return 2 "definitions":
- The class name line `class MyClass(...):`
- and the init method `def __init__(self,...):`

I'm not sure if this is by design or not, but other lsp (pyright) just return one definition `class MyClass` and thus the editor can jump directly to that definition.
Current it unnecessary open a picker to pick the definition even though there's just one implementaton of that class.


<img width="959" height="100" alt="Image" src="https://github.com/user-attachments/assets/661757f8-bf24-4b9f-bb3e-9bb97c8fa477" />


### Version

ty 0.0.5

---

_Label `server` added by @AlexWaygood on 2025-12-25 08:39_

---

_Renamed from "Duplicate definition of a class" to "Go-to-definition for a class returns both the `__init__` definition and the class itself" by @MichaReiser on 2025-12-25 14:53_

---

_Comment by @MichaReiser on 2025-12-25 14:56_

Interesting, I'm unable to reproduce this but maybe it's because I'm using VS Code or because my example is too simple? 

If I have:

```py
class Test:
    def __init__(self): ...


a = Test()
```

and click on `Test`, VS Code then jumps right to `class Test`. But I assume your case is different. Can you say me more on where you click?

---

_Comment by @ntBre on 2025-12-25 15:21_

I can partially reproduce this in Emacs. Using the `lsp-find-type-defintion` command with my cursor on the `Test()` call takes me straight to the `class Test` line, but `lsp-find-declaration` opens a buffer with links to both the class and the `__init__` method. Maybe neovim has a distinction between the two as well?

(I kind of like this behavior and actually have `lsp-find-declaration` on the more convenient keybinding)

---

_Comment by @lkhphuc on 2025-12-26 06:41_

I'm using [LazyVim](https://www.lazyvim.org/) to be precise, but the behaviour is the same for plain `vim.lsp.buf.definition()`.

In my case, both go to definition `vim.lsp.buf.definition()`, go to declaration `vim.lsp.buf.declaration()` return both class name and `__init__`.
ONly `vim.lsp.buf.type_definition()` bring me directly to the class name.

> Can you say me more on where you click?

```
class Test:
    def __init__(self): ...


a = Test()
    ^^^^ cursor is anywhere here.
```

---

_Comment by @MichaReiser on 2025-12-26 08:38_

It's somewhat intentional that clicking on the constructor returns a reference to both. I need to double-check why this wasn't working for me in VS Code. The idea is that you might be interested in both the type or the `__init__` method. This was added in https://github.com/astral-sh/ruff/pull/20014

An alternative would be to make `Test` go to the class and `(` go to the `__init__` method which we discussed on the PR but decided to wait on user feedback. So thanks for the feedback :) Let's keep this issue open to see if other people feel the same.

Clicking on `Test` anywhere else, e.g. in a type annotation, should jump directly to the class's definition

---

_Label `needs-decision` added by @MichaReiser on 2025-12-26 08:40_

---

_Comment by @lkhphuc on 2025-12-26 10:07_

Thanks. The bracket go to init thing sounds like a good UX to me.

---

_Comment by @MeGaGiGaGon on 2025-12-26 19:04_

I can reproduce this on VSC and in the playground using @MichaReiser's example by Ctrl-clicking on the `Test` in `Test()`, not sure what would be different (VSC extension version `2025.76.0` playground `9693375e1`)

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 15:45_

---

_Label `needs-decision` removed by @MichaReiser on 2026-01-10 08:51_

---

_Comment by @MichaReiser on 2026-01-10 08:51_

We're receiving more feedback that the current behavior is unintuitive. We should change the behavior so that clicking on `(` returns the constructor and clicking on `Test` returns the class

---

_Removed from milestone `Stable` by @MichaReiser on 2026-01-10 08:51_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2026-01-10 08:51_

---
