```yaml
number: 7340
title: "`uv python install <x.y.z>` disregards the versions in `uv python list` installed by other tools"
type: issue
state: closed
author: jond01
labels:
  - question
assignees: []
created_at: 2024-09-12T20:12:20Z
updated_at: 2024-09-13T10:22:04Z
url: https://github.com/astral-sh/uv/issues/7340
synced_at: 2026-01-10T04:45:10Z
```

# `uv python install <x.y.z>` disregards the versions in `uv python list` installed by other tools

---

_Issue opened by @jond01 on 2024-09-12 20:12_

The case is as follows:
1. A specific Python version is already installed by another tool, e.g. pyenv or cached Python on a GitHub runner. Let's say it's 3.10.12, and `python -V` outputs this exact version.
2. `uv python list` seems to discover it, as it's listed in the command's output.
3. Now I want to verify the exact version installation - e.g. just be sure or as a part of some automated workflow.
  I run `uv python install 3.10.12 --no-python-downloads`.
4. The command fails with:
   ```txt
   Searching for Python versions matching: Python 3.10.12
   Python downloads are not allowed (`python-downloads = "never"`). Change to `python-downloads = "manual"` to allow explicit installs.
   ```

The same happens with the `--offline` flag, although the error is different.

I do not expect this behavior, and after reading [the docs](https://docs.astral.sh/uv/guides/install-python/#using-an-existing-python-installation) - I guess it's a bug:

> uv will use existing Python installations if present on your system. There is no configuration necessary for this behavior: uv will use the system Python if it satisfies the requirements of the command invocation.

* A minimal GitHub workflow that reproduces the bug:
  * Workflow: https://github.com/jond01/uv-python-cache/blob/main/.github/workflows/test.yml
  * Run: https://github.com/jond01/uv-python-cache/actions/runs/10837938598/job/30075040681#step:7:32
* The main commands are:
  * `uv python list`, which discovers Python 3.10.12,
  * `uv python install 3.10.12 --offline` / `--no-python-downloads`.
* The uv platform: Linux. I reproduced it also locally on macOS.
* uv version: 0.4.8

This issue is somewhat related to another one I opened: https://github.com/astral-sh/setup-uv/issues/66

---

_Comment by @zanieb on 2024-09-13 01:53_

Sorry if you're using `uv python install` we will always attempt to install a managed version even if you have an existing unmanaged version. If you use `uv run --python 3.10.12 python -V` you can verify that the correct version is installed?

(The existing behavior is intended)

---

_Label `question` added by @zanieb on 2024-09-13 01:53_

---

_Comment by @jond01 on 2024-09-13 10:22_

Right, jumping straight to `uv run --python 3.10.12 python -V` worked :)
I think it can be mentioned here as an alternative:
https://docs.astral.sh/uv/guides/integration/github/#setting-up-python
I found the `UV_PYTHON` env var handy.
Thanks!

---

_Closed by @jond01 on 2024-09-13 10:22_

---
