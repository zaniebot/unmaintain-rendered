```yaml
number: 15170
title: uv creates a new directory with a new .venv instead of using the .venv in the project root
type: issue
state: closed
author: dwschulze
labels:
  - bug
assignees: []
created_at: 2025-08-08T17:45:11Z
updated_at: 2025-08-08T21:00:57Z
url: https://github.com/astral-sh/uv/issues/15170
synced_at: 2026-01-12T16:02:05Z
```

# uv creates a new directory with a new .venv instead of using the .venv in the project root

---

_@dwschulze_

From a linux terminal (no VIRTUAL_ENV set):

$ ls -lad .venv
drwxrwxr-x 5 dean dean 4096 Jul 27 13:55 .venv
$ uv run pytest tests/test_train_bpe.py
error: Failed to spawn: `pytest`
  Caused by: No such file or directory (os error 2)
$ uv run --active pytest tests/test_train_bpe.py
error: Failed to spawn: `pytest`
  Caused by: No such file or directory (os error 2)

$ uv -v run pytest tests/test_train_bpe.py
DEBUG uv 0.8.3
DEBUG Found project root: `/home/dean/courses/stanford/stanford.cs336.2025.spring/src/assignment1-basics`
DEBUG No workspace root found, using project root
DEBUG Discovered project `cs336-basics` at: /home/dean/courses/stanford/stanford.cs336.2025.spring/src/assignment1-basics
DEBUG Acquired lock for `/home/dean/courses/stanford/stanford.cs336.2025.spring/src/assignment1-basics`
DEBUG No Python version file found in workspace: /home/dean/courses/stanford/stanford.cs336.2025.spring/src/assignment1-basics
DEBUG Using Python request `>=3.11` from `requires-python` metadata
DEBUG Checking for Python environment at: `.venv`
DEBUG The project environment's Python version satisfies the request: `Python >=3.11`
DEBUG Released lock at `/tmp/uv-3709aa3b51281ab9.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: cs336-basics @ file:///home/dean/courses/stanford/stanford.cs336.2025.spring/src/assignment1-basics
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 70 packages in 1ms
DEBUG Using request timeout of 30s
DEBUG Computed cache info: Some(Timestamp(SystemTime { tv_sec: 1754669305, tv_nsec: 777821987 })), None, None, {}, {"src": None}
DEBUG Requirement already installed: cs336-basics==1.0.3 (from file:///home/dean/courses/stanford/stanford.cs336.2025.spring/src/assignment1-basics)
DEBUG Requirement already installed: einops==0.8.1
DEBUG Requirement already installed: einx==0.3.0
DEBUG Requirement already installed: jaxtyping==0.3.1
DEBUG Requirement already installed: numpy==2.2.4
DEBUG Requirement already installed: psutil==7.0.0
DEBUG Requirement already installed: pytest==8.3.5
DEBUG Requirement already installed: regex==2024.11.6
DEBUG Requirement already installed: submitit==1.5.2
DEBUG Requirement already installed: tiktoken==0.9.0
DEBUG Requirement already installed: torch==2.6.0
DEBUG Requirement already installed: tqdm==4.67.1
DEBUG Requirement already installed: transformers==4.55.0
DEBUG Requirement already installed: wandb==0.19.9
DEBUG Requirement already installed: frozendict==2.4.6
DEBUG Requirement already installed: sympy==1.13.1
DEBUG Requirement already installed: wadler-lindig==0.1.4
DEBUG Requirement already installed: iniconfig==2.1.0
DEBUG Requirement already installed: packaging==24.2
DEBUG Requirement already installed: pluggy==1.5.0
DEBUG Requirement already installed: cloudpickle==3.1.1
DEBUG Requirement already installed: typing-extensions==4.13.1
DEBUG Requirement already installed: requests==2.32.3
DEBUG Requirement already installed: filelock==3.18.0
DEBUG Requirement already installed: fsspec==2025.3.2
DEBUG Requirement already installed: jinja2==3.1.6
DEBUG Requirement already installed: networkx==3.4.2
DEBUG Requirement already installed: nvidia-cublas-cu12==12.4.5.8
DEBUG Requirement already installed: nvidia-cuda-cupti-cu12==12.4.127
DEBUG Requirement already installed: nvidia-cuda-nvrtc-cu12==12.4.127
DEBUG Requirement already installed: nvidia-cuda-runtime-cu12==12.4.127
DEBUG Requirement already installed: nvidia-cudnn-cu12==9.1.0.70
DEBUG Requirement already installed: nvidia-cufft-cu12==11.2.1.3
DEBUG Requirement already installed: nvidia-curand-cu12==10.3.5.147
DEBUG Requirement already installed: nvidia-cusolver-cu12==11.6.1.9
DEBUG Requirement already installed: nvidia-cusparse-cu12==12.3.1.170
DEBUG Requirement already installed: nvidia-cusparselt-cu12==0.6.2
DEBUG Requirement already installed: nvidia-nccl-cu12==2.21.5
DEBUG Requirement already installed: nvidia-nvjitlink-cu12==12.4.127
DEBUG Requirement already installed: nvidia-nvtx-cu12==12.4.127
DEBUG Requirement already installed: setuptools==78.1.0
DEBUG Requirement already installed: triton==3.2.0
DEBUG Requirement already installed: huggingface-hub==0.34.4
DEBUG Requirement already installed: pyyaml==6.0.2
DEBUG Requirement already installed: safetensors==0.6.2
DEBUG Requirement already installed: tokenizers==0.21.4
DEBUG Requirement already installed: click==8.1.8
DEBUG Requirement already installed: docker-pycreds==0.4.0
DEBUG Requirement already installed: gitpython==3.1.44
DEBUG Requirement already installed: platformdirs==4.3.7
DEBUG Requirement already installed: protobuf==5.29.4
DEBUG Requirement already installed: pydantic==2.11.3
DEBUG Requirement already installed: sentry-sdk==2.25.1
DEBUG Requirement already installed: setproctitle==1.3.5
DEBUG Requirement already installed: mpmath==1.3.0
DEBUG Requirement already installed: certifi==2025.1.31
DEBUG Requirement already installed: charset-normalizer==3.4.1
DEBUG Requirement already installed: idna==3.10
DEBUG Requirement already installed: urllib3==2.3.0
DEBUG Requirement already installed: markupsafe==3.0.2
DEBUG Requirement already installed: hf-xet==1.1.7
DEBUG Requirement already installed: six==1.17.0
DEBUG Requirement already installed: gitdb==4.0.12
DEBUG Requirement already installed: annotated-types==0.7.0
DEBUG Requirement already installed: pydantic-core==2.33.1
DEBUG Requirement already installed: typing-inspection==0.4.0
DEBUG Requirement already installed: smmap==5.0.2
Audited 67 packages in 0.21ms
DEBUG Released lock at `/home/dean/courses/stanford/stanford.cs336.2025.spring/src/assignment1-basics/.venv/.lock`
DEBUG Using Python 3.13.5 interpreter at: /home/dean/courses/stanford/stanford.cs336.2025.spring/src/assignment1-basics/.venv/bin/python3
DEBUG Running `pytest tests/test_train_bpe.py`
error: Failed to spawn: `pytest`
  Caused by: No such file or directory (os error 2)



### Summary

 1. uv is creating .venv at ~/courses/stanford/stanford.cs336.2025.spring/assignment1-basics/.venv
  2. But your working directory is ~/courses/stanford/stanford.cs336.2025.spring/src/assignment1-basics/
  3. There's no pyproject.toml in the parent directories that would explain this behavior
  4. PyCharm is setting VIRTUAL_ENV to the wrong path, which suggests it's also confused about the project structure

This happens each time I run a uv command in a terminal in Pycharm which has VIRTUAL_ENV set to the wrong directory (which doesn't even exist) shown in step 1.

$ uv run python --version
warning: `VIRTUAL_ENV=/home/dean/courses/stanford/stanford.cs336.2025.spring/assignment1-basics/.venv` does not match the project environment path `.venv` and will be ignored; use `--active` to target the active environment instead
Python 3.13.5
$ uv run --active python --version
Using CPython 3.13.5 interpreter at: /usr/bin/python3.13
Creating virtual environment at: /home/dean/courses/stanford/stanford.cs336.2025.spring/assignment1-basics/.venv
Installed 67 packages in 183ms
Python 3.13.5


$ uv --version
uv 0.8.3
$ uname -a
Linux hpvictus 6.8.0-65-generic #68~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Tue Jul 15 18:06:34 UTC 2 x86_64 x86_64 x86_64 GNU/Linux



### Platform

Ubuntu 22.04 amd64

### Version

uv 0.8.3

### Python version

3.13

---

_Label `bug` added by @dwschulze on 2025-08-08 17:45_

---

_Comment by @zanieb on 2025-08-08 17:48_

See #9452 we need verbose logs and a reproducible example please.

---

_Comment by @dwschulze on 2025-08-08 17:58_

Here's what happens in a Pycharm terminal which somehow has VIRTUAL_ENV set to a non existant directory:

$ uv run python --version
warning: `VIRTUAL_ENV=/home/dean/courses/stanford/stanford.cs336.2025.spring/assignment1-basics/.venv` does not match the project environment path `.venv` and will be ignored; use `--active` to target the active environment instead
Python 3.13.5
$ uv run --active python --version
Using CPython 3.13.5 interpreter at: /usr/bin/python3.13
Creating virtual environment at: /home/dean/courses/stanford/stanford.cs336.2025.spring/assignment1-basics/.venv
Installed 67 packages in 183ms
Python 3.13.5


---

_Comment by @zanieb on 2025-08-08 19:08_

I don't know what you're asking.

---

_Comment by @dwschulze on 2025-08-08 21:00_

One issue is that there is no pytest binary.  I can use -m pytest instead so that isn't really a problem.

uv run used to create a directory ../../assignment1/.venv and copy the local .venv to it.  That only seemed to happen in pycharm.  For some reason terminals in pycharm all have a VIRTUAL_ENV set to ../../assignment1/.venv which didn't exist.  uv run would create that directory.  This only was a problem in pycharm and I can't figure out where pycharm is getting VIRTUAL_ENV.

THis may be a pycharm issue.  I'll close it.

---

_Closed by @dwschulze on 2025-08-08 21:00_

---
