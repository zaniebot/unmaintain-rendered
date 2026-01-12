```yaml
number: 8679
title: Automatically add imports?
type: issue
state: open
author: Tronic
labels:
  - wish
assignees: []
created_at: 2023-11-14T17:05:04Z
updated_at: 2024-01-24T12:02:02Z
url: https://github.com/astral-sh/ruff/issues/8679
synced_at: 2026-01-12T15:54:48Z
```

# Automatically add imports?

---

_@Tronic_

Since Ruff helpfully removes imports, would it be completely blasphemous if it could also add them back at times? Often some imported symbols are not needed for a while, e.g. when cut/pasteing code around and casually hitting Ctrl-S to save and losing the now unneeded imports. When the code was restored, the imports should also (at least in some cases) come back.

Possible approaches include
(1) keeping track of recently removed imports as candidates on what to add back soon, or
(2) generally trying to guess which import is needed

Option 1 needs some state storage across runs, and has limited use.

Option 2 has been attempted by a number of tools but to my experience all they fail at it miserably, adding many spurious imports because of typos or incomplete names while typing in code, or just guess the wrong module. Not even `<stdlib module>.symbol` resolve to needing `import <stdlib module>` with any of the tools.

My hope is for Ruff to be in general so much better aware of how the code works than any of the other tooling that it could actually solve the auto imports correctly depending on how the missing symbol is being used, and what are common import conventions (thinking of things like `import numpy as np` and resolving `np.<something of numpy>` as needing that).


---

_Comment by @eli-schwartz on 2023-11-22 23:38_

For option 2: I think different projects will have a different idea of what "fail at it miserably" means, because it's fundamentally guesswork and can be guessed incorrectly.

> e.g. when cut/pasteing code around and casually hitting Ctrl-S to save and losing the now unneeded imports

In at least this case it sounds like it might not be a good idea to run autofixes on save. At least, when saving but not exiting. Personally I would suggest doing autofixes as a git-pre-commit hook instead, if possible.

---

_Comment by @Tronic on 2023-11-24 10:58_

Autoformat on save is very helpful, avoids a lot of manual formatting before ever getting to commits.

By failing miserably I mean they very rarely are able to add the correct import but that completely spurious and unrelated symbols of some module not even used by the project get imported. In other words, these tools do more harm than good. Surely it is always guesswork, but with a proper implementation the imports could be right the majority of time - a few mistakes would be tolerable if that were the case.

Needing imports is in my opinion the major usability problem with current programming languages. Ideally I would see anything listed as dependency in pyproject.toml (package.json etc) be imported as needed (by programming languages themselves). The usability problem comes primarily from that the imports need to be at top of file while the code being edited is somewhere deep down. Thus one needs to keep jumping back & forth to update the imports, losing the actual context one was working with.


---

_Comment by @eli-schwartz on 2023-11-24 14:36_

> The usability problem comes primarily from that the imports need to be at top of file while the code being edited is somewhere deep down. Thus one needs to keep jumping back & forth to update the imports, losing the actual context one was working with.

But this is not true. You can import anywhere you want in the file.

Many style guides recommend against doing this but that does not mean the language cares. It is also a critically required functionality for conditional imports, such as rarely used functions that require heavy dependencies, but only if they actually get used. Also, breaking import cycles..

---

_Comment by @Tronic on 2023-11-25 12:01_

Of course, or even import inside the function. But Ruff itself complains about, and won't move it to top, and and we'd like to stick to good style.

---

_Label `wish` added by @MichaReiser on 2023-11-27 02:36_

---

_Comment by @MichaReiser on 2023-11-27 02:39_

Would you expect this to be a editor or CLI feature? I haven't written a lot of Python myself but I know that VS Code and many other editors automatically import symbols when they're first used. I could see Ruff's editor integration supporting this eventually. 

---

_Comment by @Tronic on 2023-11-29 23:51_

A bit of both I suppose. I'm on VS Code and happy with the Ruff extension.

---

_Comment by @fightingdreamer on 2024-01-24 12:02_

Auto import can be implemented to some degree with minimal effort by scanning other files in the project and collecting (ruff already uses AST) already provided imports, assuming that these are correct that gives `import` from `symbol` directed graph.

From that point it should be quite simple to implemented lookup logic.

Pros:
- probably 99% correct most of the time
- can be extended to propose other `symbol` from already loaded modules / imports

Cons:
- not work in empty project

Notes:
- Probably is better to keep that list in memory to omit storage complexity (ruff-lsp can be a good candidate).
- [PyFlyby](https://deshaw.github.io/pyflyby/) uses similar approach, but requires manually defining list of known imports.


---
