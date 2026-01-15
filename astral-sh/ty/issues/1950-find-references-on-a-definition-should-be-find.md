```yaml
number: 1950
title: find-references on a definition should be find-uses (not include self)
type: issue
state: open
author: Gankra
labels:
  - bug
  - server
assignees: []
created_at: 2025-12-16T18:27:53Z
updated_at: 2026-01-15T08:29:54Z
url: https://github.com/astral-sh/ty/issues/1950
synced_at: 2026-01-15T08:47:00Z
```

# find-references on a definition should be find-uses (not include self)

---

_@Gankra_

### Summary

```
x = 1
print(x)
```

If you find-references on the second x it goes to the first (good!)
If you find-references on the first x it shows both itself *and* the second (bad, not helpful, should only be the second).

### Version

_No response_

---

_Label `bug` added by @Gankra on 2025-12-16 18:27_

---

_Label `server` added by @Gankra on 2025-12-16 18:27_

---

_Comment by @Gankra on 2025-12-16 18:32_

Oh wild we actually support this distinction but it's something the client tells us which it wants?

https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#referenceContext

---

_Comment by @konstin on 2025-12-16 18:32_

This problem affects PyCharm, where the default find usages shortcut doesn't work, but "find usages" from the menu does, which then shows two options, the current line and column as well sa the real find usages dialog the user wants.

---

_Comment by @MichaReiser on 2025-12-16 18:45_

>  If you find-references on the first x it shows both itself and the second (bad, not helpful, should only be the second).

This is also what pylance and rust analyzer does. So I'm not sure if this is indeed entirely undesired.

---

_Comment by @MichaReiser on 2025-12-16 18:46_

Same if you do find references on a function, r-a returns the function as well as all uses.

---

_Comment by @Gankra on 2025-12-16 19:24_

Hmm is cmd+click a different API than "find-references"? Or maybe it's one of those "they query a bunch of different ones with fallback" situtations?

---

_Comment by @GuilhermePSC on 2026-01-14 09:42_

I'm currently exploring moving from pyright to ty, and it looks great! Thank you for all the efforts you put into this.
I came across this issue because we also use PyCharm and the absence of a quick shortcut to "Go to usages" is a bit annoying. It's currently blocking adoption for me (maybe together with the [absence of full support to Pydantic](https://github.com/astral-sh/ty/issues/2403)).

Do you have a timeline to solve this issue in https://github.com/astral-sh/ruff/pull/22012?


---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2026-01-15 08:29_

---
