```yaml
number: 5964
title: Sync with system Python?
type: issue
state: closed
author: b-phi
labels:
  - enhancement
  - configuration
assignees: []
created_at: 2024-08-09T16:54:25Z
updated_at: 2024-09-05T13:56:41Z
url: https://github.com/astral-sh/uv/issues/5964
synced_at: 2026-01-10T04:45:09Z
```

# Sync with system Python?

---

_Issue opened by @b-phi on 2024-08-09 16:54_

I'm trying to use uv to synchronize environments across a cluster. This involves creating a lock file and then using it to install dependencies everywhere. As far as I can tell, `uv sync` only supports installing dependencies in a uv managed venv. Is there anyway to run sync against a target environment, the system Python in this case?

---

_Comment by @b-phi on 2024-08-09 17:54_

I was able to workaround this by activating the environment dynamically, but this does feel like a bit of a hack since it doesn't guarantee that the environments are actually synced. This is with a dask cluster, I'm not aware of an easy way to "switch" to the new venv if the workers are started with the system python.
```python
import runpy

runpy.run_path('.venv/bin/activate_this.py')
```

---

_Comment by @charliermarsh on 2024-08-09 17:55_

Na this isn't supported yet.

---

_Comment by @chrisrodrigue on 2024-08-11 01:39_

I think you can use `uv venv --system-site-packages` but `uv sync` is not yet aware of requirements that are already satisfied by the system/inherited environment so it still attempts to resolve and install them.

---

_Comment by @SamuelT-Beslogic on 2024-08-14 20:38_

In the mean time, I'm addind the venv's bin (or Scripts on Windows) folder to PATH as per https://github.com/jakebailey/pyright-action/tree/v2/?tab=readme-ov-file#using-pyright-from-path :
```yaml
jobs:
  mypy:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: canopeum_backend
    steps:
      - uses: actions/checkout@v4
      - name: Install uv using the standalone installer
        run: curl -LsSf https://astral.sh/uv/install.sh | sh
      - run: uv sync --locked --extra dev
      - run: echo "$PWD/.venv/bin" >> $GITHUB_PATH
      - run: mypy . --python-version=3.12

  pyright:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: canopeum_backend
    steps:
      - uses: actions/checkout@v4
      - name: Install uv using the standalone installer
        run: curl -LsSf https://astral.sh/uv/install.sh | sh
      - run: uv sync --locked --extra dev
      - run: echo "$PWD/.venv/bin" >> $GITHUB_PATH
      - uses: jakebailey/pyright-action@v2
        with:
          version: PATH
          python-version: "3.12"
          working-directory: canopeum_backend
```

And making sure that my docker commands are using `uv run` instead of `python`

---

_Comment by @seppzer0 on 2024-08-25 15:16_

I upvote for the optional synchronization with the host system rather than with venv-only. Not only for CI/CD, but for me personally it is also occasionally preferable to install project packages into the system directly.

---

_Comment by @hauntsaninja on 2024-08-25 20:04_

One option is to symlink:
```
ln -sf $(/path/to/target/python -c 'import sys; print(sys.prefix)') .venv 
uv sync
```

---

_Assigned to @zanieb by @zanieb on 2024-08-26 17:10_

---

_Label `enhancement` added by @zanieb on 2024-08-26 17:10_

---

_Label `configuration` added by @zanieb on 2024-08-26 17:10_

---

_Comment by @seppzer0 on 2024-09-05 11:33_

I suppose it is a functional workaround, but having this implemented natively into uv would be a big improvement

---

_Comment by @zanieb on 2024-09-05 13:56_

This is now supported via `UV_PROJECT_ENVIRONMENT` (#6834) — I still don't recommend it but you don't need to do a symlink.

---

_Closed by @zanieb on 2024-09-05 13:56_

---
