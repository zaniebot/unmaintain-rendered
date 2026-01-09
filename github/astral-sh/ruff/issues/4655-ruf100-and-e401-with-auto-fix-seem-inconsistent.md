---
number: 4655
title: RUF100 and E401 with auto-fix seem inconsistent
type: issue
state: open
author: JannKleen
labels:
  - bug
assignees: []
created_at: 2023-05-25T11:47:30Z
updated_at: 2024-02-15T07:14:28Z
url: https://github.com/astral-sh/ruff/issues/4655
synced_at: 2026-01-07T13:12:14-06:00
---

# RUF100 and E401 with auto-fix seem inconsistent

---

_Issue opened by @JannKleen on 2023-05-25 11:47_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

I have a Django app config that relies on importing some files on initialisation to get various functions in there registered. Having RUF100 enabled and running ruff with --fix, the following happens. The original looks like this:

```python
        import api.checks  # noqa: F401
        import api.signals  # noqa: F401
        import app.celery  # noqa: F401
        import tools.something  # noqa: F401
```

Running ruff will 'fix' it to the following, which seems incorrect (`api.checks` isn't used in this file) but will work and stay like that across multiple ruff runs:

```python
        import api.checks
        import api.signals  # noqa: F401
        import app.celery  # noqa: F401
        import tools.something  # noqa: F401
```

However if I modify it further to:

```python
        import api.checks
        import api.signals
        import app.celery  # noqa: F401
        import tools.something  # noqa: F401
```

Both lines will get deleted the next time I run ruff and I end up with:

```python
        import app.celery  # noqa: F401
        import tools.something  # noqa: F401
```



---

_Renamed from "RUF100 and E401 cause inconsistent situation" to "RUF100 and E401 with auto-fix seem inconsistent" by @JannKleen on 2023-05-25 11:49_

---

_Label `question` added by @charliermarsh on 2023-05-25 13:15_

---

_Comment by @charliermarsh on 2023-05-25 13:16_

Pyflakes has the same behavior. I need to look back at our code to understand why. It has to do with submodule imports, which are kind of complicated (e.g., `import api.checks` in lieu of `from api import checks`).

---

_Comment by @charliermarsh on 2023-05-25 14:27_

I think we can _probably_ flag both of these as unused if `api` is never used.

---

_Label `question` removed by @charliermarsh on 2023-05-25 14:27_

---

_Label `bug` added by @charliermarsh on 2023-05-25 14:27_

---

_Referenced in [astral-sh/ruff#4667](../../astral-sh/ruff/issues/4667.md) on 2023-05-26 12:02_

---

_Comment by @dhruvmanila on 2023-05-26 12:02_

```python
import api.checks  # noqa: F401
import api.signals as signals  # noqa: F401
```

Aliasing works (not sure why though)

---

_Comment by @dhruvmanila on 2023-06-10 16:15_

Related: #60 

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-06-10 18:56_

---

_Referenced in [astral-sh/ruff#5010](../../astral-sh/ruff/pulls/5010.md) on 2023-06-10 18:59_

---

_Referenced in [astral-sh/ruff#5011](../../astral-sh/ruff/pulls/5011.md) on 2023-06-11 02:28_

---

_Unassigned @dhruvmanila by @dhruvmanila on 2024-02-15 07:14_

---
