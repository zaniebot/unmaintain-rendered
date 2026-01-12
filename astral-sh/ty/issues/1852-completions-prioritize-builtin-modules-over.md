```yaml
number: 1852
title: "completions: Prioritize builtin modules over symbols from third-party packages"
type: issue
state: closed
author: zsol
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-11T10:39:06Z
updated_at: 2026-01-09T15:11:39Z
url: https://github.com/astral-sh/ty/issues/1852
synced_at: 2026-01-12T15:54:25Z
```

# completions: Prioritize builtin modules over symbols from third-party packages

---

_@zsol_

I'd like to suggest a new heuristic: consider prioritizing builtin modules over symbols defined with the same name in the environment.

For example in [this trivial example](https://play.ty.dev/fbc4ba96-0d33-4c7c-a120-39f4cf88f626) I'd like the `os` reference in `main.py` to complete first to the standard lib `os` module.

The situation that prompted this is the `coverage` library where [this](https://github.com/coveragepy/coveragepy/blob/bedb949d20cb16ca98415a6fc018accbeae3e763/coverage/files.py#L22) is a common pattern; in my project that happens to have a dev-dependency on `coverage`, this happens:

<img width="434" height="238" alt="Image" src="https://github.com/user-attachments/assets/949943b5-3ac2-4676-8a7f-54621becb7bf" />

Which is suboptimal.

---

_Label `server` added by @zsol on 2025-12-11 10:39_

---

_Label `completions` added by @zsol on 2025-12-11 10:39_

---

_Renamed from "completions: Prioritize builtin modules over symbols in the environment" to "completions: Prioritize builtin modules over symbols from third-party packages" by @AlexWaygood on 2025-12-11 10:43_

---

_Comment by @BurntSushi on 2025-12-11 13:39_

> For example in [this trivial example](https://play.ty.dev/fbc4ba96-0d33-4c7c-a120-39f4cf88f626) I'd like the `os` reference in `main.py` to complete first to the standard lib `os` module.

So in this example specifically, I think I'd question whether we want to prioritize the stdlib module. Or at least, I'm having trouble coming up with a heuristic. In particular, I think I would generally expect symbols that are "more local" to show up before symbols that are "less local." i.e., All else being equal, I see symbols in my project ranked higher than symbols from a dependency or the standard library.

What about a heuristic that ranks the names of modules over names within modules? I think that would fix your two examples without always prioritizing std modules over non-std modules. I don't know how well it generalizes though.

---

_Comment by @AlexWaygood on 2025-12-11 13:45_

Oh, I retitled this issue based on the `coverage` example given in the OP, but the playground example is quite different, as @BurntSushi says. These seem like pretty distinct cases, though it's of course possible that they'll share the same solution

---

_Comment by @zsol on 2025-12-11 13:48_

Yeah that's fair, I'm not sure how to represent a faithful example of the situation I care about (where the conflicting symbol is coming from a module I have no control over).

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 15:42_

---

_Assigned to @Gankra by @Gankra on 2026-01-09 03:49_

---

_Unassigned @Gankra by @BurntSushi on 2026-01-09 15:07_

---

_Assigned to @BurntSushi by @BurntSushi on 2026-01-09 15:07_

---

_Closed by @BurntSushi on 2026-01-09 15:11_

---
