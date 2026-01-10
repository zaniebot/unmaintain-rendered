```yaml
number: 2477
title: "Bug: Prioritize newest manylinux platform tags"
type: issue
state: closed
author: bdice
labels:
  - bug
  - resolver
assignees: []
created_at: 2024-03-15T17:27:57Z
updated_at: 2024-03-17T16:04:34Z
url: https://github.com/astral-sh/uv/issues/2477
synced_at: 2026-01-10T05:40:32Z
```

# Bug: Prioritize newest manylinux platform tags

---

_Issue opened by @bdice on 2024-03-15 17:27_

Hi! I'm just trying `uv` for the first time. I'm impressed so far, keep up the great work.

I ran into an issue where `uv pip install` seems to be picking the wrong manylinux tag. Some packages like `pyarrow` have multiple `manylinux` tags available, such as:

```
pyarrow-15.0.1-cp312-cp312-manylinux_2_28_x86_64.whl
pyarrow-15.0.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
```

These wheels are built with different CXX11 ABI settings, so their libraries' symbols are different. This leads to ABI symbol conflicts for packages built against a particular manylinux tag of pyarrow.

pip prefers installing the latest manylinux tag that is supported by the system's version of glibc.

**Platform info:**
```bash
$ cat /etc/lsb-release | grep DESCRIPTION
DISTRIB_DESCRIPTION="Ubuntu 20.04.6 LTS"
$ # Show the system's glibc version
$ ldd --version | grep GLIBC
ldd (Ubuntu GLIBC 2.31-0ubuntu9.14) 2.31
```

**uv version:** 0.1.21 (latest at time of writing)

uv uses `manylinux_2_17` wheels even though it should use `manylinux_2_28` (because the system's glibc is `>=2.28`). I had to add `-v` to show the manylinux tags that it uses, since this is not shown in the normal output.
```
$ uv pip install -v pyarrow
DEBUG Found a virtualenv through CONDA_PREFIX at: /home/nfs/bdice/mambaforge/envs/uv
DEBUG Cached interpreter info for Python 3.11.8, skipping probing: /home/nfs/bdice/mambaforge/envs/uv/bin/python
DEBUG Using Python 3.11.8 environment at [36m/home/nfs/bdice/mambaforge/envs/uv/bin/python[39m
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.11.8
DEBUG Adding direct dependency: pyarrow*
DEBUG Found fresh response for: https://pypi.org/simple/pyarrow/
DEBUG Searching for a compatible version of pyarrow (*)
DEBUG Selecting: pyarrow==15.0.1 (pyarrow-15.0.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a7/d0/80e390b1785c02cb3effff61032595337575248f712033d4aa6f951c9502/pyarrow-15.0.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Adding transitive dependency: numpy>=1.16.6, <2
DEBUG Found fresh response for: https://pypi.org/simple/numpy/
DEBUG Searching for a compatible version of numpy (>=1.16.6, <2)
DEBUG Selecting: numpy==1.26.4 (numpy-1.26.4-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3a/d0/edc009c27b406c4f9cbc79274d6e46d634d139075492ad055e3d68445925/numpy-1.26.4-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
Resolved 2 packages in 11ms
DEBUG Requirement already cached: numpy==1.26.4
DEBUG Requirement already cached: pyarrow==15.0.1
DEBUG Preserving seed package: wheel==0.42.0
Installed 2 packages in 299ms
 + numpy==1.26.4
 + pyarrow==15.0.1
```

For comparison, pip uses `manylinux_2_28` wheels as desired:
```
$ pip install pyarrow
Collecting pyarrow
  Using cached pyarrow-15.0.1-cp311-cp311-manylinux_2_28_x86_64.whl.metadata (3.0 kB)
Collecting numpy<2,>=1.16.6 (from pyarrow)
  Using cached numpy-1.26.4-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (61 kB)
Using cached pyarrow-15.0.1-cp311-cp311-manylinux_2_28_x86_64.whl (38.3 MB)
Using cached numpy-1.26.4-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (18.3 MB)
Installing collected packages: numpy, pyarrow
Successfully installed numpy-1.26.4 pyarrow-15.0.1
```

### Additional Context
I ran into this issue because of a notebook shared by @raybellwaves using `uv` to install RAPIDS cuDF in Google Colab, which depends on pyarrow. There is a mismatch in manylinux versions when pyarrow is installed by `uv`. I believe this is because the user-installed version uses `manylinux_2_17` which conflicts with Colab's pre-installed pyarrow packages which appear to use `manylinux_2_28`.

---

_Label `bug` added by @zanieb on 2024-03-15 17:31_

---

_Label `resolver` added by @zanieb on 2024-03-15 17:31_

---

_Comment by @zanieb on 2024-03-15 17:31_

Thanks for the report! I'm surprised we don't do this but it seems like an easy oversight too

cc @konstin 

---

_Comment by @charliermarsh on 2024-03-15 17:51_

We’re probably just pushing the tags in version order, but we should be iterating in the reverse order.

---

_Comment by @zanieb on 2024-03-15 20:52_

I've written a test case demonstrating this in #2482 and a fix is up at https://github.com/astral-sh/uv/pull/2483 — it'd be nice to have coverage for this in a resolver test too.

---

_Closed by @zanieb on 2024-03-16 22:33_

---

_Comment by @raybellwaves on 2024-03-17 16:04_

many (linux) thanks!

---
