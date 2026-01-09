---
number: 7865
title: Bundle Python Interpreter in virtual environments
type: issue
state: open
author: Turakar
labels:
  - enhancement
assignees: []
created_at: 2024-10-02T09:51:49Z
updated_at: 2025-04-09T16:24:14Z
url: https://github.com/astral-sh/uv/issues/7865
synced_at: 2026-01-07T13:12:17-06:00
---

# Bundle Python Interpreter in virtual environments

---

_Issue opened by @Turakar on 2024-10-02 09:51_

For system wide installations of applications build with `uv`, it would be nice to have an option to build an execution environment which is self contained and does not rely on the user's local files (`~/.local/share/uv`). My usecase for this would be an application that I want to install to a server for system-wide usage. At the moment, you can achieve this with the following commands (with some path adjustments):

```bash
uv venv --no-cache
uv pip install --no-cache -r requirements.lock
cp -r ~/.local/share/uv/python/cpython-3.12.5-linux-x86_64-gnu python
rm .venv/bin/python
ln -s python/bin/python3.12 .venv/bin/python

# To run the program
.venv/bin/python <your script.py>
```

I.e., you can replace the symlinked python interpreter by a bundled copy.

I am not entirely sure how this should be implemented. In a first simple step, one could add a `--bundle-python` flag to `uv venv` which copies the downloaded Python interpreter to, e.g., `.venv/python` and uses it instead of a path in `~/.local/share/uv`.

However, you can take this idea further, up to a `uv install` command which would do something similar to `make install`.

---

_Comment by @Turakar on 2024-10-02 11:28_

Ok, you can kinda do this already by passing `UV_PYTHON_INSTALL_DIR` [(docs)](https://docs.astral.sh/uv/configuration/environment/) to `uv venv`.

---

_Closed by @Turakar on 2024-10-02 11:28_

---

_Reopened by @zanieb on 2024-10-02 14:44_

---

_Renamed from "Bundle Python Interpreter" to "Bundle Python Interpreter in virtual environments" by @zanieb on 2024-10-02 14:45_

---

_Comment by @zanieb on 2024-10-02 14:45_

I think we want to add this feature.

---

_Label `enhancement` added by @zanieb on 2024-10-02 14:45_

---

_Comment by @zanieb on 2024-10-02 14:45_

Related

- https://github.com/astral-sh/uv/issues/6970

---

_Comment by @danielgafni on 2024-10-02 15:40_

Ohh til `UV_PYTHON_INSTALL_DIR` exists. 

I was looking for this parameter in CLI `--help` and it's not mentioned there. 

Perhaps 
1. The env var should be mentioned in `uv python *` docs
2. `uv python install *` commands should take an `--install-dir` argument? 


---

_Comment by @zanieb on 2024-10-02 16:03_

Yes we should mention it in the help documentation. I'd also be down to accept a target directory in the CLI.

---

_Comment by @danielgafni on 2024-10-04 08:32_

Ok, I started working on this in #7920. Does `--install-dir` name work? 

---

_Referenced in [astral-sh/uv#7419](../../astral-sh/uv/issues/7419.md) on 2024-10-08 01:47_

---

_Comment by @alexwilson1 on 2025-01-29 19:47_

Bundling Python into .venv using UV_PYTHON_INSTALL_DIR does not work because when .venv is not empty (contains Python) the installation gives this error:

`error: Project virtual environment directory cannot be used because it is not a compatible environment but cannot be recreated because it is not a virtual environment`

--bundle-python would be ideal

---

_Referenced in [astral-sh/python-build-standalone#381](../../astral-sh/python-build-standalone/issues/381.md) on 2025-01-29 23:31_

---

_Referenced in [astral-sh/uv#11056](../../astral-sh/uv/issues/11056.md) on 2025-02-01 17:36_

---

_Comment by @ginkgo97 on 2025-02-04 05:48_

Will the option `--install-dir` or the env `UV_PYTHON_INSTALL_DIR` be able to be configured in `uv.toml`?

---

_Referenced in [astral-sh/uv#11541](../../astral-sh/uv/issues/11541.md) on 2025-02-15 20:15_

---

_Referenced in [astral-sh/uv#11927](../../astral-sh/uv/issues/11927.md) on 2025-03-03 19:44_

---

_Referenced in [astral-sh/uv#12298](../../astral-sh/uv/issues/12298.md) on 2025-03-18 19:13_

---

_Comment by @kevinmarks-b on 2025-04-09 16:24_

I'm trying to do this to get the Python binary into an RPM for deployment on AWS. 
However, it seems impossible to get uv to see the python installed in a nearby directory with `UV_PYTHON_INSTALL_DIR` - even if I do as the OP did and symlink it to the install, it destroys that version and reinstalls when I use `uv run` to the ~/.local/share/uv/python/ one instead. 
Now I'm trying to do this in a make file, so `UV_PYTHON_INSTALL_DIR` may not persist into the runtime. Is there a way to make uv find it?

---

_Referenced in [astral-sh/uv#12798](../../astral-sh/uv/issues/12798.md) on 2025-04-10 03:52_

---

_Referenced in [astral-sh/uv#13097](../../astral-sh/uv/issues/13097.md) on 2025-04-25 07:56_

---

_Referenced in [astral-sh/uv#14198](../../astral-sh/uv/issues/14198.md) on 2025-06-22 12:33_

---

_Referenced in [astral-sh/uv#14602](../../astral-sh/uv/issues/14602.md) on 2025-07-14 14:47_

---

_Referenced in [astral-sh/uv#15751](../../astral-sh/uv/issues/15751.md) on 2025-09-16 02:02_

---

_Referenced in [astral-sh/uv#16289](../../astral-sh/uv/issues/16289.md) on 2025-10-13 23:07_

---
