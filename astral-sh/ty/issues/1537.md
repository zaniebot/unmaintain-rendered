```yaml
number: 1537
title: improve UX around stubs packages
type: issue
state: open
author: carljm
labels: []
assignees: []
created_at: 2025-11-13T00:26:16Z
updated_at: 2025-12-18T00:50:47Z
url: https://github.com/astral-sh/ty/issues/1537
synced_at: 2026-01-10T01:53:59Z
```

# improve UX around stubs packages

---

_Issue opened by @carljm on 2025-11-13 00:26_

For example, if we see that you are type-checking a project that depends on a package with a stubs package, but you don't have that stubs package installed, we could suggest that you install it.

---

_Added to milestone `Stable` by @carljm on 2025-11-13 00:26_

---

_Assigned to @Gankra by @Gankra on 2025-11-13 17:33_

---

_Comment by @AlexWaygood on 2025-11-13 23:52_

Is this a duplicate of https://github.com/astral-sh/ty/issues/487?

---

_Comment by @carljm on 2025-11-14 00:18_

That one seems more narrowly focused around one particular UX improvement -- detecting compiled modules and giving a better error than "module not found." But I see this issue as broader -- what about pure Python packages that just don't have inline types, and have a stubs package available? (Maybe that's not a case that exists very much in practice?)

#487 has more details, so if you are confident that it describes the full scope of the actual feature we want here, feel free to close this as duplicate. Otherwise I'd be inclined to mark #487 as a sub-issue of this one?

---

_Comment by @MichaReiser on 2025-11-14 08:42_

Related https://github.com/astral-sh/ty/issues/1104

---

_Comment by @carljm on 2025-11-19 20:19_

https://github.com/astral-sh/ty/issues/1591 is an example case of where we could do better here even for pure Python packages. Pandas has type annotations inline (presumably for internal type checking?), but there's also the `pandas-stubs` package which provides public type annotations for users of pandas. Since mypy doesn't respect inline types for non `py.typed` packages, users will just get no type-checking unless they install `pandas-stubs`. But for us and pyright, we will attempt to use the inline types and sometimes emit diagnostics that should be fixed by installing `pandas-stubs`. Ideally we could inform users if they are using `pandas` without `pandas-stubs`, that they probably want to install `pandas-stubs`.

---

_Comment by @Gankra on 2025-12-18 00:50_

Note to self: you assigned this to yourself because this is a potential "better uv integration" feature

---
