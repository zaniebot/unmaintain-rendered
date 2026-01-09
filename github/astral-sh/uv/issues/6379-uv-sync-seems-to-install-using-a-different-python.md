---
number: 6379
title: "`uv sync` seems to install using a different Python version than in `requires-python`"
type: issue
state: closed
author: krispharper
labels:
  - question
assignees: []
created_at: 2024-08-21T21:37:17Z
updated_at: 2024-08-21T22:11:27Z
url: https://github.com/astral-sh/uv/issues/6379
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv sync` seems to install using a different Python version than in `requires-python`

---

_Issue opened by @krispharper on 2024-08-21 21:37_

I'm getting a dependency resolution failure when running `uv sync` for the first time. This is my `pyproject.toml`.

```toml
[project]
name = "my-project"
version = "1.0.0"
requires-python = " == 3.10.*"
dependencies = [
    "pandas == 1.5.*",
    "internal-package == 2.*",
]

[tool.uv]
index-url = "https://internal-artifactory-url"  // I don't think this is relevant, but just in case, I am pulling from a different index.
```

The `internal-package` has a dependency on pandas which varies based on Python version. For `python >= 3.12.*` it requires `pandas >= 2.1.2`. For `python < 3.12`, it requires `pandas <= 3.0`.

When I run `uv sync`, this is the output

```
× No solution found when resolving dependencies for split (python_full_version >= '3.12'):
╰─▶ Because only the following versions of internal-package are available:
        internal-package<2.dev0
        internal-package==2.0.206
        internal-package==2.0.207
        internal-package==2.0.208
    and because internal-package>=2.0.206,<=2.0.207 depends on pandas>=2.1.2, we can conclude that internal-package>=2.dev0,<2.0.208 depends on pandas>=2.1. (1)

    Because only the following versions of pandas{python_full_version >= '3.12'} are available:
        pandas{python_full_version >= '3.12'}<=2.1.4
        pandas{python_full_version >= '3.12'}>=2.2.0
    and internal-package==2.0.208 depends on pandas{python_full_version >= '3.12'}>=2.1.2, we can conclude that internal-package==2.0.208 depends on one of:
        pandas>=2.1.2,<=2.1.4
        pandas>=2.2.0

    And because we know from (1) that internal-package>=2.dev0,<2.0.208 depends on pandas>=2.1, we can conclude that internal-package>=2.dev0 depends on pandas>=2.1.
    And because your project depends on pandas>=1.5.dev0,<1.6.dev0 and internal-package>=2.dev0, we can conclude that your project's requirements are unsatisfiable.
```

I am brand new to `uv`, but from the looks of this error, I think it's assuming Python 3.12 (`python_full_version >= '3.12'`) instead of Python 3.10 as specified in `project.requires-python`. Do I need to set Python somewhere else?

---

_Comment by @charliermarsh on 2024-08-21 21:42_

By default we treat `requires-python` as a lower bound, but you can instruct uv to explicitly ignore higher Python versions with:
```
[tool.uv]
environments = ["python_version == 3.10"]
```

Let me know if that works (or doesn't).

---

_Comment by @krispharper on 2024-08-21 21:54_

Hmm, that gives me

```
error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 12, column 16
   |
12 | environments = ["python_version == 3.10"]
   |                ^^^^^^^^^^^^^^^^^^^^^^^^^^
Expected a valid marker name, found '3.10'
python_version == 3.10
                  ^^^^
```

I tried `environments = ["python_version == 3.10.*"]` and `environments = ["python_version == 3.10.10"]` as well with the same results.

---

_Comment by @charliermarsh on 2024-08-21 21:56_

Gah, sorry: `environments = ["python_version == '3.10'"]`

---

_Comment by @krispharper on 2024-08-21 21:59_

Nice, that worked perfectly. Thanks a bunch for your help @charliermarsh.

---

_Closed by @krispharper on 2024-08-21 21:59_

---

_Comment by @charliermarsh on 2024-08-21 22:00_

No problem, glad to hear it! 

---

_Label `question` added by @charliermarsh on 2024-08-21 22:11_

---

_Referenced in [astral-sh/uv#6414](../../astral-sh/uv/pulls/6414.md) on 2024-08-22 08:09_

---
