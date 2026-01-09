---
number: 14470
title: "TCH: Remove requirement for the name `TYPE_CHECKING` to be from `typing[_extensions]`?"
type: issue
state: closed
author: bzoracler
labels:
  - question
assignees: []
created_at: 2024-11-20T00:19:05Z
updated_at: 2024-11-20T08:02:08Z
url: https://github.com/astral-sh/ruff/issues/14470
synced_at: 2026-01-07T13:12:16-06:00
---

# TCH: Remove requirement for the name `TYPE_CHECKING` to be from `typing[_extensions]`?

---

_Issue opened by @bzoracler on 2024-11-20 00:19_

Although it is canonical to import `TYPE_CHECKING` from `typing` or `typing_extensions`, type-checkers do not seem to actually require this (see playground demos: [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=53c467ed82f6ec030c4c65b5e6cc189a), [pyre](https://pyre-check.org/play?input=TYPE_CHECKING%3A%20bool%20%3D%20bool()%0A%0Aif%20not%20TYPE_CHECKING%3A%0A%20%20%20%20a%3A%20int%20%3D%20%22%22%0Aelse%3A%0A%20%20%20%20b%3A%20str%20%3D%200), [pyright](https://pyright-play.net/?pythonVersion=3.13&strict=true&reportMissingModuleSource=true&enableExperimentalFeatures=true&code=CoTQCgog%2BgwgEhGBpAkgOQOIC4AEAjAewIBscBefI4gCgEoAoegSwDMcA7AgFx1ElgTJ02ejjE4AhribseFAETz6AU2IBnZVlHi8uNVwBO5HAAYgA)).

It would be great if this requirement is also removed in ruff. Currently, `TCH*` violations aren't reported if `TYPE_CHECKING` isn't imported from `typing[_extensions]` (see [ruff playground](https://play.ruff.rs/c8e6e3b4-a13c-4bf4-a755-eb42926a5f16)).

Use-case: I'd like to be able to define my own extended typing module with extra symbols like so:
```python
# my_typing_module.py

from typing_extensions import *

...
```
```python
import my_typing_module

if my_typing_module.TYPE_CHECKING:
  from pathlib import Path  # Expected ruff: TCH004, but got no warning

p: Path = Path()
```

---

_Label `question` added by @MichaReiser on 2024-11-20 07:13_

---

_Comment by @MichaReiser on 2024-11-20 07:14_

Hi @bzoracler 

Have you tried adding your module to the [`typing-modules`](https://docs.astral.sh/ruff/settings/#lint_typing-modules) list? Ruff should then recognize it and treat it the same as a regular `typing` module.

---

_Comment by @bzoracler on 2024-11-20 08:02_

Oh, thanks for that - that'll be an acceptable workaround.

---

_Closed by @bzoracler on 2024-11-20 08:02_

---
