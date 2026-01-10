```yaml
number: 15682
title: "Always treat conda environments named `base` and `root` as base environments"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/conda-name-ii
created_at: 2025-09-04T14:08:06Z
updated_at: 2025-09-17T17:32:15Z
url: https://github.com/astral-sh/uv/pull/15682
synced_at: 2026-01-10T06:36:15Z
```

# Always treat conda environments named `base` and `root` as base environments

---

_Pull request opened by @zanieb on 2025-09-04 14:08_

Extends https://github.com/astral-sh/uv/pull/15679

I'm not sure if we want to do this. It is probably _generally_ true, but I think I'd rather remove the special casing entirely? I think the main case for this is that it could help prevent regressions from #15679 and we can remove it in a breaking release?

---

_Marked ready for review by @zanieb on 2025-09-15 21:38_

---

_Comment by @zanieb on 2025-09-15 21:38_

Feedback welcome here. 

---

_@konstin approved on 2025-09-16 07:44_

---

_Comment by @notatallshaw-gts on 2025-09-17 14:51_

Trying this out locally.

---

_Comment by @zanieb on 2025-09-17 14:55_

Appreciate it!

---

_Comment by @notatallshaw-gts on 2025-09-17 16:18_

This branch fixes how expected this should work.

Environment:

```
$ env | grep -i "^CONDA"
CONDA_SHLVL=1
CONDA_EXE=/home/STRIKETECH/dshaw/miniforge3/bin/conda
CONDA_PREFIX=/home/STRIKETECH/dshaw/miniforge3
CONDA_PKGS_DIRS=/scratch/.cache/conda/pkgs
CONDA_PYTHON_EXE=/home/STRIKETECH/dshaw/miniforge3/bin/python
CONDA_PROMPT_MODIFIER=false
CONDA_DEFAULT_ENV=base
```

Current uv

```
$ uv --version
uv 0.8.17

$ uv pip install python-stdnum
Using Python 3.13.2 environment at: /home/STRIKETECH/dshaw/miniforge3
Resolved 1 package in 67ms
Installed 1 package in 32ms
 + python-stdnum==2.1

$ uv pip uninstall python-stdnum
Using Python 3.13.2 environment at: /home/STRIKETECH/dshaw/miniforge3
Uninstalled 1 package in 6ms
```

This branches uv:

```
$ ./target/release/uv --version
uv 0.8.17+58 (9410dc5c3 2025-09-17)

$  ./target/release/uv pip install python-stdnum
error: No virtual environment found; run `uv venv` to create an environment, or pass `--system` to install into a non-virtual environment
```

As an aside, I was initially confused because it turned out I had a stray virtual environment in my home directory, so this branches uv was installing and uninstalling to that, and I didn't fully read the output and thought the behavior was the same.

Could not find any behavioral differences when installing into a non-base environment, even trying odd sequences of conda activate/deactivates, and in all those cases the new branch refused to install into the base.

---

_Merged by @zanieb on 2025-09-17 17:32_

---

_Closed by @zanieb on 2025-09-17 17:32_

---

_Branch deleted on 2025-09-17 17:32_

---
