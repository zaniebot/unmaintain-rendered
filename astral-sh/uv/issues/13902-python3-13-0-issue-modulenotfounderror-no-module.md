```yaml
number: 13902
title: "python3.13.0 issue -- `ModuleNotFoundError: No module named 'json'`"
type: issue
state: closed
author: mcint
labels:
  - bug
assignees: []
created_at: 2025-06-08T20:43:05Z
updated_at: 2025-06-10T10:50:07Z
url: https://github.com/astral-sh/uv/issues/13902
synced_at: 2026-01-12T16:01:39Z
```

# python3.13.0 issue -- `ModuleNotFoundError: No module named 'json'`

---

_@mcint_

### Summary

Affecting only `python3.13`-based projects, (not 3.12, 3.11)

even the `uv run python --version` command fails with the same error

```
0$ uv run python --version                                                                                                                 error: Querying Python at `/home/user/dev/proj/.venv/bin/python3` failed with exit status exit sta tus: 1                                                                                                                                                                                                                                                                                  [stderr]                                                                                                                                    Traceback (most recent call last):                                                                                                            File "<string>", line 1, in <module>                                                                                                          import sys; sys.path = ["/home/user/.cache/uv/.tmpr6ivOR"] + sys.path; from python.get_interpreter_info import main; main()                                                                                        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^                     File "/home/user/.cache/uv/.tmpr6ivOR/python/get_interpreter_info.py", line 9, in <module>                                                  import json                                                                                                                             ModuleNotFoundError: No module named 'json'
```

### Platform

Linux 6.5.0-45-generic x86_64 GNU/Linux

### Version

uv 0.7.12

### Python version

Python 3.13.0

---

_Label `bug` added by @mcint on 2025-06-08 20:43_

---

_Comment by @mcint on 2025-06-08 21:23_

It appears I've fixed this issue with an update to 3.13 patch version installed, `3.13.0` to `3.13.4`. `uv python install 3.13`.

---

Initially, this presented as an issue with the second invocation of `uv add` in a new virtual environment, failing to load the interpreter for the python environment just materialized. And other uses of `uv run` in a project with venv.

I still don't understand what differs, between 3.13.0 and 3.13.4 in this context. I notice that  uv managed python 3.13 used `/tmp/user/1000/.tmpXXXXX/` path when invoked with `--no-cache`, but failed with the same error as the base interpreter in opening `/home/[user]/.cache/uv/.tmpXXXXX/python/get_interpreter_info.py`. Perhaps 3.13 didn't support or included an unexpected change in sys.path use or module import handling.

---

In the future, how can I better debug this?  Web search summarizing llms are offering apparently made up `UV_DEBUG=` and `UV_LOG_LEVEL=`, which looks especially like hallucinations from common python idioms.

Real uv options include `RUST_LOG=uv=debug` and `RUST_LOG=trace`, or `--verbose` cli flag, and intensifier `UV_LOG_CONTEXT=...`, from https://docs.astral.sh/uv/reference/environment/.

---

_Closed by @mcint on 2025-06-08 21:23_

---

_Comment by @mcint on 2025-06-08 21:45_

Reproduction still works, specifying patch version, succeeds on first run, fails on second.

- `$ uv run --python 3.13.0 python -c 'import sys; print(sys.path)'`

After running with `--python 3.13.0` successfully all subsequent attempts to use any `3.13.x` versions will fail until you remove the venv and allow another version to be used.

With logging, equivalent:
- `$ RUST_LOG=uv=debug uv run --python 3.13.0 python -c 'import sys; print(sys.path)'`
- `$ uv --verbose run --python 3.13.0 python -c 'import sys; print(sys.path)'`

---

_Comment by @konstin on 2025-06-10 10:50_

Is there a specific reason for using 3.13.0? We only build and support the latest patch version of a Python minor version, so we recommend install the latest patch version for this kind of problem, which is interpreter-specific, not uv-specific.

---
