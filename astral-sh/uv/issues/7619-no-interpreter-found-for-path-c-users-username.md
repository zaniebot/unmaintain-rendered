```yaml
number: 7619
title: "No interpreter found for path `C:/Users/USERNAME/miniforge3/envs/my-custom-env/python` in managed installations, system path, or `py` launcher"
type: issue
state: open
author: kdheepak
labels:
  - question
assignees: []
created_at: 2024-09-22T14:22:04Z
updated_at: 2024-10-21T22:05:59Z
url: https://github.com/astral-sh/uv/issues/7619
synced_at: 2026-01-12T15:59:15Z
```

# No interpreter found for path `C:/Users/USERNAME/miniforge3/envs/my-custom-env/python` in managed installations, system path, or `py` launcher

---

_@kdheepak_

I'm unable to run `uv sync --python=$(which python)` on Windows. I'm getting the following error:

```
$ conda env create -n my-custom-env python=3
$ conda activate my-custom-env
$ conda install uv
$ uv init --lib
$ uv sync --python=$(which python)
error: No interpreter found for path `C:/Users/USERNAME/miniforge3/envs/my-custom-env/python` in managed installations, system path, or `py` launcher
```

This works fine on MacOS by creating a symlink.

```
$ uv --version
uv 0.4.15
```

---

_Comment by @zanieb on 2024-09-22 16:10_

Can you share verbose output with `-v`?

Note you can't use `--python` to sync a project to a different environment. That will just change the interpreter we use to create the environment. Instead, see the [documentation](https://docs.astral.sh/uv/concepts/projects/#configuring-the-project-environment-path) on configuring the project environment.

---

_Label `question` added by @zanieb on 2024-09-22 16:10_

---

_Comment by @kdheepak on 2024-09-22 17:04_

Here's the output of `--verbose`:

```
$ uv sync --python=$(which python) --verbose
DEBUG uv 0.4.15
DEBUG Found project root: `C:\Users\USERNAME\gitrepos\project-foo`
DEBUG No workspace root found, using project root
DEBUG Checking for Python interpreter at path `C:/Users/USERNAME/miniforge3/envs/my-custom-env/python`
error: No interpreter found for path `C:/Users/USERNAME/miniforge3/envs/my-custom-env/python` in managed installations, system path, or `py` launcher
```

> Note you can't use --python to sync a project to a different environment.

Yes, I was mainly just curious and tried it (on my mac it symlinked python into `.venv`), and decided to report because it was different on Windows.

---

_Comment by @zanieb on 2024-10-21 22:05_

Is this still an issue? I wonder if it's because the `.exe` is missing? It's weird that `which` would drop that?

---
