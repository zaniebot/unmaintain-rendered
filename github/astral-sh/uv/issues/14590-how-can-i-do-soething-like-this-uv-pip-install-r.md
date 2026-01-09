---
number: 14590
title: "How can I do soething like this `uv pip install -r pyproject.toml --group dev`"
type: issue
state: closed
author: CakeCrusher
labels:
  - question
assignees: []
created_at: 2025-07-13T20:46:40Z
updated_at: 2025-07-14T21:26:33Z
url: https://github.com/astral-sh/uv/issues/14590
synced_at: 2026-01-07T13:12:18-06:00
---

# How can I do soething like this `uv pip install -r pyproject.toml --group dev`

---

_Issue opened by @CakeCrusher on 2025-07-13 20:46_

### Question

I need to install from a specific pyproject.toml path but I also need to install a specific group how can I do this


### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @CakeCrusher on 2025-07-13 20:46_

---

_Comment by @charliermarsh on 2025-07-14 01:01_

There isn't standardized syntax for this. I'd probably do:

```
uv pip install /path/to/project
uv pip install -r /path/to/project/pyproject.toml --group dev
```

However, even better would be `uv sync --group dev`, if you're able to use uv's project interface rather than `uv pip`.


---

_Comment by @CakeCrusher on 2025-07-14 02:03_

The problem i think this is invalid syntax `uv pip install -r /path/to/project/pyproject.toml --group dev`  i get an error when I run this. Are you sure this is supposed to work works?

---

_Comment by @charliermarsh on 2025-07-14 02:07_

Oh sorry, it's `uv pip install --group /path/to/project/pyproject.toml:dev`. That's for compatibility with the `pip install` CLI.

---

_Comment by @CakeCrusher on 2025-07-14 14:46_

unfortunately that did not work either

```sh
------
 > [stage-0  8/10] RUN source /.venv/bin/activate &&     uv pip install --group pyproject.toml:dev &&     uv pip install clerk-backend-api==2.0.2 &&     rm -rf /root/.cache/uv:
0.088 error: unexpected argument '--group' found
0.088 
0.088   tip: to pass '--group' as a value, use '-- --group'
0.088 
0.088 Usage: uv pip install [OPTIONS] <PACKAGE|--requirements <REQUIREMENTS>|--editable <EDITABLE>>
0.088 
0.088 For more information, try '--help'.
------
Dockerfile.server.veris.dev:46

--------------------
```

I also tried -- --group

```sh
------
 > [stage-0  8/10] RUN source /.venv/bin/activate &&     uv pip install -- --group pyproject.toml:dev &&     uv pip install clerk-backend-api==2.0.2 &&     rm -rf /root/.cache/uv:
0.107 error: Failed to parse: `--group`
0.107   Caused by: Expected package name starting with an alphanumeric character, found `-`
0.107 --group
0.107 ^
------
```

---

_Comment by @zanieb on 2025-07-14 14:48_

Are you on an old version of uv?

---

_Comment by @zanieb on 2025-07-14 14:49_

See

- https://github.com/astral-sh/uv/pull/10861
- https://github.com/astral-sh/uv/pull/11686

These are available in 0.6.7+

---

_Comment by @CakeCrusher on 2025-07-14 21:25_

thanks @zanieb that is exactly it! and thanks @charliermarsh for helping to narrow things down.
My docker container was using an older version of UV (if not for versioning issues we would probably be a type 3 civilization haha)

---

_Closed by @CakeCrusher on 2025-07-14 21:25_

---
