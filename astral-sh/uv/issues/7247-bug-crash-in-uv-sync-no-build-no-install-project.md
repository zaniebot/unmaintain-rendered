```yaml
number: 7247
title: "BUG: crash in `uv sync --no-build --no-install-project`"
type: issue
state: closed
author: neutrinoceros
labels:
  - bug
assignees: []
created_at: 2024-09-10T08:50:17Z
updated_at: 2024-09-11T18:31:26Z
url: https://github.com/astral-sh/uv/issues/7247
synced_at: 2026-01-12T15:59:11Z
```

# BUG: crash in `uv sync --no-build --no-install-project`

---

_@neutrinoceros_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

XY problem disclaimer: what I was really trying to accomplish here was *installing dependencies only*, ref #1516

I understand that `uv sync --no-build` is a bit self-contradictory because it requires building the project itself but forbids building anything, but I was still surprised that adding `--no-install-project` didn't help

reprod
```shell
❯ mkdir test_pkg && cd test_pkg
❯ uv init --lib
Initialized project `test-pkg`
❯ uv sync --no-build --no-install-project --verbose
DEBUG uv 0.4.8 (Homebrew 2024-09-09)
DEBUG Found project root: `/private/tmp/test_pkg`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/private/tmp/test_pkg/.python-version`
DEBUG Searching for Python 3.12 in managed installations or system path
DEBUG Searching for managed installations at `/Users/clm/Library/Application Support/uv/python`
DEBUG Found managed installation `cpython-3.12.5-macos-aarch64-none`
DEBUG Found `cpython-3.12.5-macos-aarch64-none` at `/Users/clm/Library/Application Support/uv/python/cpython-3.12.5-macos-aarch64-none/bin/python3` (managed installations)
Using Python 3.12.5
Creating virtualenv at: .venv
DEBUG Using request timeout of 30s
DEBUG Starting clean resolution
DEBUG Allowing build for editable source distribution: test-pkg @ file:///private/tmp/test_pkg
DEBUG Found static `pyproject.toml` for: test-pkg @ file:///private/tmp/test_pkg
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: test-pkg*
DEBUG Searching for a compatible version of test-pkg @ file:///private/tmp/test_pkg (*)
DEBUG Tried 1 versions: test-pkg 1
DEBUG Split universal resolution took 0.000s
Resolved 1 package in 1ms
error: distribution test-pkg==0.1.0 @ editable+. can't be installed because it is marked as `--no-build` but has no binary distribution
```

Is this intended, and is there a way around it ?

```
$ uv --version
uv 0.4.8 (Homebrew 2024-09-09)
```


---

_Comment by @charliermarsh on 2024-09-10 13:23_

Thanks, this is a bug.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-10 13:23_

---

_Label `bug` added by @charliermarsh on 2024-09-10 13:23_

---

_Renamed from "BUG(?): crash in `uv sync --no-build --no-install-project`" to "BUG: crash in `uv sync --no-build --no-install-project`" by @neutrinoceros on 2024-09-10 15:24_

---

_Closed by @charliermarsh on 2024-09-11 18:31_

---
