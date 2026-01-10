---
number: 7157
title: Wrong output of uv pip compile
type: issue
state: closed
author: hexponent
labels:
  - question
assignees: []
created_at: 2024-09-07T08:01:08Z
updated_at: 2024-09-20T05:33:07Z
url: https://github.com/astral-sh/uv/issues/7157
synced_at: 2026-01-10T01:24:11Z
---

# Wrong output of uv pip compile

---

_Issue opened by @hexponent on 2024-09-07 08:01_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

So I'm planning to add lock file to my project and while comparing existing solutions I encountered a difference in `pip-compile` (from `pip-tools`) and `uv pip compile` results. The result makes me think it is the bug on uv's side.

Consider the following requirements file:
`requirements.in`
```
celery<5
flower==0.9.2
```
After comparing `pip-compile` and `uv pip compile requirements.in -o requirements.txt`, there is one more entry in uv's output, namely:
```
futures==3.4.0
    # via flower
```
If we check `flower`'s [source code](https://github.com/mher/flower/blob/v0.9.2/setup.py#L48), `futures` is required only for python 2, so it should not appear in uv's output, since it only works with python 3.7+.

---

_Comment by @charliermarsh on 2024-09-07 11:55_

If present, we read from the `requires.txt` in the shipped source distribution, and that file includes `futures`. We generally don't really support packages that have requirements that vary across builds (like, in newer releases, this should be expressed as `futures ; python_version < "3.0"` or similar). So it's consistent with how we treat metadata. I'd suggest either upgrading your flower version (newer versions don't have this issue) or using `--no-emit-package futures`.

---

_Label `question` added by @charliermarsh on 2024-09-07 11:55_

---

_Comment by @hexponent on 2024-09-07 19:34_

Thanks! Upgrading package is definitely something I want to do in the future, but for now I added this setting in `pyproject.toml`

---

_Closed by @hexponent on 2024-09-07 19:34_

---

_Referenced in [astral-sh/uv#7563](../../astral-sh/uv/issues/7563.md) on 2024-09-19 22:13_

---

_Comment by @andreiburov on 2024-09-20 05:08_

> I'd suggest either upgrading your flower version (newer versions don't have this issue) or using `--no-emit-package futures`.

In case we are using `uv` as project manager (uv sync/add/remove). Is there an option to skip transient dependency on futures that got picked up although it's not needed? (Something similar to `uv pip-compile --no-emit-package futures`.)

Here is the error:

```
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: futures==3.4.0
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
This backport is meant only for Python 2.
It does not work on Python 3, and Python 3 users do not need it as the concurrent.futures package is available in the standard library.
For projects that work on both Python 2 and 3, the dependency needs to be conditional on the Python version, like so:
extras_require={':python_version == "2.7"': ['futures']}
---
```

Maybe something like `tool.uv.override-dependencies` or `[[tool.uv.dependency-metadata]]`? Although rewriting all transient deps without futures is cumbersome :(

---

_Comment by @andreiburov on 2024-09-20 05:33_

Oh actually I found this solution https://github.com/astral-sh/uv/issues/4422#issuecomment-2254182800.

---
