---
number: 7914
title: uv run ignores --no-dev
type: issue
state: closed
author: Yura52
labels:
  - question
assignees: []
created_at: 2024-10-04T07:41:25Z
updated_at: 2025-05-21T07:48:35Z
url: https://github.com/astral-sh/uv/issues/7914
synced_at: 2026-01-07T13:12:17-06:00
---

# uv run ignores --no-dev

---

_Issue opened by @Yura52 on 2024-10-04 07:41_

I have `jupyterlab` as a _dev_ dependency and I expect that `uv run --no-dev jupyter-lab` will fail, but it successfully runs JupyterLab.

Minimal example:

```
uv init todo
cd todo
uv add --dev jupyterlab
uv run --no-dev jupyter-lab --version
<prints the version>
```

Software versions:

```
macOS 14.5
uv 0.4.18 (7b55e9790 2024-10-01)
```

---

_Comment by @charliermarsh on 2024-10-04 13:09_

Unlike `uv sync`, `uv run` does not perform a "strict" sync. In other words, it does not _uninstall_ packages. In the above example, `uv add` sync'd the dev dependencies, so `jupyter-lab` remains available. This, however, fails as expected:
```
uv init todo
cd todo
uv add --dev jupyterlab --no-sync`
uv run --no-dev jupyter-lab --version
```

---

_Label `question` added by @charliermarsh on 2024-10-04 13:09_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-04 13:09_

---

_Closed by @charliermarsh on 2024-10-04 13:09_

---

_Comment by @sillykelvin on 2025-05-21 07:41_

For people who might visit this issue as `uv run --no-dev` always installs dev dependencies in your environment:
note that the `uv run` command has the form `uv run [OPTIONS] [COMMAND]`, so you should run your script with:

```
uv run --no-dev <your_script>.py
```

**NOT** 

```
uv run <your_script>.py --no-dev
```

The latter one will ignore the `--no-dev` flag silently and install all dev dependencies, and leave me scratching my head for hours.

---
