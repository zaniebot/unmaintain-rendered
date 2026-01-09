---
number: 9993
title: Isolate uv virtual environment from the system python
type: issue
state: closed
author: brunorigal
labels: []
assignees: []
created_at: 2024-12-18T09:55:13Z
updated_at: 2024-12-18T14:27:41Z
url: https://github.com/astral-sh/uv/issues/9993
synced_at: 2026-01-07T13:12:18-06:00
---

# Isolate uv virtual environment from the system python

---

_Issue opened by @brunorigal on 2024-12-18 09:55_

I created a virtual environment for my package using uv sync. The virtual environment is activated. And the library is missing the pytest package. Nevertheless, I can still access the pytest library installed in the system version of python. I would rather have a missing pytest error, because the uv run pytest command cannot access the virtual env and fail in a misleading way.

```bash
(docparser) brigal@675c178416a6:/workspace/docparser$ uv run which pytest
/usr/local/bin/pytest
(docparser) brigal@675c178416a6:/workspace/docparser$ echo $VIRTUAL_ENV
/workspace/docparser/.venv
(docparser) brigal@675c178416a6:/workspace/docparser$ uv run which python
/workspace/docparser/.venv/bin/python
```
Is there a reason why the uv venv are not isolated from the system python? Is there a parameter to solve this?

---

_Comment by @nathanscain on 2024-12-18 13:52_

`uv run` puts the venv earlier on the `PATH`; however, it doesn't fully replace the `PATH`. This is because you sometimes need to run venv-aware system commands (think `make` or even `which` like your example)

Since there is no way to tell that `/usr/local/bin/pytest` *could have been* a venv managed executable in a general way, this has the current behavior. Changing this would require that `uv` be aware of all possible executables produced by **any** package which would be very slow, unexpected, and likely break workflows as I'm sure some package overrides `make`.

Maybe a warning for certain common cases could be added, but I'd favor the current behavior.

---

_Comment by @brunorigal on 2024-12-18 14:25_

It sounds reasonable. Thanks for the explanation.

---

_Closed by @brunorigal on 2024-12-18 14:25_

---

_Comment by @nathanscain on 2024-12-18 14:27_

Happy to help 🤙

---
