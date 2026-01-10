```yaml
number: 9701
title: Explaining odd behavior with dependency locking and installation.
type: issue
state: closed
author: NellyWhads
labels: []
assignees: []
created_at: 2024-12-07T02:40:27Z
updated_at: 2024-12-10T15:57:26Z
url: https://github.com/astral-sh/uv/issues/9701
synced_at: 2026-01-10T04:36:21Z
```

# Explaining odd behavior with dependency locking and installation.

---

_Issue opened by @NellyWhads on 2024-12-07 02:40_

When using the following pyproject.toml, I get some weird behavior:

<details><summary>pyptoject.toml</summary>
<p>

```toml
[project]
name = "dep-test"
version = "0.1.0"
requires-python = ">=3.8,<3.13"

[tool.uv]
environments = ["platform_system == 'Linux'", "platform_system == 'Darwin'"]
# Conflicting groups specified at the project level
conflicts = [[{ group = "mmcv21_torch21" }, { group = "mmcv22_torch24" }]]
# Repo-wide constraints
constraint-dependencies = [
  "torch>=2.1,<2.5",
  "torchvision>=0.16,<0.20",
  "mmcv>=2.1,<2.3",
]

[dependency-groups]
my_group = ["torch", "torchvision"]
mmcv21_torch21 = [
  "mmcv~=2.1.0 ; platform_system == 'Linux' and python_version >= '3.8' and python_version < '3.12'",
  # Specific dependencies which are not encoded in the wheel
  "torch~=2.1.0",
  "torchvision~=0.16.0",
  "setuptools",
]
mmcv22_torch24 = [
  "mmcv~=2.2.0 ; platform_system == 'Linux' and python_version >= '3.8' and python_version < '3.13'",
  # Specific dependencies which are not encoded in the wheel
  "torch~=2.4.0",
  "torchvision~=0.19.0",
  "setuptools",
]

[tool.uv.sources]
torch = [{ index = "torch-cu118", marker = "platform_system == 'Linux'" }]
torchvision = [{ index = "torch-cu118", marker = "platform_system == 'Linux'" }]
mmcv = [
  # mmcv 2.1.0, torch 2.1.x
  { group = "mmcv21_torch21", marker = "platform_system == 'Linux' and python_version >= '3.8' and python_version < '3.9'", url = "https://download.openmmlab.com/mmcv/dist/cu118/torch2.1.0/mmcv-2.1.0-cp38-cp38-manylinux1_x86_64.whl" },
  { group = "mmcv21_torch21", marker = "platform_system == 'Linux' and python_version >= '3.9' and python_version < '3.10'", url = "https://download.openmmlab.com/mmcv/dist/cu118/torch2.1.0/mmcv-2.1.0-cp39-cp39-manylinux1_x86_64.whl" },
  { group = "mmcv21_torch21", marker = "platform_system == 'Linux' and python_version >= '3.10' and python_version < '3.11'", url = "https://download.openmmlab.com/mmcv/dist/cu118/torch2.1.0/mmcv-2.1.0-cp310-cp310-manylinux1_x86_64.whl" },
  { group = "mmcv21_torch21", marker = "platform_system == 'Linux' and python_version >= '3.11' and python_version < '3.12'", url = "https://download.openmmlab.com/mmcv/dist/cu118/torch2.1.0/mmcv-2.1.0-cp311-cp311-manylinux1_x86_64.whl" },
  # mmcv 2.2.0, torch 2.4.x
  { group = "mmcv22_torch24", marker = "platform_system == 'Linux' and python_version >= '3.8' and python_version < '3.9'", url = "https://download.openmmlab.com/mmcv/dist/cu118/torch2.4.0/mmcv-2.2.0-cp38-cp38-manylinux1_x86_64.whl" },
  { group = "mmcv22_torch24", marker = "platform_system == 'Linux' and python_version >= '3.9' and python_version < '3.10'", url = "https://download.openmmlab.com/mmcv/dist/cu118/torch2.4.0/mmcv-2.2.0-cp39-cp39-manylinux1_x86_64.whl" },
  { group = "mmcv22_torch24", marker = "platform_system == 'Linux' and python_version >= '3.10' and python_version < '3.11'", url = "https://download.openmmlab.com/mmcv/dist/cu118/torch2.4.0/mmcv-2.2.0-cp310-cp310-manylinux1_x86_64.whl" },
  { group = "mmcv22_torch24", marker = "platform_system == 'Linux' and python_version >= '3.11' and python_version < '3.12'", url = "https://download.openmmlab.com/mmcv/dist/cu118/torch2.4.0/mmcv-2.2.0-cp311-cp311-manylinux1_x86_64.whl" },
  { group = "mmcv22_torch24", marker = "platform_system == 'Linux' and python_version >= '3.12' and python_version < '3.13'", url = "https://download.openmmlab.com/mmcv/dist/cu118/torch2.4.0/mmcv-2.2.0-cp312-cp312-manylinux1_x86_64.whl" },
]

[[tool.uv.index]]
name = "torch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true
```

</p>
</details> 

To verify everything is as expected, I run `uv sync --group my_group && uv pip show torch torchvision`

I'm befuddled with the outcome.

<details><summary>Installed versions on MacOS</summary>
<p>

```
Name: torch
Version: 2.1.2
Location: /Users/neil.wadhvana/workspaces/main/torc-robotics/pytorc/projects/dep_test/.venv/lib/python3.10/site-packages
Requires: filelock, fsspec, jinja2, networkx, sympy, typing-extensions
Required-by: torchvision
---
Name: torch
Version: 2.4.1
Location: /Users/neil.wadhvana/workspaces/main/torc-robotics/pytorc/projects/dep_test/.venv/lib/python3.10/site-packages
Requires: filelock, fsspec, jinja2, networkx, sympy, typing-extensions
Required-by: torchvision
---
Name: torchvision
Version: 0.16.2
Location: /Users/neil.wadhvana/workspaces/main/torc-robotics/pytorc/projects/dep_test/.venv/lib/python3.10/site-packages
Requires: numpy, pillow, torch
Required-by:
---
Name: torchvision
Version: 0.19.1
Location: /Users/neil.wadhvana/workspaces/main/torc-robotics/pytorc/projects/dep_test/.venv/lib/python3.10/site-packages
Requires: numpy, pillow, torch
Required-by:
```

</p>
</details> 

- Why (and how) did `uv` install 2 versions of `torch` and `torchvision` on MacOS? Or is this just a readout from the lock file?
   - In the `.venv` site-packages, I only see one installation of each package (the newer versions).
   - My intuition tells me `uv pip show` is both parsing the lock file for versions and also reading from the environment, and somehow getting confused.
   - Running the `show` without installing / after cleaning up the venv yields only the observed, installed versions.
- I can solve  this behavior by specifying the Linux platform for `torch` and `torchvision` in the `mmcv*` groups, but I don't yet understand why it's necessary - should it be?

I understand I'm not using the [exact prescribed installation setup described in the docs](https://docs.astral.sh/uv/guides/integration/pytorch/#installing-pytorch), but I'm trying to understand the tool's behavior instead of just hard-coding a solution.


---

_Comment by @charliermarsh on 2024-12-07 02:42_

I think you're running into a variety of bugs that are fixed by https://github.com/astral-sh/uv/pull/9370. Check out the linked issues there.

---

_Comment by @NellyWhads on 2024-12-07 02:45_

Is there a quick way to install uv directly from that branch to test it?

_Also, are you an agentic LLM? Or are your fingers just crazy fast?_

---

_Comment by @zanieb on 2024-12-07 02:49_

There are a limited set of binary artifacts attached to the CI run for the commit https://github.com/astral-sh/uv/actions/runs/12204703226?pr=9370 (scroll to the bottom) — you can download one of those if you don't want to  build from source.

_We're all robots_

---

_Comment by @NellyWhads on 2024-12-07 03:01_

<details><summary>You've done it again</summary>
<p>

```shell
$ ~/Downloads/uv sync --group my_group && ~/Downloads/uv pip show torch torchvision 
Using CPython 3.10.15
Creating virtual environment at: .venv
⠙ Resolving dependencies...                                                                                                                    warning: Missing version constraint (e.g., a lower bound) for `setuptools`
warning: Missing version constraint (e.g., a lower bound) for `torch`
warning: Missing version constraint (e.g., a lower bound) for `torchvision`
Resolved 73 packages in 1.56s
Installed 12 packages in 256ms
 + filelock==3.16.1
 + fsspec==2024.10.0
 + jinja2==3.1.4
 + markupsafe==2.1.5
 + mpmath==1.3.0
 + networkx==3.1
 + numpy==1.24.4
 + pillow==10.4.0
 + sympy==1.13.3
 + torch==2.4.1
 + torchvision==0.19.1
 + typing-extensions==4.12.2
Name: torch
Version: 2.4.1
Location: /Users/neil.wadhvana/workspaces/main/torc-robotics/pytorc/projects/dep_test/.venv/lib/python3.10/site-packages
Requires: filelock, fsspec, jinja2, networkx, sympy, typing-extensions
Required-by: torchvision
---
Name: torchvision
Version: 0.19.1
Location: /Users/neil.wadhvana/workspaces/main/torc-robotics/pytorc/projects/dep_test/.venv/lib/python3.10/site-packages
Requires: numpy, pillow, torch
Required-by:
```

</p>
</details> 

_Teach me your ways. The world is moving too slowly for me._

---

_Closed by @BurntSushi on 2024-12-10 15:57_

---

_Closed by @BurntSushi on 2024-12-10 15:57_

---
