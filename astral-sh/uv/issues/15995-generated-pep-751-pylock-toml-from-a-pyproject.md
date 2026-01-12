```yaml
number: 15995
title: "Generated PEP 751 pylock.toml from a pyproject.toml source does not match the source's requires-python value"
type: issue
state: open
author: AndydeCleyre
labels:
  - bug
  - needs-decision
assignees: []
created_at: 2025-09-22T21:42:22Z
updated_at: 2025-10-02T14:29:02Z
url: https://github.com/astral-sh/uv/issues/15995
synced_at: 2026-01-12T16:02:21Z
```

# Generated PEP 751 pylock.toml from a pyproject.toml source does not match the source's requires-python value

---

_@AndydeCleyre_

### Summary

Hi! It was suggested I create a new issue for the behavior noted at https://github.com/astral-sh/uv/issues/13497#issuecomment-3320061144 .

It seems that using `uv pip compile` to generate a `pylock.toml` from a `pyproject.toml` source uses the current environment's Python for `requires-python`, which can be too restrictive.

```console
$ mise shell python@3.10
$ uv venv
$ . ./.venv/bin/activate
$ python -V
Python 3.10.18
$ uv init
$ uv add attrs
$ uv pip compile -o pylock.toml pyproject.toml 
$ grep -r requires-python .
./pylock.toml:requires-python = ">=3.10.18"
./uv.lock:requires-python = ">=3.10"
./pyproject.toml:requires-python = ">=3.10"
$ deactivate
$ mise shell python@3.10.0
$ uv venv .venv-3-10-0
$ . ./.venv-3-10-0/bin/activate 
$ python -V
Python 3.10.0
$ uv pip install -r pylock.toml 
Using Python 3.10.0 environment at: .venv-3-10-0
error: The requested interpreter resolved to Python 3.10.0, which is incompatible with the `pylock.toml`'s Python requirement: `>=3.10.18`
```

I would instead expect the `requires-python` value to match that in the `pyproject.toml` exactly, and so the `pylock.toml` would be installable for the same range of Python versions.

### Platform

Linux 6.16.8-zen1-1-zen x86_64 GNU/Linux

### Version

uv 0.8.19 (fc7c2f8b5 2025-09-19)

### Python version

Python 3.10.18, Python 3.10.0

---

_Label `bug` added by @AndydeCleyre on 2025-09-22 21:42_

---

_Comment by @zanieb on 2025-09-22 21:51_

I don't know if this is intended to work with `uv pip compile`, which interacts with a `pyproject.toml` in a fairly naive way.

The `requires-python` value is correct with `uv export`.


---

_Comment by @zanieb on 2025-09-22 21:52_

I believe what you're seeing from `uv pip compile` is the version of the Python interpreter we're using for the resolution, not the Python version in the source(s) it reads.

---

_Label `needs-decision` added by @zanieb on 2025-09-22 21:52_

---

_Comment by @AndydeCleyre on 2025-09-25 20:25_

In the temporary workaround department:

```python
#!/usr/bin/env python3
# /// script
# requires-python = ">=3.10"
# dependencies = [
#     "plumbum",
#     "tomlkit",
#     "uv",
# ]
# ///

import sys

import tomlkit
from plumbum import FG, local
from plumbum.cmd import uv

if '-h' in sys.argv[1:] or '--help' in sys.argv[1:]:
    print(f"USAGE: {sys.argv[0]} [-U|--upgrade]", file=sys.stderr)
    sys.exit()

if '-o' in sys.argv[1:] or '--output-file' in sys.argv[1:]:
    print("Broken, pending https://github.com/astral-sh/uv/issues/3248")
    sys.exit(1)

pyproject = local.path('pyproject.toml')
pylock = local.path('pylock.toml')

req_python = tomlkit.parse(pyproject.read())['project'].get('requires-python')

uv_args = [
    'pip',
    'compile',
    '-qo',
    str(pylock.relative_to(local.cwd)),
    str(pyproject.relative_to(local.cwd)),
    *sys.argv[1:],
]
uv[uv_args] & FG

if req_python:
    data = tomlkit.parse(pylock.read())
    data['requires-python'] = req_python
    pylock.write(data.as_string())
```

---

_Comment by @AndydeCleyre on 2025-09-25 20:40_

In case it helps in decision making, [the spec](https://packaging.python.org/en/latest/specifications/pylock-toml/#pylock-toml-spec) defines the field as

> Specifies the [Requires-Python](https://packaging.python.org/en/latest/specifications/core-metadata/#core-metadata-requires-python) for the minimum Python version compatible for any environment supported by the lock file (i.e. the minimum viable Python version for the lock file).

And that link leads to the Core Metadata `requires-python`, which is the same definition applied to the `pyproject.toml` field:

> This field specifies the Python version(s) that the distribution is compatible with. Installation tools may look at this when picking which version of a project to install.

---

_Comment by @zanieb on 2025-09-26 00:43_

In `uv pip compile`, the `pyproject.toml` you input is just one of many sources, e.g., you could include multiple `pyproject.toml` files, or you could provide a `pyproject.toml` and a `requirements.txt` file. There's not a 1:1 mapping between the `requires-python` in one of those sources and the resolution we're performing — so we don't try. In contrast, `uv export` is for a single project. I'm not sure why you can't use that instead :)

---

_Comment by @AndydeCleyre on 2025-09-26 01:32_

I had been avoiding uv commands which push their own notion of a "project," so that the operations would fit well across workflows, and not surprise me with implicit changes.

Is it possible to use uv export without a uv lock file? If we use it to create the pylock.toml then delete the uv lock file, will future locks use the state of pylock.toml to generate minimally changed versions, as I believe the pattern is with uv pip compile?

---

_Comment by @AndydeCleyre on 2025-10-02 14:29_

Here's a much simpler workaround for those of us using mise tasks:

```
uv pip compile --python-version={{exec(command='mise tool --requested python')}} ...
```

---
