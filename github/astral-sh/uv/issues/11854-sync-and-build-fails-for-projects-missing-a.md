---
number: 11854
title: sync and build fails for projects missing a README.md
type: issue
state: closed
author: bjeffrey92
labels:
  - question
assignees: []
created_at: 2025-02-28T14:04:27Z
updated_at: 2025-02-28T14:17:11Z
url: https://github.com/astral-sh/uv/issues/11854
synced_at: 2026-01-07T13:12:18-06:00
---

# sync and build fails for projects missing a README.md

---

_Issue opened by @bjeffrey92 on 2025-02-28 14:04_

### Summary

This may be intended behaviour (if so I would like to understand why this is necessary?) but it does not seem to be possible to build or sync the dependencies for a project if there is not a `README.md` file in the working directory. I came across this problem because I am trying to build a project with uv inside a docker image and I want to keep the image as lightweight as possible. So I don't want to include a `README.md` or anything else other than the `pyproject.toml`, `uv.lock`, and the package source code.

Running `uv sync` (or `uv build`) in a directory which does not include a `README.md` raises this error:
```
▶ uv sync
Using CPython 3.12.8
Creating virtual environment at: .venv
Resolved 1 package in 12ms
  × Failed to build `test-proj @ file:///Users/ben/Desktop/boon/repos/uv-sync-test`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `hatchling.build.build_editable` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 11, in <module>
        File "/Users/ben/.cache/uv/builds-v0/.tmpTRbh53/lib/python3.12/site-packages/hatchling/build.py", line 83, in build_editable
          return os.path.basename(next(builder.build(directory=wheel_directory, versions=['editable'])))
                                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/ben/.cache/uv/builds-v0/.tmpTRbh53/lib/python3.12/site-packages/hatchling/builders/plugin/interface.py", line 90, in build
          self.metadata.validate_fields()
        File "/Users/ben/.cache/uv/builds-v0/.tmpTRbh53/lib/python3.12/site-packages/hatchling/metadata/core.py", line 266, in validate_fields
          self.core.validate_fields()
        File "/Users/ben/.cache/uv/builds-v0/.tmpTRbh53/lib/python3.12/site-packages/hatchling/metadata/core.py", line 1366, in validate_fields
          getattr(self, attribute)
        File "/Users/ben/.cache/uv/builds-v0/.tmpTRbh53/lib/python3.12/site-packages/hatchling/metadata/core.py", line 531, in readme
          raise OSError(message)
      OSError: Readme file does not exist: README.md

      hint: This usually indicates a problem with the package or the build environment.
```
#### Steps To Reproduce
- Create an empty directory
- Create a subdirectory called `test_proj`
- Add a `pyproject.toml` with this contents:
```toml
[build-system]
build-backend = "hatchling.build"
requires = ["hatchling"]

[project]
description = ""
name = "test_proj"
readme = "README.md"
requires-python = ">=3.11,<3.14"
version = "0.1.0"

[tool.hatch.build.targets.wheel]
packages = ["test_proj"]
```
- Run `uv sync/build`, this should raise the error described above
- Then run `touch README.md` and repeat the `uv sync` command and the problem is resolved



### Platform

Darwin 23.6.0 arm64

### Version

uv 0.5.20 (1c17662b3 2025-01-15)

### Python version

Python 3.12.8

---

_Label `bug` added by @bjeffrey92 on 2025-02-28 14:04_

---

_Comment by @charliermarsh on 2025-02-28 14:09_

This isn't a uv issue, exactly -- notice that the trace is from `hatchling`, your build backend. You can remove the `readme = "README.md"` line from your `pyproject.toml` to resolve the error, though. You just can't reference a `README.md` that doesn't exist, per `hatchling`.

---

_Label `bug` removed by @charliermarsh on 2025-02-28 14:09_

---

_Label `question` added by @charliermarsh on 2025-02-28 14:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-28 14:09_

---

_Comment by @bjeffrey92 on 2025-02-28 14:11_

> This isn't a uv issue, exactly -- notice that the trace is from `hatchling`, your build backend. You can remove the `readme = "README.md"` line from your `pyproject.toml` to resolve the error, though. You just can't reference a `README.md` that doesn't exist, per `hatchling`.

Ahh, I see thanks for the explanation and such a speedy response

---

_Comment by @charliermarsh on 2025-02-28 14:17_

No prob! Happy to help.

---

_Closed by @charliermarsh on 2025-02-28 14:17_

---
