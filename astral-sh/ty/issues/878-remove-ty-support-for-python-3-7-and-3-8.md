```yaml
number: 878
title: Remove ty support for Python 3.7 (and 3.8?)
type: issue
state: open
author: MatthewMckee4
labels: []
assignees: []
created_at: 2025-07-23T21:53:29Z
updated_at: 2025-11-14T09:09:22Z
url: https://github.com/astral-sh/ty/issues/878
synced_at: 2026-01-12T15:54:24Z
```

# Remove ty support for Python 3.7 (and 3.8?)

---

_@MatthewMckee4_

In the [docs](https://docs.astral.sh/ty/reference/cli/#ty-check--python-version), it is said that you can supply `--python-version 3.7` but you cant pip install with python 3.7.

What is the intended outcome, can we support 3.7?

---

_Comment by @zanieb on 2025-07-23 21:58_

I don't think having a wheel of ty for 3.7 is needed for it to type check 3.7 code?

---

_Comment by @MatthewMckee4 on 2025-07-23 21:59_

True, but ideally we can pip install in a codebase that runs 3.7 no?

---

_Comment by @zanieb on 2025-07-23 22:04_

Per your deleted comment, and to clarify, we build universal wheels so it is just the `requires-python` metadata that prevents this.

---

_Comment by @MatthewMckee4 on 2025-07-23 22:06_

ah okay, understood, thank you.

Would I be able to make a PR for this?

---

_Comment by @zanieb on 2025-07-23 22:46_

I'm not sure. I still don't really see the case for it, we'd need consensus from other stakeholders.

---

_Comment by @carljm on 2025-07-23 23:43_

Python 3.7 has been EOL for over two years now, and it's been a year and a half already since typeshed [removed all 3.7-specific type information from typeshed](https://github.com/python/typeshed/pull/11238). Typeshed [doesn't even support 3.8 anymore](https://github.com/python/typeshed/pull/13776) since earlier this spring; 3.8 has been EOL since last October.

Given the typeshed situation, I think any type checker's claim to support 3.7 or 3.8 is pretty empty at this point (unless they vendor an old version of typeshed specifically for that purpose). The results in practice won't be much different from just type-checking your project as if it were 3.9, since typeshed doesn't know anything about pre-3.9 versions.

So if we are to take any action here, I think it should be to clarify that we don't support 3.7. And I think we should seriously consider dropping support for 3.8 as well.

---

_Renamed from "Do we support 3.7 or not?" to "Remove ty support for Python 3.7 (and 3.8?)" by @carljm on 2025-07-23 23:45_

---

_Comment by @charliermarsh on 2025-07-23 23:45_

In uv we have `requires-python: >=3.8`, and we don't formally support Python 3.7.

---

_Comment by @MatthewMckee4 on 2025-07-24 06:50_

Thanks for the input and clarity. That all makes sense, and yeah it would be good to sync those 2 up so they agree with each other

---

_Comment by @carljm on 2025-07-24 19:20_

I would happily approve a PR to remove 3.7 support from ty. I think the main question mark here is whether Ruff wants to continue supporting it, since that might impact the details of how that PR should look. (I.e. removing 3.7 from the shared `PythonVersion` enum altogether, vs leaving it in place and just having ty validate 3.8+ and avoid mentioning 3.7 in docs or help text.) Maybe @MichaReiser or @AlexWaygood have thoughts on the Ruff version support.

---

_Comment by @MichaReiser on 2025-07-24 20:04_

I'd prefer to keep 3.7 support in Ruff. It is very low maintenance on the Ruff side and removing support would at least require a minor release

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 09:09_

---
