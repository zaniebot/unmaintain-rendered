---
number: 2538
title: "\"No solution found when resolving dependencies\", but pip installs readily"
type: issue
state: closed
author: reece
labels:
  - question
assignees: []
created_at: 2024-03-19T03:25:27Z
updated_at: 2024-07-28T00:49:30Z
url: https://github.com/astral-sh/uv/issues/2538
synced_at: 2026-01-10T01:23:19Z
---

# "No solution found when resolving dependencies", but pip installs readily

---

_Issue opened by @reece on 2024-03-19 03:25_

Summary: uv pip install fails to install a package that pip installs without issue. The package is an alpha release that I pushed to pypi a few minutes ago.


* A minimal code snippet that reproduces the bug.

```
$ cd /tmp

$ python3 -m venv venv

$ source venv/bin/activate

$ pip install -U setuptools pip uv

(venv) $ uv pip install --verbose uta-align==0.3.0a5
DEBUG Found a virtualenv through VIRTUAL_ENV at: /tmp/x/venv
DEBUG Cached interpreter info for Python 3.11.6, skipping probing: /tmp/x/venv/bin/python
DEBUG Using Python 3.11.6 environment at /tmp/x/venv/bin/python
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.11.6
DEBUG Adding direct dependency: uta-align==0.3.0a5
DEBUG Found fresh response for: https://pypi.org/simple/uta-align/
DEBUG Searching for a compatible version of uta-align (==0.3.0a5)
DEBUG No compatible version found for: uta-align
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of uta-align==0.3.0a5 and you require uta-align==0.3.0a5, we can conclude that the requirements are unsatisfiable.

(venv) $ pip install uta-align==0.3.0a5
Looking in indexes: https://pypi.shr.myome.info/simple
Collecting uta-align==0.3.0a5
  Using cached uta_align-0.3.0a5-cp311-cp311-linux_x86_64.whl
Collecting coloredlogs~=15.0 (from uta-align==0.3.0a5)
  Using cached coloredlogs-15.0.1-py2.py3-none-any.whl.metadata (12 kB)
Collecting pysam~=0.22 (from uta-align==0.3.0a5)
  Using cached pysam-0.22.0-cp311-cp311-manylinux_2_28_x86_64.whl.metadata (1.5 kB)
Collecting pyyaml~=6.0 (from uta-align==0.3.0a5)
  Using cached PyYAML-6.0.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (2.1 kB)
Collecting humanfriendly>=9.1 (from coloredlogs~=15.0->uta-align==0.3.0a5)
  Using cached humanfriendly-10.0-py2.py3-none-any.whl.metadata (9.2 kB)
Using cached coloredlogs-15.0.1-py2.py3-none-any.whl (46 kB)
Using cached pysam-0.22.0-cp311-cp311-manylinux_2_28_x86_64.whl (25.1 MB)
Using cached PyYAML-6.0.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (757 kB)
Using cached humanfriendly-10.0-py2.py3-none-any.whl (86 kB)
Installing collected packages: pyyaml, pysam, humanfriendly, coloredlogs, uta-align
Successfully installed coloredlogs-15.0.1 humanfriendly-10.0 pysam-0.22.0 pyyaml-6.0.1 uta-align-0.3.0a5

```


* The current uv platform & version

```
(venv) snafu$ uname -a
Linux snafu 6.5.0-26-generic #26-Ubuntu SMP PREEMPT_DYNAMIC Tue Mar  5 21:19:28 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux

(venv) snafu$ uv --version
uv 0.1.22

```


---

_Comment by @reece on 2024-03-19 03:27_

Well, of course I tried again after creating the issue and it worked immediately. I conclude that uv caches results with a TTL of something like 5-10 minutes. Is that right?

---

_Comment by @zanieb on 2024-03-19 03:37_

Yep there's a 10 minute cache, I'd recommend using `--refresh-package <name>` if you're testing something you just published. Some more context at https://github.com/astral-sh/uv/issues/505

---

_Label `question` added by @zanieb on 2024-03-19 03:37_

---

_Comment by @reece on 2024-03-19 03:51_

Fabulous. I'm happy with the choice. 

Two comments/suggestions/questions (and I'm happy to help):

1) "Because there is no version of uta-align==0.3.0a5 and you require uta-align==0.3.0a5, we can conclude that the requirements are unsatisfiable." seems to assert a stronger condition than is warranted. How about something more like "uta-align==0.3.0a5 could not be satisfied; consider using --refresh to update known versions". Or, if --refresh was used, then drop the second clause.

2) How about assuming --refresh for non-production releases?

---

_Comment by @zanieb on 2024-03-19 14:37_

(1) "Could not be satisfied" has a bit more of a complex meaning, like "this version exists but its requirements cannot be reconciled with the solution". I'm pretty hesitant to change the wording there since in most cases the version actually will not exist. We could do something like.. find all the versions that "do not exist" in a resolution tree and check if it exists with caching disabled then add hints or mutate the message in that case. This would slow down display of the error report but we worry less about performance once you're in an error case.

(2) I worry changing the behavior based on version specifiers, it seems confusing. A simple policy is my preference.


---

_Comment by @yashgorana on 2024-03-19 16:56_

Same when installing torch

```
~ # uv version
uv 0.1.22

~ # uv venv
Using Python 3.11.8 interpreter at: /usr/bin/python3
Creating virtualenv at: .venv

~ # uv pip install torch==2.2.0 --index-url https://download.pytorch.org/whl/cpu
  x No solution found when resolving dependencies:
  `-> Because torch==2.2.0 is unusable because no wheels are available with a matching Python implementation and you require torch==2.2.0, we can conclude that the requirements are unsatisfiable.
```

Now the same with pip
```
~ # python -m venv pyvenv
~ # source pyvenv/bin/activate
(pyvenv) ~ # pip install torch==2.2.0 --index-url https://download.pytorch.org/whl/cpu
Looking in indexes: https://download.pytorch.org/whl/cpu
Collecting torch==2.2.0
  Downloading https://download.pytorch.org/whl/cpu/torch-2.2.0%2Bcpu-cp311-cp311-linux_x86_64.whl (186.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 186.8/186.8 MB 22.9 MB/s eta 0:00:00
Collecting filelock (from torch==2.2.0)
  Downloading https://download.pytorch.org/whl/filelock-3.9.0-py3-none-any.whl (9.7 kB)
Collecting typing-extensions>=4.8.0 (from torch==2.2.0)
  Downloading https://download.pytorch.org/whl/typing_extensions-4.8.0-py3-none-any.whl (31 kB)
Collecting sympy (from torch==2.2.0)
  Downloading https://download.pytorch.org/whl/sympy-1.12-py3-none-any.whl (5.7 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 5.7/5.7 MB 32.2 MB/s eta 0:00:00
...
```

uv needs to be explicitly provided with `uv pip install torch==2.2.0+cpu ...` for it to correctly pick up the wheel.

---

_Comment by @charliermarsh on 2024-03-19 16:56_

@yashgorana - I think that's unrelated, and covered in the pip compatibility guide: https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#local-version-identifiers

---

_Comment by @yashgorana on 2024-03-19 16:58_

Aha - thank you for pointing to the docs!

---

_Comment by @charliermarsh on 2024-03-19 17:01_

Np. The local version support is relatively new. There's also an open issue here to track improving it further to remove the existing limitations: #2541

---

_Comment by @marcelotduarte on 2024-05-22 22:12_

I got the same error, in a different situation, using Linux. 
`uv pip install -i https://pypi.anaconda.org/intel/simple numpy`
From the error message, I assume that uv does not recognize manylinux packages when using --index-url.



---

_Comment by @charliermarsh on 2024-05-22 22:56_

uv does recognize manylinux packages when using `--index-url` and other sources -- we would have much bigger problems if we didn't! Unfortunately I'd need more information to understand the problem, e.g., the output of running with `--verbose`, your platform, your Python version.

---

_Comment by @marcelotduarte on 2024-05-22 23:26_

```
lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 22.04.4 LTS
Release:	22.04
Codename:	jammy
```
```
uv pip install -i https://pypi.anaconda.org/intel/simple numpy --verbose
INFO Found a virtualenv through VIRTUAL_ENV at: /home/marcelo/.local/venv/cx310tests
DEBUG Cached interpreter info for Python 3.10.12, skipping probing: /home/marcelo/.local/venv/cx310tests/bin/python
DEBUG Using Python 3.10.12 environment at /home/marcelo/.local/venv/cx310tests/bin/python
DEBUG Trying to lock if free: /home/marcelo/.local/venv/cx310tests/.lock
DEBUG At least one requirement is not satisfied: numpy
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.10.12
DEBUG Adding direct dependency: numpy*
DEBUG Found fresh response for: https://pypi.anaconda.org/intel/simple/numpy/
DEBUG Searching for a compatible version of numpy (*)
DEBUG Selecting: numpy==1.26.4 (numpy-1.26.4-1-cp310-cp310-manylinux2014_x86_64.whl)
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/numpy/1.26.4/numpy-1.26.4-1-cp310-cp310-manylinux2014_x86_64.whl
WARN Range requests not supported for numpy-1.26.4-cp310-cp310-manylinux2014_x86_64.whl; streaming wheel
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/numpy/1.26.4/numpy-1.26.4-1-cp310-cp310-manylinux2014_x86_64.whl
DEBUG Adding transitive dependency for numpy==1.26.4: mkl-fft*
DEBUG Adding transitive dependency for numpy==1.26.4: mkl-random*
DEBUG Adding transitive dependency for numpy==1.26.4: mkl-umath*
DEBUG Adding transitive dependency for numpy==1.26.4: mkl*
DEBUG Adding transitive dependency for numpy==1.26.4: tbb4py*
DEBUG Adding transitive dependency for numpy==1.26.4: mkl-service*
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl-fft/
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl-random/
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl/
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/tbb4py/
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl-service/
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl-umath/
DEBUG Searching for a compatible version of mkl-fft (*)
DEBUG Selecting: mkl-fft==1.3.8 (mkl_fft-1.3.8-63-cp310-cp310-manylinux2014_x86_64.whl)
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl-fft/1.3.8/mkl_fft-1.3.8-63-cp310-cp310-manylinux2014_x86_64.whl
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl-random/1.2.4/mkl_random-1.2.4-83-cp310-cp310-manylinux2014_x86_64.whl
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl/2024.1.0/mkl-2024.1.0-py2.py3-none-manylinux1_x86_64.whl
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl-umath/0.1.1/mkl_umath-0.1.1-100-cp310-cp310-manylinux2014_x86_64.whl
WARN Range requests not supported for mkl_random-1.2.4-cp310-cp310-manylinux2014_x86_64.whl; streaming wheel
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl-random/1.2.4/mkl_random-1.2.4-83-cp310-cp310-manylinux2014_x86_64.whl
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/tbb4py/2021.12.0/tbb4py-2021.12.0-py310-none-manylinux1_x86_64.whl
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl-service/2.4.1/mkl_service-2.4.1-0-cp310-cp310-manylinux2014_x86_64.whl
WARN Range requests not supported for mkl_umath-0.1.1-cp310-cp310-manylinux2014_x86_64.whl; streaming wheel
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl-umath/0.1.1/mkl_umath-0.1.1-100-cp310-cp310-manylinux2014_x86_64.whl
WARN Range requests not supported for mkl-2024.1.0-py2.py3-none-manylinux1_x86_64.whl; streaming wheel
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl/2024.1.0/mkl-2024.1.0-py2.py3-none-manylinux1_x86_64.whl
WARN Range requests not supported for tbb4py-2021.12.0-py310-none-manylinux1_x86_64.whl; streaming wheel
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/tbb4py/2021.12.0/tbb4py-2021.12.0-py310-none-manylinux1_x86_64.whl
WARN Range requests not supported for mkl_fft-1.3.8-cp310-cp310-manylinux2014_x86_64.whl; streaming wheel
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl-fft/1.3.8/mkl_fft-1.3.8-63-cp310-cp310-manylinux2014_x86_64.whl
WARN Range requests not supported for mkl_service-2.4.1-cp310-cp310-manylinux2014_x86_64.whl; streaming wheel
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl-service/2.4.1/mkl_service-2.4.1-0-cp310-cp310-manylinux2014_x86_64.whl
DEBUG Adding transitive dependency for mkl-fft==1.3.8: numpy>=1.24.4, <1.25.0
DEBUG Adding transitive dependency for mkl-fft==1.3.8: mkl*
DEBUG Searching for a compatible version of mkl-fft (<1.3.8 | >1.3.8)
DEBUG Selecting: mkl-fft==1.3.6 (mkl_fft-1.3.6-58-cp310-cp310-manylinux2014_x86_64.whl)
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl-fft/1.3.6/mkl_fft-1.3.6-58-cp310-cp310-manylinux2014_x86_64.whl
WARN Range requests not supported for mkl_fft-1.3.6-cp310-cp310-manylinux2014_x86_64.whl; streaming wheel
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/mkl-fft/1.3.6/mkl_fft-1.3.6-58-cp310-cp310-manylinux2014_x86_64.whl
DEBUG Adding transitive dependency for mkl-fft==1.3.6: numpy>=1.24.3, <1.25.0
DEBUG Adding transitive dependency for mkl-fft==1.3.6: mkl*
DEBUG Adding transitive dependency for mkl-fft==1.3.6: dpcpp-cpp-rt*
DEBUG Searching for a compatible version of mkl-fft (<1.3.6 | >1.3.6, <1.3.8 | >1.3.8)
DEBUG No compatible version found for: mkl-fft
DEBUG Searching for a compatible version of numpy (<1.26.4 | >1.26.4)
DEBUG Selecting: numpy==1.24.4 (numpy-1.24.4-1-cp310-cp310-manylinux2014_x86_64.whl)
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/numpy/1.24.4/numpy-1.24.4-1-cp310-cp310-manylinux2014_x86_64.whl
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/dpcpp-cpp-rt/
WARN Range requests not supported for numpy-1.24.4-cp310-cp310-manylinux2014_x86_64.whl; streaming wheel
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/numpy/1.24.4/numpy-1.24.4-1-cp310-cp310-manylinux2014_x86_64.whl
DEBUG Adding transitive dependency for numpy==1.24.4: mkl-fft*
DEBUG Adding transitive dependency for numpy==1.24.4: mkl-random*
DEBUG Adding transitive dependency for numpy==1.24.4: mkl-umath*
DEBUG Adding transitive dependency for numpy==1.24.4: mkl*
DEBUG Adding transitive dependency for numpy==1.24.4: tbb4py*
DEBUG Adding transitive dependency for numpy==1.24.4: mkl-service*
DEBUG Searching for a compatible version of mkl-fft (==1.3.6 | ==1.3.8)
DEBUG Selecting: mkl-fft==1.3.8 (mkl_fft-1.3.8-63-cp310-cp310-manylinux2014_x86_64.whl)
DEBUG Searching for a compatible version of mkl-random (*)
DEBUG Selecting: mkl-random==1.2.4 (mkl_random-1.2.4-83-cp310-cp310-manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for mkl-random==1.2.4: numpy>=1.24.4, <1.25.0
DEBUG Adding transitive dependency for mkl-random==1.2.4: mkl*
DEBUG Searching for a compatible version of mkl-umath (*)
DEBUG Selecting: mkl-umath==0.1.1 (mkl_umath-0.1.1-100-cp310-cp310-manylinux2014_x86_64.whl)
DEBUG Adding transitive dependency for mkl-umath==0.1.1: numpy>=1.26.4, <1.27.0
DEBUG Adding transitive dependency for mkl-umath==0.1.1: mkl*
DEBUG Adding transitive dependency for mkl-umath==0.1.1: intel-cmplr-lib-rt*
DEBUG Searching for a compatible version of mkl-umath (<0.1.1 | >0.1.1)
DEBUG No compatible version found for: mkl-umath
DEBUG Searching for a compatible version of numpy (<1.24.4 | >1.24.4, <1.26.4 | >1.26.4)
DEBUG Selecting: numpy==1.24.3 (numpy-1.24.3-1-cp310-cp310-manylinux2014_x86_64.whl)
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/intel-cmplr-lib-rt/
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/numpy/1.24.3/numpy-1.24.3-1-cp310-cp310-manylinux2014_x86_64.whl
WARN Range requests not supported for numpy-1.24.3-cp310-cp310-manylinux2014_x86_64.whl; streaming wheel
DEBUG No cache entry for: https://pypi.anaconda.org/intel/simple/numpy/1.24.3/numpy-1.24.3-1-cp310-cp310-manylinux2014_x86_64.whl
DEBUG Adding transitive dependency for numpy==1.24.3: mkl-fft*
DEBUG Adding transitive dependency for numpy==1.24.3: mkl-random*
DEBUG Adding transitive dependency for numpy==1.24.3: mkl-umath*
DEBUG Adding transitive dependency for numpy==1.24.3: dpcpp-cpp-rt*
DEBUG Adding transitive dependency for numpy==1.24.3: mkl*
DEBUG Adding transitive dependency for numpy==1.24.3: tbb4py*
DEBUG Adding transitive dependency for numpy==1.24.3: mkl-service*
DEBUG Searching for a compatible version of numpy (<1.24.3 | >1.24.3, <1.24.4 | >1.24.4, <1.26.4 | >1.26.4)
DEBUG Searching for a compatible version of numpy (<1.22.3 | >1.22.3, <1.24.3 | >1.24.3, <1.24.4 | >1.24.4, <1.26.4 | >1.26.4)
DEBUG Searching for a compatible version of numpy (<1.21.4 | >1.21.4, <1.22.3 | >1.22.3, <1.24.3 | >1.24.3, <1.24.4 | >1.24.4, <1.26.4 | >1.26.4)
DEBUG Searching for a compatible version of numpy (<1.21.2 | >1.21.2, <1.21.4 | >1.21.4, <1.22.3 | >1.22.3, <1.24.3 | >1.24.3, <1.24.4 | >1.24.4, <1.26.4 | >1.26.4)
DEBUG Searching for a compatible version of numpy (<1.20.3 | >1.20.3, <1.21.2 | >1.21.2, <1.21.4 | >1.21.4, <1.22.3 | >1.22.3, <1.24.3 | >1.24.3, <1.24.4 | >1.24.4, <1.26.4 | >1.26.4)
DEBUG Searching for a compatible version of numpy (<1.20.2 | >1.20.2, <1.20.3 | >1.20.3, <1.21.2 | >1.21.2, <1.21.4 | >1.21.4, <1.22.3 | >1.22.3, <1.24.3 | >1.24.3, <1.24.4 | >1.24.4, <1.26.4 | >1.26.4)
DEBUG Searching for a compatible version of numpy (<1.20.1 | >1.20.1, <1.20.2 | >1.20.2, <1.20.3 | >1.20.3, <1.21.2 | >1.21.2, <1.21.4 | >1.21.4, <1.22.3 | >1.22.3, <1.24.3 | >1.24.3, <1.24.4 | >1.24.4, <1.26.4 | >1.26.4)
DEBUG Searching for a compatible version of numpy (<1.20.0 | >1.20.0, <1.20.1 | >1.20.1, <1.20.2 | >1.20.2, <1.20.3 | >1.20.3, <1.21.2 | >1.21.2, <1.21.4 | >1.21.4, <1.22.3 | >1.22.3, <1.24.3 | >1.24.3, <1.24.4 | >1.24.4, <1.26.4 | >1.26.4)
DEBUG Searching for a compatible version of numpy (<1.19.5 | >1.19.5, <1.20.0 | >1.20.0, <1.20.1 | >1.20.1, <1.20.2 | >1.20.2, <1.20.3 | >1.20.3, <1.21.2 | >1.21.2, <1.21.4 | >1.21.4, <1.22.3 | >1.22.3, <1.24.3 | >1.24.3, <1.24.4 | >1.24.4, <1.26.4 | >1.26.4)
DEBUG No compatible version found for: numpy
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of numpy are available:
          numpy==1.19.5
          numpy==1.20.0
          numpy==1.20.1
          numpy==1.20.2
          numpy==1.20.3
          numpy==1.21.2
          numpy==1.21.4
          numpy==1.22.3
          numpy==1.24.3
          numpy==1.24.4
          numpy==1.26.4
      and numpy==1.19.5 has no wheels are available with a matching Python ABI, we can conclude that numpy<1.20.0 cannot be used.
      And because numpy==1.20.0 has no wheels are available with a matching Python ABI, we can conclude that numpy<1.20.1 cannot be used.
      And because numpy==1.20.1 has no wheels are available with a matching Python ABI and numpy==1.20.2 has no wheels are available with
      a matching Python ABI, we can conclude that numpy<1.20.3 cannot be used.
      And because numpy==1.20.3 has no wheels are available with a matching Python ABI and numpy==1.21.2 has no wheels are available with
      a matching Python ABI, we can conclude that numpy<1.21.4 cannot be used.
      And because numpy==1.21.4 has no wheels are available with a matching Python ABI and numpy==1.22.3 has no wheels are available with
      a matching Python ABI, we can conclude that numpy<1.24.3 cannot be used. (1)

      Because only mkl-umath==0.1.1 is available and mkl-umath==0.1.1 depends on numpy>=1.26.4, we can conclude that all versions of
      mkl-umath depend on numpy>=1.26.4.
      And because numpy>=1.24.3 depends on mkl-umath, we can conclude that numpy>=1.24.3,<=1.24.4 cannot be used.
      And because we know from (1) that numpy<1.24.3 cannot be used, we can conclude that numpy<1.26.4 cannot be used. (2)

      Because only the following versions of mkl-fft are available:
          mkl-fft==1.3.6
          mkl-fft==1.3.8
      and mkl-fft==1.3.6 depends on numpy>=1.24.3,<1.25.0, we can conclude that mkl-fft<1.3.8 depends on numpy>=1.24.3,<1.25.0.
      And because mkl-fft==1.3.8 depends on numpy>=1.24.4,<1.25.0 and numpy==1.26.4 depends on mkl-fft, we can conclude that numpy==1.26.4
      cannot be used.
      And because we know from (2) that numpy<1.26.4 cannot be used, we can conclude that all versions of numpy cannot be used.
      And because you require numpy, we can conclude that the requirements are unsatisfiable.
```

---

_Comment by @charliermarsh on 2024-05-22 23:42_

And you can get this to work with pip?

---

_Comment by @charliermarsh on 2024-05-22 23:44_

A lot of this looks correct to me... You're using Python 3.10, and so the _only_ versions you're allowed to use from that index are 1.24.3, 1.24.4, and 1.26.4, because they don't publish Python 3.10 wheels for any other versions.


---

_Comment by @marcelotduarte on 2024-05-22 23:52_

> And you can get this to work with pip?

Yes. There are wheel of numpy in this URL only works for Python 3.10, and with pip it install correctly.
Below is an excerpt taken from [here](https://productionresultssa4.blob.core.windows.net/actions-results/72689d91-5692-410e-96cd-4ed0a70a464e/workflow-job-run-908f5f0e-46fa-59d3-58e5-08784ac67068/logs/job/job-logs.txt?rsct=text%2Fplain&se=2024-05-22T23%3A58%3A23Z&sig=mTAuqC6QK7a7ZQuMh06MIJqnsqH36srRYU0kprfivcU%3D&sp=r&spr=https&sr=b&st=2024-05-22T23%3A48%3A18Z&sv=2021-12-02):
```
2024-05-22T23:43:57.6456129Z [36;1m  pip install -i https://pypi.anaconda.org/intel/simple numpy[0m
...
2024-05-22T23:43:58.3875244Z Looking in indexes: https://pypi.anaconda.org/intel/simple
2024-05-22T23:43:58.7362316Z Collecting numpy
2024-05-22T23:43:59.1005954Z   Downloading https://pypi.anaconda.org/intel/simple/numpy/1.26.4/numpy-1.26.4-1-cp310-cp310-manylinux2014_x86_64.whl (6.9 MB)
2024-05-22T23:43:59.2047043Z      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 6.9/6.9 MB 68.0 MB/s eta 0:00:00
2024-05-22T23:43:59.3131822Z Collecting mkl_fft (from numpy)
2024-05-22T23:43:59.5575118Z   Downloading https://pypi.anaconda.org/intel/simple/mkl-fft/1.3.8/mkl_fft-1.3.8-70-cp310-cp310-manylinux2014_x86_64.whl (4.1 MB)
2024-05-22T23:43:59.6612849Z      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.1/4.1 MB 40.1 MB/s eta 0:00:00
2024-05-22T23:43:59.7621888Z Collecting mkl_random (from numpy)
2024-05-22T23:43:59.9797680Z   Downloading https://pypi.anaconda.org/intel/simple/mkl-random/1.2.4/mkl_random-1.2.4-90-cp310-cp310-manylinux2014_x86_64.whl (4.2 MB)
2024-05-22T23:44:00.2130052Z      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 4.2/4.2 MB 18.1 MB/s eta 0:00:00
2024-05-22T23:44:00.3371021Z Collecting mkl_umath (from numpy)
2024-05-22T23:44:00.4734499Z   Downloading https://pypi.anaconda.org/intel/simple/mkl-umath/0.1.1/mkl_umath-0.1.1-100-cp310-cp310-manylinux2014_x86_64.whl (166 kB)
2024-05-22T23:44:00.4800378Z      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 166.2/166.2 kB 35.6 MB/s eta 0:00:00
2024-05-22T23:44:00.6635898Z Collecting mkl (from numpy)
2024-05-22T23:44:00.8930468Z   Downloading https://pypi.anaconda.org/intel/simple/mkl/2024.1.0/mkl-2024.1.0-py2.py3-none-manylinux1_x86_64.whl (202.0 MB)
2024-05-22T23:44:05.9492750Z      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 202.0/202.0 MB 14.3 MB/s eta 0:00:00
2024-05-22T23:44:06.3192224Z Collecting tbb4py (from numpy)
2024-05-22T23:44:06.4885416Z   Downloading https://pypi.anaconda.org/intel/simple/tbb4py/2021.12.0/tbb4py-2021.12.0-py310-none-manylinux1_x86_64.whl (324 kB)
2024-05-22T23:44:06.5317914Z      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 324.5/324.5 kB 7.6 MB/s eta 0:00:00
2024-05-22T23:44:06.6094997Z Collecting mkl-service (from numpy)
2024-05-22T23:44:06.7799475Z   Downloading https://pypi.anaconda.org/intel/simple/mkl-service/2.4.1/mkl_service-2.4.1-0-cp310-cp310-manylinux2014_x86_64.whl (75 kB)
2024-05-22T23:44:06.7847737Z      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 75.6/75.6 kB 23.5 MB/s eta 0:00:00
2024-05-22T23:44:08.3345836Z Collecting intel-openmp==2024.* (from mkl->numpy)
2024-05-22T23:44:08.5523459Z   Downloading https://pypi.anaconda.org/intel/simple/intel-openmp/2024.1.0/intel_openmp-2024.1.0-py2.py3-none-manylinux1_x86_64.whl (23.7 MB)
2024-05-22T23:44:08.8075350Z      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 23.7/23.7 MB 65.1 MB/s eta 0:00:00
2024-05-22T23:44:08.9822296Z Collecting tbb==2021.* (from mkl->numpy)
2024-05-22T23:44:09.2655745Z   Downloading https://pypi.anaconda.org/intel/simple/tbb/2021.12.0/tbb-2021.12.0-py2.py3-none-manylinux1_x86_64.whl (5.4 MB)
2024-05-22T23:44:09.3220323Z      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 5.4/5.4 MB 98.9 MB/s eta 0:00:00
2024-05-22T23:44:09.5826547Z Collecting intel-cmplr-lib-rt (from mkl_umath->numpy)
2024-05-22T23:44:09.7444666Z   Downloading https://pypi.anaconda.org/intel/simple/intel-cmplr-lib-rt/2024.1.0/intel_cmplr_lib_rt-2024.1.0-py2.py3-none-manylinux1_x86_64.whl (52.4 MB)
2024-05-22T23:44:10.4123798Z      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 52.4/52.4 MB 51.7 MB/s eta 0:00:00
2024-05-22T23:44:10.5537663Z Installing collected packages: tbb, intel-openmp, intel-cmplr-lib-rt, tbb4py, mkl, mkl-service, mkl_umath, mkl_random, mkl_fft, numpy
2024-05-22T23:44:16.9347703Z Successfully installed intel-cmplr-lib-rt-2024.1.0 intel-openmp-2024.1.0 mkl-2024.1.0 mkl-service-2.4.1 mkl_fft-1.3.8 mkl_random-1.2.4 mkl_umath-0.1.1 numpy-1.26.4 tbb-2021.12.0 tbb4py-2021.12.0
```

---

_Comment by @charliermarsh on 2024-05-23 00:04_

The problem is that they have multiple wheels for the same version with different metadata on that registry. For `mkl_fft==1.3.8`, they have two wheels:

- [mkl_fft-1.3.8-63-cp310-cp310-manylinux2014_x86_64.whl](https://pypi.anaconda.org/intel/simple/mkl-fft/1.3.8/mkl_fft-1.3.8-63-cp310-cp310-manylinux2014_x86_64.whl)
- [mkl_fft-1.3.8-70-cp310-cp310-manylinux2014_x86_64.whl](https://pypi.anaconda.org/intel/simple/mkl-fft/1.3.8/mkl_fft-1.3.8-70-cp310-cp310-manylinux2014_x86_64.whl)

The former requires `numpy>=1.24.4,<1.25.0`, the latter requires `Requires-Dist: numpy>=1.26.4,<1.27.0`. So the resolution isn't compatible if you use the former wheel.

We require metadata to be consistent across wheels. I guess we could try to select the wheel with the higher build number but I'm not sure that's standardized. I'll look.


---

_Comment by @charliermarsh on 2024-05-23 00:08_

It looks like it _is_ standardized so I'll create a separate issue for it: https://peps.python.org/pep-0427/#file-name-convention

---

_Referenced in [astral-sh/uv#3779](../../astral-sh/uv/issues/3779.md) on 2024-05-23 00:09_

---

_Comment by @charliermarsh on 2024-05-23 00:52_

Fixed in https://github.com/astral-sh/uv/pull/3781, thanks.

---

_Referenced in [astral-sh/uv#5351](../../astral-sh/uv/issues/5351.md) on 2024-07-23 17:15_

---

_Closed by @charliermarsh on 2024-07-28 00:49_

---

_Referenced in [astral-sh/uv#9923](../../astral-sh/uv/issues/9923.md) on 2025-01-01 01:10_

---
