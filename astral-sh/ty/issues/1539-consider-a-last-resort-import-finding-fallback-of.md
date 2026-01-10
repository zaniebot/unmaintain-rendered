```yaml
number: 1539
title: "Consider a last-resort import-finding fallback of \"try all ancestor directories as candidate import roots\""
type: issue
state: closed
author: carljm
labels:
  - imports
assignees: []
created_at: 2025-11-13T15:34:16Z
updated_at: 2025-12-03T20:04:38Z
url: https://github.com/astral-sh/ty/issues/1539
synced_at: 2026-01-10T01:56:40Z
```

# Consider a last-resort import-finding fallback of "try all ancestor directories as candidate import roots"

---

_Issue opened by @carljm on 2025-11-13 15:34_

Pyright does this, see #6 in https://microsoft.github.io/pyright/#/import-resolution
Pyrefly also does something similar (though not clear if its a fallback for each failed import, or a configuration-time fallback): https://pyrefly.org/en/docs/import-resolution/#fallback-search-path

The benefit here is that it can make lots more scenarios work out of the box.

The con is that it's kind of an expensive way to make imports work throughout your project, and if it "fixes" the issue well enough, you may never know to properly config your import roots.

We'd probably want a config option to turn it off.

---

_Added to milestone `Stable` by @carljm on 2025-11-13 15:34_

---

_Label `imports` added by @carljm on 2025-11-13 15:34_

---

_Comment by @MichaReiser on 2025-11-13 15:35_

> We'd probably want a config option to turn it off.

This could also be its own rule where we emit a warning diagnostic for those imports by default that can be turned off.

---

_Comment by @AlexWaygood on 2025-11-13 23:54_

Is this a duplicate of #870? Or should we consider closing that issue in favour of this one?

---

_Comment by @carljm on 2025-11-14 00:14_

Oh, I think I missed that one because it was tagged `server` -- I guess it commonly helps in an LSP case, but I think if we implement this it shouldn't be LSP-specific. (The recent case that came up was someone running ty via CLI, not LSP.)

I don't really care which issue we keep, but I think this one has slightly more detail (and up to date links) in the description and discussion, so I'll close the other as duplicate.

---

_Label `needs-decision` added by @carljm on 2025-11-14 22:02_

---

_Assigned to @Gankra by @Gankra on 2025-11-28 14:04_

---

_Comment by @MichaReiser on 2025-11-28 14:08_

I'll bump the priority of this because we need it for pyx. 

The issue they're facing is that they have a single workspace with multiple members defining their own `conftest.py`. We can't work this around with `environment.root`, because ty would then always use the conftest of the first workspace member in `environment.root` rather than the one local to the workspace member. 

Implementing this might also allow us to remove `tests` from the paths we add by default to `environment.root` if left unspecified. Adding `tests` seems sort of a terrible idea in hindsight because it prevents ty from catching accidental imports of testing-only modules in production code.

For pyx, it would be sufficient to only consider directories that contain a `pyproject.toml` as an additional search path.

Other alternative solutions that we discussed to solve the issue for pyx are:

* Add an option to suppress specific unresolved imports. While that removes the diagnostics, it also breaks go-to-def. Which is why I think the solution proposed in this issue is preferred
* https://github.com/astral-sh/ruff/pull/21666 This is probably the ideal but it's a bit more work to get done and the solution discussed in this issue might also help with multiple `tests` directories

---

_Label `needs-decision` removed by @MichaReiser on 2025-11-28 14:10_

---

_Removed from milestone `Stable` by @MichaReiser on 2025-11-28 14:10_

---

_Added to milestone `Beta` by @MichaReiser on 2025-11-28 14:10_

---

_Comment by @Gankra on 2025-11-28 14:13_

> For pyx, it would be sufficient to only consider directories that contain a pyproject.toml as an additional search path.

This was my preferred option (easy to explain and understand) so I'll start with that.

---

_Closed by @Gankra on 2025-12-03 20:04_

---
