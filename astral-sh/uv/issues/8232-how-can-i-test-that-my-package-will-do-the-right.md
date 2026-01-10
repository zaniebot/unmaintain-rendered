---
number: 8232
title: How can I test that my package will do the right thing when it is installed?
type: issue
state: closed
author: vedantroy
labels:
  - question
assignees: []
created_at: 2024-10-16T00:11:12Z
updated_at: 2024-10-16T23:34:22Z
url: https://github.com/astral-sh/uv/issues/8232
synced_at: 2026-01-10T01:24:25Z
---

# How can I test that my package will do the right thing when it is installed?

---

_Issue opened by @vedantroy on 2024-10-16 00:11_

I have written a simple Python package -- is there a way to test that people will be able to import it (if they install it from PyPi, etc.), and call the expected functions -- without me having to publish the package to the test PyPi registry.

Also (this is probably not be the right place to ask this, but maybe the responder will just know the answer off the top of their head) ... I'm trying to do the following:
```
pyproject.toml
src/
    foo/
        bar.py <-- imports from baz.py using "from foo.baz import some_function"
        baz.py
```

And I want people to be able to run bar with `python3 -m foo.bar:main`, but right now I'm getting errors that the module "foo" does not exist.

The FluxAI repository somehow has made this work: https://github.com/black-forest-labs/flux. Do anyone know how?


---

_Comment by @samypr100 on 2024-10-16 00:50_

This might help

1. Build it with `uv build`, copy the wheel from dist
2. Make a new virtual env via `uv venv`, then `uv pip install /path/to/wheel`
3. Run it using something like `uv run python -m foo.bar:main`

---

_Label `question` added by @zanieb on 2024-10-16 03:38_

---

_Comment by @paveldikov on 2024-10-16 16:40_

^ the above high-level approach is also encapsulated by frameworks such as `tox` (and you can use `tox-uv` so that you benefit from the performance boost of `uv venv` and `uv build`)

---

_Closed by @vedantroy on 2024-10-16 23:34_

---
