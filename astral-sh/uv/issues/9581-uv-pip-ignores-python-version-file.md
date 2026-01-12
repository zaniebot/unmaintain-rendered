```yaml
number: 9581
title: "`uv pip` ignores `.python-version` file"
type: issue
state: open
author: oliversen
labels:
  - needs-decision
  - breaking
assignees: []
created_at: 2024-12-02T19:37:11Z
updated_at: 2025-02-17T08:42:46Z
url: https://github.com/astral-sh/uv/issues/9581
synced_at: 2026-01-12T15:59:53Z
```

# `uv pip` ignores `.python-version` file

---

_@oliversen_

Currently, for any `uv pip` commands, you must explicitly specify the `--python-version` option. The `.python-version` file is ignored. Why?

uv 0.5.4
Windows 10

---

_Comment by @zanieb on 2024-12-02 20:12_

Can you share the specific `uv pip` command you're using? We might want different behavior per `uv pip` command.

Regardless, the goal of the `uv pip` interface is compatibility with `pip` which does not read these files. We need a strong justification to differ from the upstream.

---

_Comment by @samypr100 on 2024-12-03 02:13_

Agreed, I believe this would be a breaking change

---

_Label `needs-decision` added by @charliermarsh on 2024-12-06 00:33_

---

_Label `breaking` added by @charliermarsh on 2024-12-06 00:33_

---

_Comment by @oliversen on 2024-12-06 19:28_

> Can you share the specific `uv pip` command you're using?

I am using uv in the build of an extension. First, I use the command `uv export --only-group bundle -o ./tmp/bundle-reqs.txt`. And this command uses the `.python-version` file. Then I use `uv pip install -r ./tmp/bundle-reqs.txt --target ./bundled/libs --python-version 3.9`. Here I have to add `--python-version 3.9`. When I need to change the python version, I will have to change it in two places, which I need to keep in mind. It seems to me that this is not predictable and confusing. Also the fact that `uv pip` ignores the `.python-version` file is not mentioned anywhere in the documentation.

---

_Comment by @ollie-bell on 2024-12-30 14:23_

Having a "single source of truth" for the python version in either `pyproject.toml` or `.python-version` file isn't currently straightforwardly achievable when using `uv pip ...` as it is when using native `uv ...` commands.

It would be nice if for example `uv pip compile` defaults `--python-version` if not explicitly given to the value in `.python-version` file or the `requires-python` in `pyproject.toml`.

If it is too tricky or undesirable to untangle with the current default / pip compatibility for the version of the Python interpreter used, then perhaps a new option could be added like `--python-version-file` similar to how the [`setup-python` action allows](https://github.com/actions/setup-python/blob/main/docs/advanced-usage.md#using-the-python-version-file-input)? Or perhaps `--python-version` could accept a filename of `pyproject.toml` or `.python-version` as an argument?


---

_Comment by @staticf0x on 2025-02-12 13:32_

I can reproduce this issue.

**.python-version**:
```
3.12
```

**pyproject.toml**
```
requires-python = ">=3.12"
```

Result of `$ uv python find`: `/usr/bin/python3.12`

The default system python, though, is 3.9: `$ command -v python3` -> `/usr/bin/python3` and `$ python3 --version` -> `Python 3.9.21`.

So `uv` can definitely find the 3.12 if I ask it to, but here's what's happen with `uv pip compile`:

```
$ uv -v pip compile requirements.in
[...]
DEBUG Starting Python discovery for a default Python
DEBUG Looking for exact match for request a default Python
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `/home/renovate/.local/share/uv/python`
DEBUG Found `cpython-3.9.21-linux-x86_64-gnu` at `/usr/bin/python` (first executable in the search path)
DEBUG Using Python 3.9.21 interpreter at /usr/bin/python for builds
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.9.21
DEBUG Solving with target Python version: >=3.9.21
```

The same happens with `UV_PYTHON=3.12`.

However, if I specify `-p` directly:

```
$ uv -v pip compile -p 3.12 requirements.in
[...]
DEBUG Starting Python discovery for Python 3.12
DEBUG Looking for exact match for request Python 3.12
DEBUG Searching for Python 3.12 in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `/home/renovate/.local/share/uv/python`
DEBUG Found `cpython-3.12.5-linux-x86_64-gnu` at `/usr/bin/python3.12` (first executable in the search path)
DEBUG Using Python 3.12.5 interpreter at /usr/bin/python3.12 for builds
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12
```

So not only it ignores `.python-version`, but also `UV_PYTHON`.

Edit: using `uv 0.5.30`

---

_Comment by @zanieb on 2025-02-13 19:41_

With #11486 we'll respect `UV_PYTHON` in `uv pip compile`

---

_Comment by @staticf0x on 2025-02-17 08:42_

Can confirm with 0.6.0 that `UV_PYTHON` is honored, but `.python-version` still isn't.

---
