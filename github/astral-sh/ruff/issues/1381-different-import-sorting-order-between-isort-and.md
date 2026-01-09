---
number: 1381
title: Different import sorting order between isort and ruff if import is aliased.
type: issue
state: closed
author: ghuls
labels:
  - isort
assignees: []
created_at: 2022-12-26T10:30:22Z
updated_at: 2023-01-23T17:33:24Z
url: https://github.com/astral-sh/ruff/issues/1381
synced_at: 2026-01-07T13:12:14-06:00
---

# Different import sorting order between isort and ruff if import is aliased.

---

_Issue opened by @ghuls on 2022-12-26 10:30_

Differnt import sorting order between isort and ruff if import is aliased.

```python
# isort --profile=black input.py
$ cat /tmp/isorted_output.py
from numpy import cos, int8, int16, int32, int64
from numpy import sin as np_sin
from numpy import tan, uint8, uint16, uint32, uint64

$ ruff --select I001 --ignore F401 --fix input.py
$ cat /tmp/ruff_isorted_output.py
from numpy import cos, int8, int16, int32, int64, tan, uint8, uint16, uint32, uint64
from numpy import sin as np_sin
```

---

_Label `isort` added by @charliermarsh on 2022-12-26 12:12_

---

_Comment by @charliermarsh on 2022-12-26 14:45_

Good call, thank you.

---

_Renamed from "Differnt import sorting order between isort and ruff if import is aliased." to "Different import sorting order between isort and ruff if import is aliased." by @charliermarsh on 2022-12-26 15:08_

---

_Comment by @andersk on 2022-12-27 09:30_

Related: PyCQA/isort#1952

---

_Comment by @ghuls on 2022-12-27 11:48_

Personally I like the ruff behavior more than the isort behavior. So I hope isort changes their behavior.

---

_Referenced in [pola-rs/polars#5916](../../pola-rs/polars/pulls/5916.md) on 2022-12-28 07:52_

---

_Comment by @artpelling on 2023-01-04 14:28_

I get a completely different sorting when using `isort` and `ruff`. In my project, `isort file.py` gives me:
```python
from abc import abstractmethod
from contextlib import contextmanager

from numpy import concatenate, int32, zeros
from traits.api import Dict, Float, Str, cached_property

import tapy.core.config as config
from tapy.core.base import BasicObject
```

whereas `ruff --select I001 --ignore F401 --fix file.py` gives me
```python
from abc import abstractmethod
from contextlib import contextmanager

import tapy.core.config as config
from numpy import concatenate, int32, zeros
from tapy.core.base import BasicObject
from traits.api import Dict, Float, Str, cached_property
```

Any idea what is going on here? I don't think it is an issue with my config.

---

_Comment by @charliermarsh on 2023-01-04 14:43_

It looks like it’s not registering tapy as a first-party module. Would you mind posting your pyproject.toml and project structure?

---

_Comment by @artpelling on 2023-01-04 17:18_

> It looks like it’s not registering tapy as a first-party module. Would you mind posting your pyproject.toml and project structure?

Hi. Thanks for the swift reply! Sure thing, I have a `src` structure, so submodules are contained in `src/tapy/submodule` with `__init__.py` in `tapy` and `submodule` directories. The relevant part of my `pyproject.toml` is
```toml
[project]
name = "tapy"
...
requires-python = ">=3.9"
...
dependencies = [
    "tqdm",
    "traits"
]

[project.optional-dependencies]
dev = [
    "ipython",
    "pre-commit",
    "ruff",
    "sphinx",
    "sphinx-autoapi",
    "nbsphinx",
    "sphinx-rtd-theme",
    "sphinx-gallery"
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.ruff]
ignore = [
    "D100", "D101", "D102", "D103", "D104", "D105", "D107",
    "D203", "D213",
    "B905",
    "F401",
    "N806"]
line-length = 120
select = ["B", "D", "E", "F", "I", "N", "Q", "W"]

[tool.ruff.flake8-quotes]
inline-quotes = "single"

[tool.ruff.pydocstyle]
convention = "numpy"
```

---

_Comment by @charliermarsh on 2023-01-04 17:21_

Can you try adding the following?

```toml
[tool.ruff]
src = ["src"]
```

---

_Comment by @charliermarsh on 2023-01-04 17:21_

(By default, we use `src = ["."]` which wouldn't pick up your modules.)

---

_Comment by @artpelling on 2023-01-04 17:39_

OK, so I just found the section in the readme regarding `known-first-party` modules with `tool.ruff.isort` which I have not set. However, now everything works without setting `src` nor `known-first-party` which is very peculiar.

Thanks for helping out. I don't want to hijack this issue. If I encounter this problem again, I will give an update. For now, it seems to 'just work again'...

---

_Comment by @charliermarsh on 2023-01-04 17:40_

(Is it possible you were missing an `__init__.py` somewhere? Feel free to file another issue if you have any follow-ups :))

---

_Comment by @artpelling on 2023-01-04 17:47_

Yes, exactly I just found it out, too. I had an `__init__.py` in `src` that produces that behaviour without setting `src`.

So either no `__init__.py` in `src` or setting `src = ["src"]` works correctly. Thanks a lot. Sorry for the clutter, I thought this was related due to the import alias.

---

_Comment by @charliermarsh on 2023-01-04 17:58_

No prob at all, glad it’s resolved!

---

_Comment by @charliermarsh on 2023-01-23 17:33_

I added a note on this to the docs. I think I view this as a known, but acceptable deviation for now.

---

_Closed by @charliermarsh on 2023-01-23 17:33_

---

_Referenced in [astral-sh/ruff#2604](../../astral-sh/ruff/issues/2604.md) on 2023-02-06 11:17_

---

_Referenced in [python-telegram-bot/python-telegram-bot#3594](../../python-telegram-bot/python-telegram-bot/pulls/3594.md) on 2023-03-19 18:07_

---

_Referenced in [astral-sh/ruff#3908](../../astral-sh/ruff/issues/3908.md) on 2023-04-07 22:28_

---

_Referenced in [pypa/packaging#769](../../pypa/packaging/pulls/769.md) on 2024-01-08 22:51_

---

_Referenced in [emlid/code-quality#24](../../emlid/code-quality/pulls/24.md) on 2025-02-24 14:36_

---

_Referenced in [astral-sh/ruff#21863](../../astral-sh/ruff/issues/21863.md) on 2025-12-09 12:51_

---
