---
number: 5040
title: "`uv` fails to install `torch-scatter` with `ModuleNotFoundError`, `pip` works"
type: issue
state: closed
author: janosh
labels:
  - error messages
assignees: []
created_at: 2024-07-13T19:24:02Z
updated_at: 2024-07-13T21:12:27Z
url: https://github.com/astral-sh/uv/issues/5040
synced_at: 2026-01-10T01:23:44Z
---

# `uv` fails to install `torch-scatter` with `ModuleNotFoundError`, `pip` works

---

_Issue opened by @janosh on 2024-07-13 19:24_

i'm running python 3.12 on macOS 14.5 in a `uv` venv

```sh
$ uv pip install torch torch-scatter

# torch installs fine

error: Failed to download and build `torch-scatter==2.1.2`
  Caused by: Failed to build: `torch-scatter==2.1.2`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/Users/janosh/Library/Caches/uv/environments-v0/.tmp8n22Qt/lib/python3.12/site-packages/setuptools/build_meta.py", line 327, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/janosh/Library/Caches/uv/environments-v0/.tmp8n22Qt/lib/python3.12/site-packages/setuptools/build_meta.py", line 297, in _get_build_requires
    self.run_setup()
  File "/Users/janosh/Library/Caches/uv/environments-v0/.tmp8n22Qt/lib/python3.12/site-packages/setuptools/build_meta.py", line 497, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/Users/janosh/Library/Caches/uv/environments-v0/.tmp8n22Qt/lib/python3.12/site-packages/setuptools/build_meta.py", line 313, in run_setup
    exec(code, locals())
  File "<string>", line 8, in <module>
ModuleNotFoundError: No module named 'torch'
```

meanwhile `pip` succeeds

```sh
$ pip install torch torch-scatter

Requirement already satisfied: torch in /Users/janosh/.venv/py312/lib/python3.12/site-packages (2.3.1)
Collecting torch-scatter
  Downloading torch_scatter-2.1.2.tar.gz (108 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 108.0/108.0 kB 4.4 MB/s eta 0:00:00
  Preparing metadata (setup.py) ... done
Requirement already satisfied: filelock in /Users/janosh/.venv/py312/lib/python3.12/site-packages (from torch) (3.15.4)
Requirement already satisfied: typing-extensions>=4.8.0 in /Users/janosh/.venv/py312/lib/python3.12/site-packages (from torch) (4.12.2)
Requirement already satisfied: sympy in /Users/janosh/.venv/py312/lib/python3.12/site-packages (from torch) (1.13.0)
Requirement already satisfied: networkx in /Users/janosh/.venv/py312/lib/python3.12/site-packages (from torch) (3.3)
Requirement already satisfied: jinja2 in /Users/janosh/.venv/py312/lib/python3.12/site-packages (from torch) (3.1.4)
Requirement already satisfied: fsspec in /Users/janosh/.venv/py312/lib/python3.12/site-packages (from torch) (2024.6.1)
Requirement already satisfied: MarkupSafe>=2.0 in /Users/janosh/.venv/py312/lib/python3.12/site-packages (from jinja2->torch) (2.1.5)
Requirement already satisfied: mpmath<1.4,>=1.1.0 in /Users/janosh/.venv/py312/lib/python3.12/site-packages (from sympy->torch) (1.3.0)
Building wheels for collected packages: torch-scatter
  Building wheel for torch-scatter (setup.py) ... done
  Created wheel for torch-scatter: filename=torch_scatter-2.1.2-cp312-cp312-macosx_14_0_arm64.whl size=265133 sha256=5858d50f99b14b323d9dd09a7969aa3dfbe1943c3ca3169729a40be0ef91f5f7
  Stored in directory: /Users/janosh/Library/Caches/pip/wheels/84/20/50/44800723f57cd798630e77b3ec83bc80bd26a1e3dc3a672ef5
Successfully built torch-scatter
Installing collected packages: torch-scatter
Successfully installed torch-scatter-2.1.2
```





---

_Comment by @charliermarsh on 2024-07-13 19:25_

`pip install torch torch-scatter --use-pep517` fails in the same way. You'll need to install `torch`, then install `torch-scatter` with `--no-build-isolation`, since `torch-scatter` doesn't declare `torch` as a build-time dependency (but does depend on it).

---

_Comment by @charliermarsh on 2024-07-13 19:25_

See: https://github.com/astral-sh/uv/issues/2252/.

---

_Comment by @charliermarsh on 2024-07-13 19:25_

I'm going to add a custom error for this since it's so common.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-13 19:25_

---

_Label `error messages` added by @charliermarsh on 2024-07-13 19:25_

---

_Comment by @janosh on 2024-07-13 19:30_

thanks for the lightning response. a custom error recommending `--no-build-isolation` would be great time saver! 👍 

---

_Comment by @charliermarsh on 2024-07-13 19:36_

I'll keep this open to track that!

---

_Referenced in [astral-sh/uv#5041](../../astral-sh/uv/pulls/5041.md) on 2024-07-13 21:04_

---

_Closed by @charliermarsh on 2024-07-13 21:12_

---

_Closed by @charliermarsh on 2024-07-13 21:12_

---

_Referenced in [rusty1s/pytorch_scatter#455](../../rusty1s/pytorch_scatter/issues/455.md) on 2024-08-05 14:29_

---
