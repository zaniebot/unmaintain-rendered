---
number: 16242
title: Is it possible to always prefer a local index?
type: issue
state: open
author: rabyj
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2025-10-10T22:39:06Z
updated_at: 2025-12-17T18:33:44Z
url: https://github.com/astral-sh/uv/issues/16242
synced_at: 2026-01-10T01:26:05Z
---

# Is it possible to always prefer a local index?

---

_Issue opened by @rabyj on 2025-10-10 22:39_

### Question

## Summary

My goal is to maintain a consistent preference for pre-compiled wheels from local indexes across all Python versions, falling back to PyPI only when no local wheel is available.

When using `index-strategy = "unsafe-first-match"`, uv selects packages from different indexes depending on the Python version, even though local indexes 'should' be consistently preferred (according to "Indexes are prioritized in the order in which they’re defined"). This appears to be related to wheel compatibility tags and possibly transitive dependency resolution.

## Expected Behavior

With `unsafe-first-match` index strategy, uv should consistently select `scikit-learn==1.2.1+computecanada` from local indexes across all Python versions where compatible wheels are available.

## Actual Behavior

- **Python 3.8/3.9**: Resolves to `scikit-learn==1.2.1` from PyPI (ignoring local index)
- **Python 3.10/3.11**: Resolves to `scikit-learn==1.2.1+computecanada` from local index (as expected)

## Reproduction

### Configuration

pyproject.toml:
```toml
[project]
name = "test"
version = "0"
requires-python = ">=3.8, <3.12"
dependencies = [
    "scikit-learn==1.2.1",
    "scikit-optimize @ git+https://github.com/QuentinSoubeyran/scikit-optimize.git@3f92bf42d947efb4ece3d89595d67dc56591c7c3"
]

[tool.uv.pip]
index-strategy = "unsafe-first-match"

# PATH1
[[tool.uv.index]]
name = "computecanada-generic"
url = "/cvmfs/soft.computecanada.ca/custom/python/wheelhouse/generic" # PATH1
format = "flat"

 # PATH2
[[tool.uv.index]]
name = "computecanada-gentoo-generic"
url = "/cvmfs/soft.computecanada.ca/custom/python/wheelhouse/gentoo/generic" # PATH2
format = "flat"

[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple"
```

### Available Wheels in Local Indexes
- **path1**: `scikit-learn==1.2.1+computecanada` with `cp310`, `cp311` compatibility tags
- **path2**: `scikit-learn==1.2.1+computecanada` with `cp38`, `cp39` compatibility tags

### Steps to Reproduce

1. Use the pyproject.toml.
2. Be in an environment with cvmfs (sorry I don't know how to reproduce the issue locally)
3. Run `uv pip compile --python 3.8 pyproject.toml` (or 3.9) → Results in PyPI version being selected
4. Run `uv pip compile --python 3.10 pyproject.toml` (or 3.11) → Results in local version being selected

## Attempted Workarounds

#### Using explicit local version and `unsafe-first-match`

Specifying `scikit-learn==1.2.1+computecanada` directly fails depending on index order, since the wheel for the specific Python version may not be in the first-matched index. 

Specifically, putting `PATH1` first, and trying to install for python 3.8/3.9, or `PATH2` first, and trying to install for python3.10/3.11, result in errors.

#### Using `unsafe-best-match`

This strategy works but causes many other packages to be superseded by PyPI versions, which is not desirable for this use case.

## Questions

1. Is this the expected behavior for `unsafe-first-match` when wheels have different compatibility tags across indexes?
2. Is there a way to configure uv to always prefer local indexes when compatible wheels exist, regardless of Python version?
3. Should the presence of a transitive dependency (`scikit-optimize`) affect the index selection for a direct dependency?

Thanks for any help!

## Debug logs

<details><summary>Python 3.11</summary>
<p>

~~~text
(epiclass_uv_test) [rabyj][rabyj@narval1 python]$ uv -v pip compile pyproject.toml
DEBUG uv 0.9.0
DEBUG Acquired shared lock for `/home/rabyj/.cache/uv`
DEBUG Starting Python discovery for a default Python
DEBUG Looking for exact match for request a default Python
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.11.5-linux-x86_64-gnu` at `/home/rabyj/scratch/envs/epiclass_uv_test/bin/python3` (active virtual environment)
DEBUG Using Python 3.11.5 interpreter at \x1b[36m/home/rabyj/scratch/envs/epiclass_uv_test/bin/python3\x1b[39m for builds
DEBUG Using request timeout of 30s
DEBUG Found static `requires-dist` for: /lustre06/project/6007017/rabyj/sources/epiclass/src/python
DEBUG No workspace root found, using project root
DEBUG Skipping GitHub fast path for: scikit-optimize @ git+https://github.com/QuentinSoubeyran/scikit-optimize.git@3f92bf42d947efb4ece3d89595d67dc56591c7c3 (shard exists)
DEBUG Fetching source distribution from Git: https://github.com/QuentinSoubeyran/scikit-optimize.git
DEBUG Acquired lock for `https://github.com/quentinsoubeyran/scikit-optimize`
DEBUG Using existing Git source `https://github.com/QuentinSoubeyran/scikit-optimize.git`
DEBUG Released lock at `/home/rabyj/.cache/uv/git-v0/locks/53c4968954a92382`
DEBUG Acquired lock for `/home/rabyj/.cache/uv/sdists-v9/git/6a01d2bda689f574/3f92bf42d947efb4`
DEBUG No static `pyproject.toml` available for: scikit-optimize @ git+https://github.com/QuentinSoubeyran/scikit-optimize.git@3f92bf42d947efb4ece3d89595d67dc56591c7c3 (FieldNotFound("project"))
DEBUG No static `PKG-INFO` available for: scikit-optimize @ git+https://github.com/QuentinSoubeyran/scikit-optimize.git@3f92bf42d947efb4ece3d89595d67dc56591c7c3 (MissingPkgInfo)
DEBUG Using cached metadata for: scikit-optimize @ git+https://github.com/QuentinSoubeyran/scikit-optimize.git@3f92bf42d947efb4ece3d89595d67dc56591c7c3
DEBUG Released lock at `/home/rabyj/.cache/uv/sdists-v9/git/6a01d2bda689f574/3f92bf42d947efb4/.lock`
DEBUG Solving with installed Python version: 3.11.5
DEBUG Solving with target Python version: >=3.11.5
DEBUG Adding direct dependency: scikit-learn>=1.2.1, <1.2.1+
DEBUG Adding direct dependency: scikit-optimize*
DEBUG Searching for a compatible version of scikit-optimize @ git+https://github.com/QuentinSoubeyran/scikit-optimize.git@3f92bf42d947efb4ece3d89595d67dc56591c7c3 (*)
DEBUG Adding transitive dependency for scikit-optimize==0.9.0+unofficial: joblib>=0.11
DEBUG Adding transitive dependency for scikit-optimize==0.9.0+unofficial: pyaml>=16.9
DEBUG Adding transitive dependency for scikit-optimize==0.9.0+unofficial: numpy>=1.13.3
DEBUG Adding transitive dependency for scikit-optimize==0.9.0+unofficial: scipy>=0.19.1
DEBUG Adding transitive dependency for scikit-optimize==0.9.0+unofficial: scikit-learn>=0.20.0
DEBUG Ignoring `--find-links` entry (expected a wheel or source distribution filename): /cvmfs/soft.computecanada.ca/custom/python/wheelhouse/generic/dulwich-0.24.2-7968.log
DEBUG Found fresh response for: https://pypi.org/simple/joblib/
DEBUG Found fresh response for: https://pypi.org/simple/pyaml/
DEBUG Found fresh response for: https://pypi.org/simple/scikit-learn/
DEBUG Searching for a compatible version of scikit-learn (>=1.2.1, <1.2.1+)
DEBUG Selecting: scikit-learn==1.2.1+computecanada [compatible] (scikit_learn-1.2.1+computecanada-cp311-cp311-linux_x86_64.whl)
DEBUG Found fresh response for: https://pypi.org/simple/numpy/
DEBUG Found fresh response for: https://pypi.org/simple/scipy/
DEBUG Adding transitive dependency for scikit-learn==1.2.1+computecanada: numpy>=1.23
DEBUG Adding transitive dependency for scikit-learn==1.2.1+computecanada: scipy>=1.3.2
DEBUG Adding transitive dependency for scikit-learn==1.2.1+computecanada: joblib>=1.1.1
DEBUG Adding transitive dependency for scikit-learn==1.2.1+computecanada: threadpoolctl>=2.0.0
DEBUG Searching for a compatible version of joblib (>=1.1.1)
DEBUG Selecting: joblib==1.5.2+computecanada [compatible] (joblib-1.5.2+computecanada-py3-none-any.whl)
DEBUG Searching for a compatible version of pyaml (>=16.9)
DEBUG Selecting: pyaml==20.4.0+computecanada [compatible] (pyaml-20.4.0+computecanada-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for pyaml==20.4.0+computecanada: pyyaml*
DEBUG Searching for a compatible version of numpy (>=1.23)
DEBUG Selecting: numpy==1.24.2+computecanada [compatible] (numpy-1.24.2+computecanada-cp311-cp311-linux_x86_64.whl)
DEBUG Found fresh response for: https://pypi.org/simple/threadpoolctl/
DEBUG Found fresh response for: https://pypi.org/simple/pyyaml/
DEBUG Searching for a compatible version of scipy (>=1.3.2)
DEBUG Selecting: scipy==1.10.1+computecanada [compatible] (scipy-1.10.1+computecanada-cp311-cp311-linux_x86_64.whl)
DEBUG Adding transitive dependency for scipy==1.10.1+computecanada: numpy>=1.23, <1.27.0
DEBUG Searching for a compatible version of threadpoolctl (>=2.0.0)
DEBUG Selecting: threadpoolctl==3.6.0+computecanada [compatible] (threadpoolctl-3.6.0+computecanada-py3-none-any.whl)
DEBUG Searching for a compatible version of pyyaml (*)
DEBUG Searching for a compatible version of pyyaml (<5.4.1+computecanada | >5.4.1+computecanada)
DEBUG Searching for a compatible version of pyyaml (<5.3.1+computecanada | >5.3.1+computecanada, <5.4.1+computecanada | >5.4.1+computecanada)
DEBUG Searching for a compatible version of pyyaml (<5.1.2+computecanada | >5.1.2+computecanada, <5.3.1+computecanada | >5.3.1+computecanada, <5.4.1+computecanada | >5.4.1+computecanada)
DEBUG Searching for a compatible version of pyyaml (<5.1.1+computecanada | >5.1.1+computecanada, <5.1.2+computecanada | >5.1.2+computecanada, <5.3.1+computecanada | >5.3.1+computecanada, <5.4.1+computecanada | >5.4.1+computecanada)
DEBUG Searching for a compatible version of pyyaml (<5.1+computecanada | >5.1+computecanada, <5.1.1+computecanada | >5.1.1+computecanada, <5.1.2+computecanada | >5.1.2+computecanada, <5.3.1+computecanada | >5.3.1+computecanada, <5.4.1+computecanada | >5.4.1+computecanada)
DEBUG Searching for a compatible version of pyyaml (<4.1+computecanada | >4.1+computecanada, <5.1+computecanada | >5.1+computecanada, <5.1.1+computecanada | >5.1.1+computecanada, <5.1.2+computecanada | >5.1.2+computecanada, <5.3.1+computecanada | >5.3.1+computecanada, <5.4.1+computecanada | >5.4.1+computecanada)
DEBUG Searching for a compatible version of pyyaml (<3.12+computecanada | >3.12+computecanada, <4.1+computecanada | >4.1+computecanada, <5.1+computecanada | >5.1+computecanada, <5.1.1+computecanada | >5.1.1+computecanada, <5.1.2+computecanada | >5.1.2+computecanada, <5.3.1+computecanada | >5.3.1+computecanada, <5.4.1+computecanada | >5.4.1+computecanada)
DEBUG Searching for a compatible version of pyyaml (<3.12+computecanada | >3.12+computecanada, <4.1+computecanada | >4.1+computecanada, <5.1+computecanada | >5.1+computecanada, <5.1.1+computecanada | >5.1.1+computecanada, <5.1.2+computecanada | >5.1.2+computecanada, <5.2+computecanada | >5.2+computecanada, <5.3.1+computecanada | >5.3.1+computecanada, <5.4.1+computecanada | >5.4.1+computecanada)
DEBUG Selecting: pyyaml==6.0.3 [compatible] (pyyaml-6.0.3-cp311-cp311-manylinux2014_x86_64.manylinux_2_17_x86_64.manylinux_2_28_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/71/60/917329f640924b18ff085ab889a11c763e0b573da888e8404ff486657602/pyyaml-6.0.3-cp311-cp311-manylinux2014_x86_64.manylinux_2_17_x86_64.manylinux_2_28_x86_64.whl.metadata
DEBUG Tried 8 versions: joblib 1, numpy 1, pyaml 1, pyyaml 1, scikit-learn 1, scikit-optimize 1, scipy 1, threadpoolctl 1
DEBUG marker environment resolution took 0.402s
Resolved 8 packages in 422ms
# This file was autogenerated by uv via the following command:
#    uv pip compile pyproject.toml
joblib==1.5.2+computecanada
    # via
    #   scikit-learn
    #   scikit-optimize
numpy==1.24.2+computecanada
    # via
    #   scikit-learn
    #   scikit-optimize
    #   scipy
pyaml==20.4.0+computecanada
    # via scikit-optimize
pyyaml==6.0.3
    # via pyaml
scikit-learn==1.2.1+computecanada
    # via
    #   test (pyproject.toml)
    #   scikit-optimize
scikit-optimize @ git+https://github.com/QuentinSoubeyran/scikit-optimize.git@3f92bf42d947efb4ece3d89595d67dc56591c7c3
    # via test (pyproject.toml)
scipy==1.10.1+computecanada
    # via
    #   scikit-learn
    #   scikit-optimize
threadpoolctl==3.6.0+computecanada
    # via scikit-learn
DEBUG Released lock at `/home/rabyj/.cache/uv/.lock`
~~~

</p>
</details>

<details><summary>Python 3.8</summary>
<p>

~~~python(epiclass_uv_test) [rabyj][rabyj@narval1 python]$ uv -v pip compile pyproject.toml
DEBUG uv 0.9.0
DEBUG Acquired shared lock for `/home/rabyj/.cache/uv`
DEBUG Starting Python discovery for a default Python
DEBUG Looking for exact match for request a default Python
DEBUG Searching for default Python interpreter in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.8.10-linux-x86_64-gnu` at `/home/rabyj/scratch/envs/epiclass_uv_test/bin/python3` (active virtual environment)
DEBUG Using Python 3.8.10 interpreter at \x1b[36m/home/rabyj/scratch/envs/epiclass_uv_test/bin/python3\x1b[39m for builds
DEBUG Using request timeout of 30s
DEBUG Found static `requires-dist` for: /lustre06/project/6007017/rabyj/sources/epiclass/src/python
DEBUG No workspace root found, using project root
DEBUG Skipping GitHub fast path for: scikit-optimize @ git+https://github.com/QuentinSoubeyran/scikit-optimize.git@3f92bf42d947efb4ece3d89595d67dc56591c7c3 (shard exists)
DEBUG Fetching source distribution from Git: https://github.com/QuentinSoubeyran/scikit-optimize.git
DEBUG Acquired lock for `https://github.com/quentinsoubeyran/scikit-optimize`
DEBUG Using existing Git source `https://github.com/QuentinSoubeyran/scikit-optimize.git`
DEBUG Released lock at `/home/rabyj/.cache/uv/git-v0/locks/53c4968954a92382`
DEBUG Acquired lock for `/home/rabyj/.cache/uv/sdists-v9/git/6a01d2bda689f574/3f92bf42d947efb4`
DEBUG No static `pyproject.toml` available for: scikit-optimize @ git+https://github.com/QuentinSoubeyran/scikit-optimize.git@3f92bf42d947efb4ece3d89595d67dc56591c7c3 (FieldNotFound("project"))
DEBUG No static `PKG-INFO` available for: scikit-optimize @ git+https://github.com/QuentinSoubeyran/scikit-optimize.git@3f92bf42d947efb4ece3d89595d67dc56591c7c3 (MissingPkgInfo)
DEBUG Using cached metadata for: scikit-optimize @ git+https://github.com/QuentinSoubeyran/scikit-optimize.git@3f92bf42d947efb4ece3d89595d67dc56591c7c3
DEBUG Released lock at `/home/rabyj/.cache/uv/sdists-v9/git/6a01d2bda689f574/3f92bf42d947efb4/.lock`
DEBUG Solving with installed Python version: 3.8.10
DEBUG Solving with target Python version: >=3.8.10
DEBUG Adding direct dependency: scikit-learn>=1.2.1, <1.2.1+
DEBUG Adding direct dependency: scikit-optimize*
DEBUG Searching for a compatible version of scikit-optimize @ git+https://github.com/QuentinSoubeyran/scikit-optimize.git@3f92bf42d947efb4ece3d89595d67dc56591c7c3 (*)
DEBUG Adding transitive dependency for scikit-optimize==0.9.0+unofficial: joblib>=0.11
DEBUG Adding transitive dependency for scikit-optimize==0.9.0+unofficial: pyaml>=16.9
DEBUG Adding transitive dependency for scikit-optimize==0.9.0+unofficial: numpy>=1.13.3
DEBUG Adding transitive dependency for scikit-optimize==0.9.0+unofficial: scipy>=0.19.1
DEBUG Adding transitive dependency for scikit-optimize==0.9.0+unofficial: scikit-learn>=0.20.0
DEBUG Ignoring `--find-links` entry (expected a wheel or source distribution filename): /cvmfs/soft.computecanada.ca/custom/python/wheelhouse/generic/dulwich-0.24.2-7968.log
DEBUG Found fresh response for: https://pypi.org/simple/scikit-learn/
DEBUG Found fresh response for: https://pypi.org/simple/pyaml/
DEBUG Searching for a compatible version of scikit-learn (>=1.2.1, <1.2.1+)
DEBUG Found fresh response for: https://pypi.org/simple/joblib/
DEBUG Searching for a compatible version of scikit-learn (>=1.2.1, <1.2.1+computecanada | >1.2.1+computecanada, <1.2.1+)
DEBUG Selecting: scikit-learn==1.2.1 [compatible] (scikit_learn-1.2.1-cp38-cp38-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Found fresh response for: https://pypi.org/simple/scipy/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f0/95/0ea0a2412e33080a47ec02802210c008a7a540471581c95145f030d304b4/scikit_learn-1.2.1-cp38-cp38-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Adding transitive dependency for scikit-learn==1.2.1: numpy>=1.17.3
DEBUG Adding transitive dependency for scikit-learn==1.2.1: scipy>=1.3.2
DEBUG Adding transitive dependency for scikit-learn==1.2.1: joblib>=1.1.1
DEBUG Adding transitive dependency for scikit-learn==1.2.1: threadpoolctl>=2.0.0
DEBUG Searching for a compatible version of joblib (>=1.1.1)
DEBUG Selecting: joblib==1.5.2+computecanada [compatible] (joblib-1.5.2+computecanada-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/numpy/
DEBUG Searching for a compatible version of joblib (>=1.1.1, <1.5.2+computecanada | >1.5.2+computecanada)
DEBUG Selecting: joblib==1.5.1+computecanada [compatible] (joblib-1.5.1+computecanada-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/threadpoolctl/
DEBUG Searching for a compatible version of joblib (>=1.1.1, <1.5.1+computecanada | >1.5.1+computecanada, <1.5.2+computecanada | >1.5.2+computecanada)
DEBUG Selecting: joblib==1.4.2+computecanada [compatible] (joblib-1.4.2+computecanada-py3-none-any.whl)
DEBUG Searching for a compatible version of pyaml (>=16.9)
DEBUG Selecting: pyaml==20.4.0+computecanada [compatible] (pyaml-20.4.0+computecanada-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for pyaml==20.4.0+computecanada: pyyaml*
DEBUG Searching for a compatible version of numpy (>=1.17.3)
DEBUG Selecting: numpy==1.17.5+computecanada [compatible] (numpy-1.17.5+computecanada-cp38-cp38-linux_x86_64.whl)
DEBUG Searching for a compatible version of scipy (>=1.3.2)
DEBUG Selecting: scipy==1.10.1+computecanada [compatible] (scipy-1.10.1+computecanada-cp38-cp38-linux_x86_64.whl)
DEBUG Adding transitive dependency for scipy==1.10.1+computecanada: numpy>=1.19.5, <1.27.0
DEBUG Recording unit propagation conflict of scipy from incompatibility of (scipy, numpy)
DEBUG Searching for a compatible version of scipy (>=1.3.2, <1.10.1+computecanada | >1.10.1+computecanada)
DEBUG Selecting: scipy==1.9.3+computecanada [compatible] (scipy-1.9.3+computecanada-cp38-cp38-linux_x86_64.whl)
DEBUG Found fresh response for: https://pypi.org/simple/pyyaml/
DEBUG Adding transitive dependency for scipy==1.9.3+computecanada: numpy>=1.21, <1.26.0
DEBUG Recording dependency conflict of scipy==1.9.3+computecanada from incompatibility of (scipy, numpy)
DEBUG Searching for a compatible version of scipy (>=1.3.2, <1.9.3+computecanada | >1.9.3+computecanada, <1.10.1+computecanada | >1.10.1+computecanada)
DEBUG Selecting: scipy==1.8.0+computecanada [compatible] (scipy-1.8.0+computecanada-cp38-cp38-linux_x86_64.whl)
DEBUG Adding transitive dependency for scipy==1.8.0+computecanada: numpy>=1.21, <1.25.0
DEBUG Recording dependency conflict of scipy==1.8.0+computecanada from incompatibility of (scipy, numpy)
DEBUG Searching for a compatible version of scipy (>=1.3.2, <1.8.0+computecanada | >1.8.0+computecanada, <1.9.3+computecanada | >1.9.3+computecanada, <1.10.1+computecanada | >1.10.1+computecanada)
DEBUG Selecting: scipy==1.7.3+computecanada [compatible] (scipy-1.7.3+computecanada-cp38-cp38-linux_x86_64.whl)
DEBUG Adding transitive dependency for scipy==1.7.3+computecanada: numpy>=1.16.5, <1.23.0
DEBUG Searching for a compatible version of threadpoolctl (>=2.0.0)
DEBUG Selecting: threadpoolctl==3.6.0+computecanada [compatible] (threadpoolctl-3.6.0+computecanada-py3-none-any.whl)
DEBUG Searching for a compatible version of threadpoolctl (>=2.0.0, <3.6.0+computecanada | >3.6.0+computecanada)
DEBUG Selecting: threadpoolctl==3.5.0+computecanada [compatible] (threadpoolctl-3.5.0+computecanada-py3-none-any.whl)
DEBUG Searching for a compatible version of pyyaml (*)
DEBUG Selecting: pyyaml==5.4.1+computecanada [compatible] (PyYAML-5.4.1+computecanada-cp38-cp38-linux_x86_64.whl)
DEBUG Tried 14 versions: scipy 4, joblib 3, threadpoolctl 2, numpy 1, pyaml 1, pyyaml 1, scikit-learn 1, scikit-optimize 1
DEBUG marker environment resolution took 0.130s
Resolved 8 packages in 150ms
# This file was autogenerated by uv via the following command:
#    uv pip compile pyproject.toml
joblib==1.4.2+computecanada
    # via
    #   scikit-learn
    #   scikit-optimize
numpy==1.17.5+computecanada
    # via
    #   scikit-learn
    #   scikit-optimize
    #   scipy
pyaml==20.4.0+computecanada
    # via scikit-optimize
pyyaml==5.4.1+computecanada
    # via pyaml
scikit-learn==1.2.1
    # via
    #   test (pyproject.toml)
    #   scikit-optimize
scikit-optimize @ git+https://github.com/QuentinSoubeyran/scikit-optimize.git@3f92bf42d947efb4ece3d89595d67dc56591c7c3
    # via test (pyproject.toml)
scipy==1.7.3+computecanada
    # via
    #   scikit-learn
    #   scikit-optimize
threadpoolctl==3.5.0+computecanada
    # via scikit-learn
DEBUG Released lock at `/home/rabyj/.cache/uv/.lock`
~~~

</p>
</details>

### Platform

Linux 4.18.0-553.77.1.el8_10.x86_64 x86_64 GNU/Linux

### Version

uv 0.9.0

---

_Label `question` added by @rabyj on 2025-10-10 22:39_

---

_Comment by @michael-o on 2025-12-13 22:56_

I have reported a quite similar issue with #17109. Tried for two days all possible variations. scikit-learn being one of my deps. It seems uv have a problem with split dependencies. It kept ignoring my setup. Unfortunately, I had to resort to a proxpi setup to combine multiple indexes: https://github.com/EpicWink/proxpi/pull/76

---

_Comment by @rabyj on 2025-12-15 18:10_

@michael-o In my case, I think something like #16301 might resolve my problem, I don't know if it could also help your use case?

---

_Comment by @michael-o on 2025-12-16 07:49_

> [@michael-o](https://github.com/michael-o) In my case, I think something like [#16301](https://github.com/astral-sh/uv/issues/16301) might resolve my problem, I don't know if it could also help your use case?

I don't this so, but I also don't think that this proposal is right. What you are looking for is a profile to activate an index based on external factors/markers. E.g., Maven offers to active repositories based on evaluators in a profile.
The philosophy with uv seems to be that the pyproject.toml is expected to be static...

---

_Comment by @konstin on 2025-12-16 09:47_

In the example, is there a reason why the wheelhouse indexes don't mirror the packages for PyPI fully, so the index can replace PyPI for the given package?

---

_Comment by @rabyj on 2025-12-16 18:18_

@konstin The example comes from a problem I had on a DRAC (Digital Research Alliance of Canada) HPC system, where new wheels are built on a "needed" basis. Common scientific packages are built, but not for each version of each package systematically, and obscure packages are not built by default. They are distributed through the [CVMFS system](https://docs.alliancecan.ca/wiki/Accessing_CVMFS).

Wheelhouse reference doc: https://docs.alliancecan.ca/wiki/Available_Python_wheels

---

_Comment by @rabyj on 2025-12-16 18:41_

@michael-o 
> The philosophy with uv seems to be that the pyproject.toml is expected to be static...

I don't see how my feature request (ignoring missing indexes) is incompatible with that philosophy, if the comment was referring to #16301  :/

As for the present issue, I ended giving up and dropping python < cp3.10, (it's EOL anyways), so that I only need to put one of the wheelhouses in the config. 

It really would be great if UV was more flexible with the indexes, in any case. I imagine there must be other older issues that have a better view of the whole problem, I just didn't find when searching. Maybe https://github.com/astral-sh/uv/issues/8253 is related, but I'm unsure.

---

_Comment by @konstin on 2025-12-17 16:36_

Currently, uv isn't really supporting layering and index on top of another in the way of only adding specific packages from another URL, uv and the `uv.lock` format assume that there's one source per version. This is partially for security (https://pytorch.org/blog/compromised-nightly-dependency/), hence the `unsafe-` name in the options that partially ignore this.

The ideal thing from a uv perspective would be the CVMFS mirroring the other wheels for each version they build, this would create consistency from a resolving and locking perspective.

We can keep this issue to track better index layering.

---

_Label `question` removed by @konstin on 2025-12-17 16:37_

---

_Label `enhancement` added by @konstin on 2025-12-17 16:37_

---

_Label `needs-design` added by @konstin on 2025-12-17 16:37_

---

_Comment by @michael-o on 2025-12-17 17:36_

> Currently, uv isn't really supporting layering and index on top of another in the way of only adding specific packages from another URL, uv and the `uv.lock` format assume that there's one source per version. This is partially for security (https://pytorch.org/blog/compromised-nightly-dependency/), hence the `unsafe-` name in the options that partially ignore this.
> 
> The ideal thing from a uv perspective would be the CVMFS mirroring the other wheels for each version they build, this would create consistency from a resolving and locking perspective.
> 
> We can keep this issue to track better index layering.

In other words you need proxpi or similar to.solve this problem...

---

_Comment by @rabyj on 2025-12-17 17:50_

> The ideal thing from a uv perspective would be the CVMFS mirroring the other wheels for each version they build, this would create consistency from a resolving and locking perspective.

We can't really expect them to build wheels for each package versions, sadly. Limited workforce and all.

Thank you for confirming my use case is not possible for now.

I guess for now I'll have to use 'best-match' and exclude PyPI entirely when compiling requirements, and iteratively build missing packages. This process makes resolving the full missing packages list take much longer, but it is what it is. At least I know it's safe since I'll only be allowing CVMFS wheelhouses.

---

_Comment by @konstin on 2025-12-17 18:16_

I didn't mean that they need to build all versions, rather than mirroring the PyPI wheels in addition to their own builds. There may be some complications around 1.2.1 vs. 1.2.1+computecanada, cause they are technically not same, at least for indexes I'm more familiar with creating builds that use the same version without local suffix; But that of course depends on the specific index setup.

---

_Comment by @rabyj on 2025-12-17 18:22_

Ah I see, I misunderstood. All of their wheels are marked with that suffix.

If you mean having PyPI wheels directly accessible in their wheelhouses, they won't do that for sure, since it's supposed to be a secure-ish system, and there are no guarantees on PyPI packages. They only make wheels built on the system available on compute nodes (internet-less).

~~If you mean shadowing the PyPI packages, they probably won't care for it since they don't explicitly support uv use (only pip). Bleh. My understanding is that they explicitly add a suffix so that users don't get confused by the source of the package (since the suffix shows up on `pip list`), if this method means the name shadows the PyPI package, they probably won't want to do that.~~

---
