```yaml
number: 585
title: Improve diagnostics (and documentation) for unresolved imports caused by editable installs that use setuptools as their build backend
type: issue
state: open
author: SoftwareApe
labels:
  - imports
assignees: []
created_at: 2025-06-05T12:25:29Z
updated_at: 2025-11-13T15:31:54Z
url: https://github.com/astral-sh/ty/issues/585
synced_at: 2026-01-10T02:06:24Z
```

# Improve diagnostics (and documentation) for unresolved imports caused by editable installs that use setuptools as their build backend

---

_Issue opened by @SoftwareApe on 2025-06-05 12:25_

### Summary

Hi,

I hope this isn't a duplicate issue. I scanned through https://github.com/astral-sh/ty/issues/445 and the current open issues but didn't find it.

I have some development submodules added through `uv add --editable ...`. These are found in the `tool.uv.sources` section of the `pyproject.toml`.

They seem to work fine with uv and ruff. But `ty` complains that:

```
error[unresolved-import]: Cannot resolve imported module ...
```

PS: Amazing work from the astral team as always. This alpha version is already better than many mature python type-checkers!

### Version

`ty==0.0.1a8`


---

_Comment by @AlexWaygood on 2025-06-05 12:32_

This is probably a duplicate of https://github.com/astral-sh/ty/issues/475 if your development submodules are using `setuptools` for their build backend. But we've decided internally that we want to prioritise better diagnostics for this situation, so I'll keep this issue open to track that 👍

Thanks for the report!

---

_Label `imports` added by @AlexWaygood on 2025-06-05 12:32_

---

_Comment by @SoftwareApe on 2025-06-05 14:42_

@AlexWaygood thank you so much! It indeed seems like #475 it's the same issue. I guess I didn't search for the right combination of strings. It's indeed a setuptools dependency.

---

_Renamed from "error[unresolved-import]: Cannot resolve imported module for editable sources from tool.uv.sources are not found by ty." to "Improve diagnostics for unresolved imports caused by local packages that use setuptools as their build backend" by @AlexWaygood on 2025-06-05 16:21_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-06-05 16:22_

---

_Comment by @carljm on 2025-11-13 06:46_

I've reopened #475 to see if there's any way we can support these -- but if we can't, we should still improve the diagnostics.

---

_Added to milestone `Stable` by @carljm on 2025-11-13 06:47_

---

_Comment by @MichaReiser on 2025-11-13 07:43_

A first step (and very easy) could be to add some debug or info level logging when ty skips a `pth` file.

https://github.com/astral-sh/ruff/blob/64edfb6ef6f26bb3618c5c888f5abf3a866a172c/crates/ty_python_semantic/src/module_resolver/resolver.rs#L600-L615


---

_Comment by @MichaReiser on 2025-11-13 15:29_

Some notes from our internal discord on how we could detect packages that use setuptools

> Would you go and try to find a matching .pth file with wild python code in it? 
> and there are other things that setuptools inserts into site-packages that you can look for to know for sure that the package is installed, we just have no idea where the package actually is located
> and those things will also tell us that it was setuptools that installed it

---

_Renamed from "Improve diagnostics for unresolved imports caused by local packages that use setuptools as their build backend" to "Improve diagnostics (and documentation) for unresolved imports caused by editable installs that use setuptools as their build backend" by @carljm on 2025-11-13 15:29_

---

_Comment by @carljm on 2025-11-13 15:31_

Some useful links for future reference: https://github.com/pypa/setuptools/issues/3518 and https://docs.basedpyright.com/v1.25.0/usage/import-resolution/#setuptools

---
