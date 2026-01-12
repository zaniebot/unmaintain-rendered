```yaml
number: 2366
title: Add option to limit auto-import completions to direct dependencies
type: issue
state: open
author: BurntSushi
labels:
  - server
  - completions
assignees: []
created_at: 2026-01-06T14:47:54Z
updated_at: 2026-01-11T06:17:11Z
url: https://github.com/astral-sh/ty/issues/2366
synced_at: 2026-01-11T06:53:31Z
```

# Add option to limit auto-import completions to direct dependencies

---

_Issue opened by @BurntSushi on 2026-01-06 14:47_

This idea was offered here: https://github.com/astral-sh/ty/issues/2254#issuecomment-3703049537

It strikes me as a decent option to have. I don't know what's conventional in the Python ecosystem, but "I only want to import things from dependencies I've directly added and not from transitive dependencies that I don't directly depend on" seems reasonable.

This could be viewed as both a UX improvement (you're offered fewer completions that you don't care about) and as a perf improvement (we don't need to consider exported items from non-direct dependencies).

---

_Added to milestone `Stable` by @BurntSushi on 2026-01-06 14:47_

---

_Label `server` added by @BurntSushi on 2026-01-06 14:47_

---

_Label `completions` added by @BurntSushi on 2026-01-06 14:47_

---

_Comment by @MichaReiser on 2026-01-06 14:49_

Would this require us to parse `pyproject.toml` / requirements.txt files (I don't mind it, just curious)

---

_Comment by @MichaReiser on 2026-01-06 14:52_

An alternative is to at least consider whether a symbol is from a direct dependency when ranking the completions.

---

_Comment by @BurntSushi on 2026-01-06 14:53_

> Would this require us to parse `pyproject.toml` / requirements.txt files (I don't mind it, just curious)

I believe so, yes. Unless there's something in a virtual environment that I'm unaware of that distinguishes between direct vs non-direct dependencies.

> An alternative is to at least consider whether a symbol is from a direct dependency when ranking the completions.

This would get the UX improvement but not the perf improvement. The perf improvement might be the best way to tackle something like #2254 without asking users to disable auto-import entirely.

---

_Assigned to @Gankra by @Gankra on 2026-01-06 16:53_

---

_Comment by @BurntSushi on 2026-01-06 16:57_

Speaking with @zanieb, they suggested (and I agree) that enabling this option by default when there is a `pyproject.toml` might be desirable.

---

_Comment by @Gankra on 2026-01-06 17:04_

This would have a bad interaction with metapackages, right? (packages which only exist to depend on a cluster of packages so you can depend on them all with one dependency).

---

_Comment by @BurntSushi on 2026-01-06 17:06_

Yeah. I think that's at least part of the reason why this should be configurable.

---

_Comment by @AlexWaygood on 2026-01-06 17:14_

> Unless there's something in a virtual environment that I'm unaware of that distinguishes between direct vs non-direct dependencies.

nope!

---

_Comment by @AlexWaygood on 2026-01-06 17:15_

> This would have a bad interaction with metapackages, right? (packages which only exist to depend on a cluster of packages so you can depend on them all with one dependency).

Are those common? I don't think those are particularly idiomatic in the Python ecosystem. (But to be clear, I'm not objecting to having a mechanism to opt out of this behaviour, that seems fine. Though I agree that it should probably be the default to exclude non-direct dependencies; that seems good!)

---

_Comment by @toppk on 2026-01-11 06:17_

Sorry to add more suggestions here (especially since I've only started to test ty, and havent' actually switch my LSP to it just yet), but I'm wondering if this direct dependency will ignore python default packages, for example typing.  Also, if the user has an existing from randompackage import Foo, would auto-import of Bar look inside randompackage even if it was not a direct dependency?    

---
