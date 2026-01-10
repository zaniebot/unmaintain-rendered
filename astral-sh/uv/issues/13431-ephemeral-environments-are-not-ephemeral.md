---
number: 13431
title: Ephemeral environments are not ephemeral
type: issue
state: open
author: engpas
labels:
  - question
assignees: []
created_at: 2025-05-13T13:55:24Z
updated_at: 2025-05-13T14:15:12Z
url: https://github.com/astral-sh/uv/issues/13431
synced_at: 2026-01-10T01:25:33Z
---

# Ephemeral environments are not ephemeral

---

_Issue opened by @engpas on 2025-05-13 13:55_

# Expectation
The [documentation](https://docs.astral.sh/uv/reference/cli/#uv-run) of  the `uv run` command states
>  If the script contains inline dependency metadata, it will be installed into an isolated, **ephemeral** environment.

Thus my expectation upon running a python script with script header via
```
uv run script.py
```
is that uv will generate a virtual environment, install the declared dependencies in this environment, execute the script in this environment, and finally clean up the environment.

# Observation
In my (HPC) use case, everything is organized in projects (not in the uv sense). Per project, I run the same script in hundreds to thousands of different folders. There is no upper limit on the number of projects.

After several ten thousand script runs via `uv run script.py` over the timeframe of roughly a week, I realized that the uv cache directory had grown to the order of a hundred gigabytes in size. Apparently, the `environments-v2` subfolder had cached one environment per script that was run, not ephemerally, but forever. While the dependencies were indeed hardlinked into the venvs, each virtual environment directory still occupied several megabytes of disk space.

# Reproduction
This can be reproduced with the following steps: 

- Take the following script.py:
```
# /// script
# requires-python = ">=3.10,<=3.10"
# dependencies = [
#      "scipy",
#      "numpy",
#      "matplotlib",
#      "pandas",
# ]
# ///

import sys

print("Hello World!")
print(sys.executable)
```
- Execute the script via `uv run --cache-dir ~/uv-testing/local-cache script.py`
- Check the cache size: `ncdu ~/uv-testing/local-cache`
![Image](https://github.com/user-attachments/assets/0395fa87-662a-46bc-9b79-b2a24f4d2441)
- Check contents of environments-v2: `ls ~/uv-testing/local-cache/environments-v2/`
```
script-54e2783e35ec760d
```
- Copy script to several directories and execute all copies:
```
mkdir -p dir{0..9}
for i in {0..9}; do cp script.py dir$i/; done
for i in {0..9}; do uv run --cache-dir ~/uv-testing/local-cache dir$i/script.py; done
```
- Check the cache size: `ncdu ~/uv-testing/local-cache/`
![Image](https://github.com/user-attachments/assets/4c36672d-375b-48a9-9ab1-849481aaeadf)
- Check contents of environments-v2: `ls ~/uv-testing/local-cache/environments-v2/`
```
script-4307683faee53c3a  script-54e2783e35ec760d  script-660d48cc284811d8  script-a55279b276551a9d  script-c7fbf7307ec17a5b  script-e836ce8ccbde9e79
script-4d1c98669ed66fd6  script-5fceb63b378a5dfc  script-9bec0282f392db27  script-c79e51d570cd5f5a  script-e266d49a4b608c66
```
In this example, each venv adds ~3 MB, this quickly adds up to a problematic environments-v2 folder in my use case.

# Proposed resolution
If the observed behaviour is desired, maybe the wording in the documentation could be changed to something like "**disposable** environment", as it doesn't disappear once execution is over. One could then think about maybe adding an option to have the disposable environment cleaned up after execution, e.g. `uv run --ephemeral-environment script.py`, or adding an option to clean the cached environments, e.g. `uv cache --environments clean`. Alternatively, there may be a different recommended way to deal with my use case.

If the behaviour is not desired, automatic deletion of the environment once execution finishes would be a solution.

### Platform

Ubuntu 20.04.6 LTS

### Version

uv 0.6.17

### Python version

3.10.0

---

_Label `bug` added by @engpas on 2025-05-13 13:55_

---

_Comment by @konstin on 2025-05-13 14:13_

When we say ephemeral, we mean that you can't rely on this environment being there when you check the next time. uv may remove ephemeral venvs, and they may be in a new location next time. You can also remove them by clearing the user cache directory (and uv will recreate it). For script environments specifically, we're currently in a transition to "more real" environments with stable paths to help IDEs and other tools interacting with script metadata. The docs will be updated when we finalize that.

> In my (HPC) use case, everything is organized in projects (not in the uv sense). Per project, I run the same script in hundreds to thousands of different folders. There is no upper limit on the number of projects.

For this use case, I would recommend building a venv and reusing it. You can use `uv sync --script` to install the dependencies for a script in an environment. Inline script metadata is targeted at having a bunch of helper script in a project or on a machine, and potentially running it often, but isn't meant to hundreds of thousands of scripts.

---

_Label `bug` removed by @konstin on 2025-05-13 14:13_

---

_Label `question` added by @konstin on 2025-05-13 14:13_

---

_Comment by @zanieb on 2025-05-13 14:15_

I'm surprised the script environments use that much space despite the hard links. Do you know what's actually consuming space there?

This is related to https://github.com/astral-sh/uv/issues/5731 — which probably captures your need.


---

_Referenced in [ansys/pyhps#619](../../ansys/pyhps/pulls/619.md) on 2025-06-30 10:07_

---
