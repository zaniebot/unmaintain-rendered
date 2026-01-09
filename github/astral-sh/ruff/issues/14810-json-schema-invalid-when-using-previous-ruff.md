---
number: 14810
title: JSON Schema invalid when using previous ruff version
type: issue
state: open
author: DaniBodor
labels:
  - configuration
assignees: []
created_at: 2024-12-06T09:58:10Z
updated_at: 2025-08-11T13:05:08Z
url: https://github.com/astral-sh/ruff/issues/14810
synced_at: 2026-01-07T13:12:16-06:00
---

# JSON Schema invalid when using previous ruff version

---

_Issue opened by @DaniBodor on 2024-12-06 09:58_

Hi,

I am using a previous version of ruff (v0.7.4) in a given project. Recently, my TOML formatting extension in VS (Even Better TOML) code has been showing an error on my ruff settings for this project. The error message appears under the `[tool.ruff.lint]` line in pyproject.toml and show all the settings together as being the problem (see below).
After digging into it a bit, it turns out that the specific reason for the error is that I ignore ANN101 and ANN102 (which have been removed in v0.8) and list TCH (which has been renamed to TC in v0.8) as a safe fix. I discovered this by removing the `.lint` part from the line (resulting in just `[tools.ruff]`, at which point the errors appear at those specific lines. However, running ruff like this results in a deprecation warning, telling me that those settings should not be top level ruff settings any more.

So few issues here:
1. When using v7.X, these should not be considered errors to start with. Removing them from the ignore/safe-fix lists leads to unwanted behavior.
2. The errors should be displayed at the specific rules where they exist rather than in the header.
    - This is already the case when using `ignore` in top level ruff settings (despite it being deprecated). This makes me think it should be possible when listed under the lint-specific rules as well.
3. The error message is not useful here. I guess that if 2 gets resolved, then 3 will automatically get resolved as well, but want to flag it as a specific issues as well.

NB: These issues seem reminiscent of #11880



![Image](https://github.com/user-attachments/assets/58816a7b-08f8-4090-84f5-a054464adb42)


> {"select":["ALL"],"pydocstyle":{"convention":"google"},"ignore":["FBT","ANN002","ANN003","ANN101","ANN102","ANN204","S105","S311","PT011","PD011","TD","FIX002","B028","D100","D104","D105","D107","COM812","ISC001"],"fixable":["ALL"],"unfixable":["F401","RUF100","B905"],"extend-safe-fixes":["D415","D300","D200","TCH","ISC001","EM","RUF013","B006","TID252"],"pylint":{"max-args":9},"flake8-tidy-imports":{"ban-relative-imports":"all"},"isort":{"known-first-party":["eitprocessing"]},"per-file-ignores":{"*.ipynb":["ERA001","T203","T201"],"tests/*":["S101","ANN201","D103","PLR2004","SLF001"],"docs/*":["ALL"]}} is not valid under any of the given schemasEven Better TOML

---

_Comment by @MichaReiser on 2024-12-06 10:02_

That makes sense. 

I think the fix here is that Ruff's VS code extension [contributes the json schema](https://code.visualstudio.com/api/references/contribution-points#contributes.jsonValidation). But I'm not quite sure how we'd serve it from the LSP itself

---

_Label `configuration` added by @MichaReiser on 2024-12-06 10:02_

---

_Comment by @dmyersturnbull on 2025-04-11 13:46_

#16763 confused some extremely confusing behavior.

<b>New violation:</b>

```python
# File: src/pkg/core.py
from os import PathLike  # Violation: TC003: Move [...] into a type-checking block
```

<b>Doesn't suppress:</b>

```toml
# File: pyproject.toml
[tool.ruff.lint]
ignore = ["TC003"]
```

<b>Does suppress:</b>

```python
# File: src/pkg/core.py
# ruff: noqa: TC003
from os import PathLike
```


---

_Comment by @MichaReiser on 2025-04-11 13:51_

> [#16763](https://github.com/astral-sh/ruff/issues/16763) confused some extremely confusing behavior.
> 
> New violation:
> 
> # File: src/pkg/core.py
> from os import PathLike  # Violation: TC003: Move [...] into a type-checking block
> 
> Doesn't suppress:
> 
> # File: pyproject.toml
> [tool.ruff.lint]
> ignore = ["TC003"]
> 
> Does suppress:
> 
> # File: src/pkg/core.py
> # ruff: noqa: TC003
> from os import PathLike

I'm not sure I see how this is related to this issue. Can you create a new issue and share some more details (e.g. what ruff version are you using)

---

_Referenced in [astral-sh/ruff#17356](../../astral-sh/ruff/issues/17356.md) on 2025-04-11 16:20_

---

_Comment by @ssbarnea on 2025-08-11 11:43_

Took me a lot of time to find this one. The question is why ruff does not just fail running when it finds `TCH` inside the skip, so we can update the config. Is not the first time when an identifier was removed or renamed and it would be much better than now, where ruff silently seems to be working but our editors complain about invalid schema. 

---

_Comment by @MichaReiser on 2025-08-11 13:05_

I think `TCH` could be fixed by keeping them in the schema but marking the entries as deprecated. 

---
