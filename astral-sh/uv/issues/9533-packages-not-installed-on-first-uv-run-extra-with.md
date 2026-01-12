```yaml
number: 9533
title: "Packages not installed on first `uv run --extra` with conflicting extras"
type: issue
state: closed
author: eginhard
labels:
  - needs-mre
assignees: []
created_at: 2024-11-29T22:46:33Z
updated_at: 2024-11-30T14:22:19Z
url: https://github.com/astral-sh/uv/issues/9533
synced_at: 2026-01-12T15:59:52Z
```

# Packages not installed on first `uv run --extra` with conflicting extras

---

_@eginhard_

A basic project with 2 conflicting extras (uv 0.5.5 on Ubuntu):
```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.9.0"
dependencies = []

[project.optional-dependencies]
t1 = ["tqdm==4.67.1"]
t2 = ["tqdm==4.67.0"]

[tool.uv]
conflicts = [
  [
    { extra = "t1" },
    { extra = "t2" },
  ],
]
```

If `.venv/` and `uv.lock` are **not** present yet, the first execution of `uv run --extra t1 python -c "import tqdm"` results in `ModuleNotFoundError: No module named 'tqdm'`. It works the second time or when running e.g. `uv lock` before. It also works when the extras are not conflicting.

---

_Comment by @charliermarsh on 2024-11-30 03:35_

Hmm, this ran without error for me. I copied the `pyproject.toml`, then:

```
❯ uv run --extra t1 python -c "import tqdm; print(tqdm.__version__)"
4.67.1
```

---

_Label `needs-mre` added by @charliermarsh on 2024-11-30 03:35_

---

_Comment by @eginhard on 2024-11-30 09:46_

I checked on another machine (Debian 11) and got the same issue. I can always reproduce by deleting the venv and lockfile again:

```bash
$ uv run --extra t1 python -c "import tqdm; print(tqdm.__version__)"
Using CPython 3.12.7
Creating virtual environment at: .venv
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'tqdm'

$ ls .venv/lib/python3.12/site-packages/
__pycache__  _virtualenv.pth  _virtualenv.py

$ uv run --extra t1 python -c "import tqdm; print(tqdm.__version__)"
⠙ Preparing packages... (0/1)
⠙ Preparing packages... (0/1)
⠙ Preparing packages... (0/1)
⠙ Preparing packages... (0/1)
⠙ Preparing packages... (0/1)
⠙ Preparing packages... (0/1)
░░░░░░░░░░░░░░░░░░░░ [0/1] Installing wheels...
Installed 1 package in 105ms
4.67.1

$ ls .venv/lib/python3.12/site-packages/
__pycache__  _virtualenv.pth  _virtualenv.py  tqdm  tqdm-4.67.1.dist-info

$ rm .venv/ uv.lock -rdf

$ uv run --extra t1 python -c "import tqdm; print(tqdm.__version__)"
Using CPython 3.12.7
Creating virtual environment at: .venv
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'tqdm'
```

---

_Comment by @eginhard on 2024-11-30 10:03_

Trying to run this with verbose output actually fails completely:
```bash
$ rm .venv/ uv.lock -rdf
$ RUST_BACKTRACE=full uv run -v --extra t1 python -c "import tqdm; print(tqdm.__version__)"
DEBUG uv 0.5.5
DEBUG Found project root: `/home/eginhard/uv-mre`
DEBUG No workspace root found, using project root
DEBUG Discovered project `project` at: /home/eginhard/uv-mre
DEBUG Using Python request `>=3.9.0` from `requires-python` metadata
DEBUG Searching for Python >=3.9.0 in managed installations or search path
DEBUG Searching for managed installations at `/home/eginhard/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.0-linux-x86_64-gnu`
DEBUG Found `cpython-3.13.0-linux-x86_64-gnu` at `/home/eginhard/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/bin/python3.13` (managed installations)
Using CPython 3.13.0
Creating virtual environment at: .venv
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: project @ file:///home/eginhard/uv-mre
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.9.0
DEBUG Pre-fork all marker environments took 0.000s
DEBUG Splitting resolution on root==0a0.dev0 over project into 3 resolutions with separate markers
DEBUG Adding direct dependency: project*
DEBUG Adding direct dependency: project*
DEBUG Adding direct dependency: project[t1]*
DEBUG Adding direct dependency: project*
DEBUG Adding direct dependency: project[t2]*
DEBUG Searching for a compatible version of project @ file:///home/eginhard/uv-mre (*)
DEBUG Adding transitive dependency for project==0.1.0: project==0.1.0
DEBUG Adding transitive dependency for project==0.1.0: project[t2]==0.1.0
DEBUG Searching for a compatible version of project @ file:///home/eginhard/uv-mre (==0.1.0)
DEBUG Searching for a compatible version of project @ file:///home/eginhard/uv-mre (==0.1.0)
DEBUG Adding transitive dependency for project==0.1.0: tqdm>=4.67.0, <4.67.0+
DEBUG Found fresh response for: https://pypi.org/simple/tqdm/
DEBUG Searching for a compatible version of tqdm (>=4.67.0, <4.67.0+)
DEBUG Selecting: tqdm==4.67.0 [compatible] (tqdm-4.67.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/2b/78/57043611a16c655c8350b4c01b8d6abfb38cc2acb475238b62c2146186d7/tqdm-4.67.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for tqdm==4.67.0: colorama{platform_system == 'Windows'}*
DEBUG Found fresh response for: https://pypi.org/simple/colorama/
DEBUG Searching for a compatible version of colorama{platform_system == 'Windows'} (*)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{platform_system == 'Windows'}==0.4.6
DEBUG Searching for a compatible version of colorama{platform_system == 'Windows'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Tried 3 versions: colorama 1, project 1, tqdm 1
DEBUG all marker environments resolution took 0.003s
DEBUG Searching for a compatible version of project @ file:///home/eginhard/uv-mre (*)
DEBUG Adding transitive dependency for project==0.1.0: project==0.1.0
DEBUG Adding transitive dependency for project==0.1.0: project[t1]==0.1.0
DEBUG Searching for a compatible version of project @ file:///home/eginhard/uv-mre (==0.1.0)
DEBUG Searching for a compatible version of project @ file:///home/eginhard/uv-mre (==0.1.0)
DEBUG Adding transitive dependency for project==0.1.0: tqdm>=4.67.1, <4.67.1+
DEBUG Searching for a compatible version of tqdm (>=4.67.1, <4.67.1+)
DEBUG Selecting: tqdm==4.67.1 [compatible] (tqdm-4.67.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d0/30/dc54f88dd4a2b5dc8a0279bdd7270e735851848b762aeb1c1184ed1f6b14/tqdm-4.67.1-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for tqdm==4.67.1: colorama{platform_system == 'Windows'}*
DEBUG Searching for a compatible version of colorama{platform_system == 'Windows'} (*)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{platform_system == 'Windows'}==0.4.6
DEBUG Searching for a compatible version of colorama{platform_system == 'Windows'} (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of colorama (==0.4.6)
DEBUG Selecting: colorama==0.4.6 [preference] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Tried 6 versions: colorama 2, project 2, tqdm 2
DEBUG all marker environments resolution took 0.001s
DEBUG Searching for a compatible version of project @ file:///home/eginhard/uv-mre (*)
thread 'uv-resolver' panicked at crates/uv-resolver/src/resolver/batch_prefetch.rs:267:53:
IndexSet: index out of bounds
stack backtrace:
   0:     0x5833ab14e3aa - <unknown>
   1:     0x5833aad9a9ab - <unknown>
   2:     0x5833ab10f062 - <unknown>
   3:     0x5833ab151bf7 - <unknown>
   4:     0x5833ab152e4c - <unknown>
   5:     0x5833ab152695 - <unknown>
   6:     0x5833ab1525f9 - <unknown>
   7:     0x5833ab1525e4 - <unknown>
   8:     0x5833aaa6e382 - <unknown>
   9:     0x5833aaa6e97a - <unknown>
  10:     0x5833abfffe42 - <unknown>
  11:     0x5833ab64ff24 - <unknown>
  12:     0x5833ab4d68f1 - <unknown>
  13:     0x5833ab4d67cd - <unknown>
  14:     0x5833ab1561cb - <unknown>
  15:     0x70dd7c69ca94 - start_thread
                               at ./nptl/pthread_create.c:447:8
  16:     0x70dd7c729c3c - __GI___clone3
                               at ./misc/../sysdeps/unix/sysv/linux/x86_64/clone3.S:78
  17:                0x0 - <unknown>
error: The channel closed unexpectedly
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-30 13:04_

---

_Comment by @charliermarsh on 2024-11-30 13:05_

I'll take a second look. I suspect that's unrelated but obviously gets in the way of our debugging here.

---

_Comment by @charliermarsh on 2024-11-30 13:49_

I was able to reproduce!

---

_Closed by @charliermarsh on 2024-11-30 14:22_

---

_Closed by @charliermarsh on 2024-11-30 14:22_

---
