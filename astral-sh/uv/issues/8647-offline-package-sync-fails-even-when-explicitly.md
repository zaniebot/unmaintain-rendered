---
number: 8647
title: "offline package sync fails even when explicitly installing build dependency `hatchling`"
type: issue
state: closed
author: benniekiss
labels: []
assignees: []
created_at: 2024-10-28T20:57:46Z
updated_at: 2024-10-28T21:30:49Z
url: https://github.com/astral-sh/uv/issues/8647
synced_at: 2026-01-10T01:24:31Z
---

# offline package sync fails even when explicitly installing build dependency `hatchling`

---

_Issue opened by @benniekiss on 2024-10-28 20:57_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Even when `hatchling` is explicitly installed in the environment, an offline install will fail because it does not look up the `[build-system.requires]` dependency the same way--it looks up `https://pypi.org/simple/hatchling/` instead, which does not get cached unless the project itself is first installed online.

Here are two scripts, one where the cache is warmed by installing the project, and another where it is not:

**INSTALLED**
<details><summary>script</summary>
<p>

```bash
#!/bin/bash

mkdir -p uv_test_caching
cd uv_test_caching

echo "--------------"
echo "CLEANING CACHE"
echo "--------------"

uv cache clean

echo "-----------------"
echo "PREPARING PROJECT"
echo "-----------------"

uv init --lib
uv add numpy

# explicitly cache hatchling
uv add --group build hatchling

echo "-------------"
echo "WARMING CACHE"
echo "-------------"

uv cache clean

uv venv

# install deps online to make sure they are cached
uv sync --no-dev --group build

echo "--------------------"
echo "   TESTING CACHE    "
echo "--------------------"
echo "                    "
echo "--------------------"
echo "OFFLINE DEPS INSTALL"
echo "--------------------"

# reset the venv
uv venv

# show that the packages can be found in the cache
uv sync --no-install-project --group build --offline --verbose

echo "-----------------------"
echo "OFFLINE PROJECT INSTALL"
echo "-----------------------"

# reset the venv
uv venv

# the sync is successful
uv sync --no-binary-package=testing --offline --verbose

echo "--------------------------"
echo "OFFLINE PROJECT INSTALL W/"
echo "EXPLICIT HATCHLING INSTALL"
echo "--------------------------"

# reset the venv
uv venv

# the sync is successful
uv sync --no-binary-package=testing --group build --offline --verbose

```

</p>
</details> 

<details><summary>results</summary>
<p>

```
--------------
CLEANING CACHE
--------------
Clearing cache at: /Users/benniemt/.cache/uv
Removed 1007 files (18.9MiB)
-----------------
PREPARING PROJECT
-----------------
error: Project is already initialized in `/Users/benniemt/uv_test_caching` (`pyproject.toml` file exists)
Resolved 7 packages in 0.24ms
Audited 2 packages in 0.07ms
Resolved 7 packages in 0.20ms
Audited 7 packages in 0.04ms
-------------
WARMING CACHE
-------------
Clearing cache at: /Users/benniemt/.cache/uv
Removed 5 files (1.7KiB)
Using CPython 3.13.0 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
Resolved 7 packages in 0.61ms
   Built uv-test-caching @ file:///Users/benniemt/uv_test_caching
Prepared 7 packages in 434ms
Installed 7 packages in 17ms
 + hatchling==1.25.0
 + numpy==2.1.2
 + packaging==24.1
 + pathspec==0.12.1
 + pluggy==1.5.0
 + trove-classifiers==2024.10.21.16
 + uv-test-caching==0.1.0 (from file:///Users/benniemt/uv_test_caching)
--------------------
   TESTING CACHE    
--------------------
                    
--------------------
OFFLINE DEPS INSTALL
--------------------
Using CPython 3.13.0 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
DEBUG uv 0.4.27 (Homebrew 2024-10-25)
DEBUG Found project root: `/Users/benniemt/uv_test_caching`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/Users/benniemt/uv_test_caching/.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.13`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: uv-test-caching @ file:///Users/benniemt/uv_test_caching
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 7 packages in 0.52ms
DEBUG Omitting `uv-test-caching` from resolution due to `--no-install-project`
DEBUG Using request timeout of 30s
DEBUG Requirement already cached: hatchling==1.25.0
DEBUG Requirement already cached: numpy==2.1.2
DEBUG Requirement already cached: packaging==24.1
DEBUG Requirement already cached: pathspec==0.12.1
DEBUG Requirement already cached: pluggy==1.5.0
DEBUG Requirement already cached: trove-classifiers==2024.10.21.16
Installed 6 packages in 14ms
 + hatchling==1.25.0
 + numpy==2.1.2
 + packaging==24.1
 + pathspec==0.12.1
 + pluggy==1.5.0
 + trove-classifiers==2024.10.21.16
-----------------------
OFFLINE PROJECT INSTALL
-----------------------
Using CPython 3.13.0 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
DEBUG uv 0.4.27 (Homebrew 2024-10-25)
DEBUG Found project root: `/Users/benniemt/uv_test_caching`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/Users/benniemt/uv_test_caching/.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.13`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: uv-test-caching @ file:///Users/benniemt/uv_test_caching
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 7 packages in 0.56ms
DEBUG Using request timeout of 30s
DEBUG Must revalidate requirement: numpy
DEBUG Must revalidate requirement: uv-test-caching
DEBUG Acquired lock for `/Users/benniemt/.cache/uv/sdists-v5/editable/4a4b69fa9b468847`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/35/eb/5677556d9ba13436dab51e129f98d4829d95cd1b6bd0e199c14485a4bdb9/numpy-2.1.2-cp313-cp313-macosx_14_0_arm64.whl
DEBUG Building: uv-test-caching @ file:///Users/benniemt/uv_test_caching
DEBUG No workspace root found, using project root
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: hatchling*
DEBUG Found fresh response for: https://pypi.org/simple/hatchling/
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.25.0 [compatible] (hatchling-1.25.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for hatchling==1.25.0: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.25.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.25.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.25.0: trove-classifiers*
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.1 [compatible] (packaging-24.1-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/pathspec/
DEBUG Found fresh response for: https://pypi.org/simple/pluggy/
DEBUG Found fresh response for: https://pypi.org/simple/trove-classifiers/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/35/35/5055ab8d215af853d07bbff1a74edf48f91ed308f037380a5ca52dd73348/trove_classifiers-2024.10.21.16-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.10.21.16 [compatible] (trove_classifiers-2024.10.21.16-py3-none-any.whl)
DEBUG Tried 5 versions: hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1
DEBUG Split specific environment resolution took 0.001s
DEBUG Installing in hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.10.21.16 in /Users/benniemt/.cache/uv/builds-v0/.tmpjOscFD
DEBUG Must revalidate requirement: hatchling
DEBUG Must revalidate requirement: packaging
DEBUG Must revalidate requirement: pathspec
DEBUG Must revalidate requirement: pluggy
DEBUG Must revalidate requirement: trove-classifiers
DEBUG Downloading and building requirements for build: hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.10.21.16
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/35/35/5055ab8d215af853d07bbff1a74edf48f91ed308f037380a5ca52dd73348/trove_classifiers-2024.10.21.16-py3-none-any.whl
DEBUG Installing build requirements: hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.10.21.16
DEBUG Creating PEP 517 build environment
DEBUG Calling `hatchling.build.get_requires_for_build_editable()`
DEBUG No workspace root found, using project root
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: editables>=0.3, <1.dev0
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.25.0 [compatible] (hatchling-1.25.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatchling==1.25.0: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.25.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.25.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.25.0: trove-classifiers*
DEBUG Found fresh response for: https://pypi.org/simple/editables/
DEBUG Searching for a compatible version of editables (>=0.3, <1.dev0)
DEBUG Selecting: editables==0.5 [compatible] (editables-0.5-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6b/be/0f2f4a5e8adc114a02b63d92bf8edbfa24db6fc602fca83c885af2479e0e/editables-0.5-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.1 [compatible] (packaging-24.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.10.21.16 [compatible] (trove_classifiers-2024.10.21.16-py3-none-any.whl)
DEBUG Tried 6 versions: editables 1, hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1
DEBUG Split specific environment resolution took 0.000s
DEBUG Installing in editables==0.5, hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.10.21.16 in /Users/benniemt/.cache/uv/builds-v0/.tmpjOscFD
DEBUG Must revalidate requirement: editables
DEBUG Requirement already installed: hatchling==1.25.0
DEBUG Requirement already installed: packaging==24.1
DEBUG Requirement already installed: pathspec==0.12.1
DEBUG Requirement already installed: pluggy==1.5.0
DEBUG Requirement already installed: trove-classifiers==2024.10.21.16
DEBUG Downloading and building requirement for build: editables==0.5
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6b/be/0f2f4a5e8adc114a02b63d92bf8edbfa24db6fc602fca83c885af2479e0e/editables-0.5-py3-none-any.whl
DEBUG Installing build requirement: editables==0.5
DEBUG Calling `hatchling.build.build_editable("/Users/benniemt/.cache/uv/sdists-v5/editable/4a4b69fa9b468847/m4_zlZ4MdfBFDLaoUM4a5/.tmpkmgpVT", {}, None)`
DEBUG Finished building: uv-test-caching @ file:///Users/benniemt/uv_test_caching
DEBUG Released lock at `/Users/benniemt/.cache/uv/sdists-v5/editable/4a4b69fa9b468847/.lock`
Prepared 2 packages in 232ms
Installed 2 packages in 9ms
 + numpy==2.1.2
 + uv-test-caching==0.1.0 (from file:///Users/benniemt/uv_test_caching)
--------------------------
OFFLINE PROJECT INSTALL W/
EXPLICIT HATCHLING INSTALL
--------------------------
Using CPython 3.13.0 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
DEBUG uv 0.4.27 (Homebrew 2024-10-25)
DEBUG Found project root: `/Users/benniemt/uv_test_caching`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/Users/benniemt/uv_test_caching/.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.13`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: uv-test-caching @ file:///Users/benniemt/uv_test_caching
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 7 packages in 0.54ms
DEBUG Using request timeout of 30s
DEBUG Must revalidate requirement: hatchling
DEBUG Must revalidate requirement: numpy
DEBUG Must revalidate requirement: packaging
DEBUG Must revalidate requirement: pathspec
DEBUG Must revalidate requirement: pluggy
DEBUG Must revalidate requirement: trove-classifiers
DEBUG Must revalidate requirement: uv-test-caching
DEBUG Acquired lock for `/Users/benniemt/.cache/uv/sdists-v5/editable/4a4b69fa9b468847`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/35/eb/5677556d9ba13436dab51e129f98d4829d95cd1b6bd0e199c14485a4bdb9/numpy-2.1.2-cp313-cp313-macosx_14_0_arm64.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/35/35/5055ab8d215af853d07bbff1a74edf48f91ed308f037380a5ca52dd73348/trove_classifiers-2024.10.21.16-py3-none-any.whl
DEBUG Building: uv-test-caching @ file:///Users/benniemt/uv_test_caching
DEBUG No workspace root found, using project root
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: hatchling*
DEBUG Found fresh response for: https://pypi.org/simple/hatchling/
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.25.0 [compatible] (hatchling-1.25.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for hatchling==1.25.0: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.25.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.25.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.25.0: trove-classifiers*
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Found fresh response for: https://pypi.org/simple/pathspec/
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.1 [compatible] (packaging-24.1-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/pluggy/
DEBUG Found fresh response for: https://pypi.org/simple/trove-classifiers/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.10.21.16 [compatible] (trove_classifiers-2024.10.21.16-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/35/35/5055ab8d215af853d07bbff1a74edf48f91ed308f037380a5ca52dd73348/trove_classifiers-2024.10.21.16-py3-none-any.whl.metadata
DEBUG Tried 5 versions: hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1
DEBUG Split specific environment resolution took 0.001s
DEBUG Installing in hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.10.21.16 in /Users/benniemt/.cache/uv/builds-v0/.tmpLpv1qO
DEBUG Must revalidate requirement: hatchling
DEBUG Must revalidate requirement: packaging
DEBUG Must revalidate requirement: pathspec
DEBUG Must revalidate requirement: pluggy
DEBUG Must revalidate requirement: trove-classifiers
DEBUG Downloading and building requirements for build: hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.10.21.16
DEBUG Installing build requirements: hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.10.21.16
DEBUG Creating PEP 517 build environment
DEBUG Calling `hatchling.build.get_requires_for_build_editable()`
DEBUG No workspace root found, using project root
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: editables>=0.3, <1.dev0
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.25.0 [compatible] (hatchling-1.25.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatchling==1.25.0: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.25.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.25.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.25.0: trove-classifiers*
DEBUG Found fresh response for: https://pypi.org/simple/editables/
DEBUG Searching for a compatible version of editables (>=0.3, <1.dev0)
DEBUG Selecting: editables==0.5 [compatible] (editables-0.5-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6b/be/0f2f4a5e8adc114a02b63d92bf8edbfa24db6fc602fca83c885af2479e0e/editables-0.5-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.1 [compatible] (packaging-24.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.10.21.16 [compatible] (trove_classifiers-2024.10.21.16-py3-none-any.whl)
DEBUG Tried 6 versions: editables 1, hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1
DEBUG Split specific environment resolution took 0.000s
DEBUG Installing in editables==0.5, hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.10.21.16 in /Users/benniemt/.cache/uv/builds-v0/.tmpLpv1qO
DEBUG Must revalidate requirement: editables
DEBUG Requirement already installed: hatchling==1.25.0
DEBUG Requirement already installed: packaging==24.1
DEBUG Requirement already installed: pathspec==0.12.1
DEBUG Requirement already installed: pluggy==1.5.0
DEBUG Requirement already installed: trove-classifiers==2024.10.21.16
DEBUG Downloading and building requirement for build: editables==0.5
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6b/be/0f2f4a5e8adc114a02b63d92bf8edbfa24db6fc602fca83c885af2479e0e/editables-0.5-py3-none-any.whl
DEBUG Installing build requirement: editables==0.5
DEBUG Calling `hatchling.build.build_editable("/Users/benniemt/.cache/uv/sdists-v5/editable/4a4b69fa9b468847/urxogDbJJCgFIokq8RP6A/.tmpuczKUd", {}, None)`
DEBUG Finished building: uv-test-caching @ file:///Users/benniemt/uv_test_caching
DEBUG Released lock at `/Users/benniemt/.cache/uv/sdists-v5/editable/4a4b69fa9b468847/.lock`
Prepared 7 packages in 236ms
Installed 7 packages in 15ms
 + hatchling==1.25.0
 + numpy==2.1.2
 + packaging==24.1
 + pathspec==0.12.1
 + pluggy==1.5.0
 + trove-classifiers==2024.10.21.16
 + uv-test-caching==0.1.0 (from file:///Users/benniemt/uv_test_caching)
```

</p>
</details> 

**NOT INSTALLED**
<details><summary>script</summary>
<p>

```bash
#!/bin/bash

mkdir -p uv_test_caching
cd uv_test_caching

echo "--------------"
echo "CLEANING CACHE"
echo "--------------"

uv cache clean

echo "-----------------"
echo "PREPARING PROJECT"
echo "-----------------"

uv init --lib
uv add numpy

# explicitly cache hatchling
uv add --group build hatchling

echo "-------------"
echo "WARMING CACHE"
echo "-------------"

uv cache clean

uv venv

# install deps online to make sure they are cached
uv sync --no-install-project 
--no-dev --group build

echo "--------------------"
echo "   TESTING CACHE    "
echo "--------------------"
echo "                    "
echo "--------------------"
echo "OFFLINE DEPS INSTALL"
echo "--------------------"

# reset the venv
uv venv

# show that the packages can be found in the cache
uv sync --no-install-project --group build --offline --verbose

echo "-----------------------"
echo "OFFLINE PROJECT INSTALL"
echo "-----------------------"

# reset the venv
uv venv

# hatchling cannot be found in the cache
uv sync --no-binary-package=testing --offline --verbose

echo "--------------------------"
echo "OFFLINE PROJECT INSTALL W/"
echo "EXPLICIT HATCHLING INSTALL"
echo "--------------------------"

# reset the venv
uv venv

# hatchling cannot be found in the cache
uv sync --no-binary-package=testing --group build --offline --verbose
```

</p>
</details> 

<details><summary>results</summary>
<p>

```
--------------
CLEANING CACHE
--------------
Clearing cache at: /Users/benniemt/.cache/uv
Removed 1007 files (18.9MiB)
-----------------
PREPARING PROJECT
-----------------
error: Project is already initialized in `/Users/benniemt/uv_test_caching` (`pyproject.toml` file exists)
Resolved 7 packages in 0.26ms
Audited 2 packages in 0.11ms
Resolved 7 packages in 0.20ms
Audited 7 packages in 0.04ms
-------------
WARMING CACHE
-------------
Clearing cache at: /Users/benniemt/.cache/uv
Removed 5 files (1.7KiB)
Using CPython 3.13.0 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
Resolved 7 packages in 0.53ms
Prepared 6 packages in 235ms
Installed 6 packages in 13ms
 + hatchling==1.25.0
 + numpy==2.1.2
 + packaging==24.1
 + pathspec==0.12.1
 + pluggy==1.5.0
 + trove-classifiers==2024.10.21.16
--------------------
   TESTING CACHE    
--------------------
                    
--------------------
OFFLINE DEPS INSTALL
--------------------
Using CPython 3.13.0 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
DEBUG uv 0.4.27 (Homebrew 2024-10-25)
DEBUG Found project root: `/Users/benniemt/uv_test_caching`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/Users/benniemt/uv_test_caching/.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.13`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: uv-test-caching @ file:///Users/benniemt/uv_test_caching
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 7 packages in 0.57ms
DEBUG Omitting `uv-test-caching` from resolution due to `--no-install-project`
DEBUG Using request timeout of 30s
DEBUG Requirement already cached: hatchling==1.25.0
DEBUG Requirement already cached: numpy==2.1.2
DEBUG Requirement already cached: packaging==24.1
DEBUG Requirement already cached: pathspec==0.12.1
DEBUG Requirement already cached: pluggy==1.5.0
DEBUG Requirement already cached: trove-classifiers==2024.10.21.16
Installed 6 packages in 13ms
 + hatchling==1.25.0
 + numpy==2.1.2
 + packaging==24.1
 + pathspec==0.12.1
 + pluggy==1.5.0
 + trove-classifiers==2024.10.21.16
-----------------------
OFFLINE PROJECT INSTALL
-----------------------
Using CPython 3.13.0 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
DEBUG uv 0.4.27 (Homebrew 2024-10-25)
DEBUG Found project root: `/Users/benniemt/uv_test_caching`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/Users/benniemt/uv_test_caching/.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.13`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: uv-test-caching @ file:///Users/benniemt/uv_test_caching
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 7 packages in 0.56ms
DEBUG Using request timeout of 30s
DEBUG Must revalidate requirement: numpy
DEBUG Must revalidate requirement: uv-test-caching
DEBUG Acquired lock for `/Users/benniemt/.cache/uv/sdists-v5/editable/4a4b69fa9b468847`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/35/eb/5677556d9ba13436dab51e129f98d4829d95cd1b6bd0e199c14485a4bdb9/numpy-2.1.2-cp313-cp313-macosx_14_0_arm64.whl
DEBUG Building: uv-test-caching @ file:///Users/benniemt/uv_test_caching
DEBUG No workspace root found, using project root
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: hatchling*
DEBUG No cache entry for: https://pypi.org/simple/hatchling/
DEBUG Searching for a compatible version of hatchling (*)
DEBUG No compatible version found for: hatchling
DEBUG Released lock at `/Users/benniemt/.cache/uv/sdists-v5/editable/4a4b69fa9b468847/.lock`
error: Failed to prepare distributions
  Caused by: Failed to build `uv-test-caching @ file:///Users/benniemt/uv_test_caching`
  Caused by: Failed to resolve requirements from `build-system.requires`
  Caused by: No solution found when resolving: `hatchling`
  Caused by: Because hatchling was not found in the cache and you require hatchling, we can conclude that your requirements are unsatisfiable.

hint: Packages were unavailable because the network was disabled. When the network is disabled, registry packages may only be read from the cache.
--------------------------
OFFLINE PROJECT INSTALL W/
EXPLICIT HATCHLING INSTALL
--------------------------
Using CPython 3.13.0 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
DEBUG uv 0.4.27 (Homebrew 2024-10-25)
DEBUG Found project root: `/Users/benniemt/uv_test_caching`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/Users/benniemt/uv_test_caching/.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.13`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: uv-test-caching @ file:///Users/benniemt/uv_test_caching
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 7 packages in 0.53ms
DEBUG Using request timeout of 30s
DEBUG Must revalidate requirement: hatchling
DEBUG Must revalidate requirement: numpy
DEBUG Must revalidate requirement: packaging
DEBUG Must revalidate requirement: pathspec
DEBUG Must revalidate requirement: pluggy
DEBUG Must revalidate requirement: trove-classifiers
DEBUG Must revalidate requirement: uv-test-caching
DEBUG Acquired lock for `/Users/benniemt/.cache/uv/sdists-v5/editable/4a4b69fa9b468847`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/35/eb/5677556d9ba13436dab51e129f98d4829d95cd1b6bd0e199c14485a4bdb9/numpy-2.1.2-cp313-cp313-macosx_14_0_arm64.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/35/35/5055ab8d215af853d07bbff1a74edf48f91ed308f037380a5ca52dd73348/trove_classifiers-2024.10.21.16-py3-none-any.whl
DEBUG Building: uv-test-caching @ file:///Users/benniemt/uv_test_caching
DEBUG No workspace root found, using project root
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: hatchling*
DEBUG No cache entry for: https://pypi.org/simple/hatchling/
DEBUG Searching for a compatible version of hatchling (*)
DEBUG No compatible version found for: hatchling
DEBUG Released lock at `/Users/benniemt/.cache/uv/sdists-v5/editable/4a4b69fa9b468847/.lock`
error: Failed to prepare distributions
  Caused by: Failed to build `uv-test-caching @ file:///Users/benniemt/uv_test_caching`
  Caused by: Failed to resolve requirements from `build-system.requires`
  Caused by: No solution found when resolving: `hatchling`
  Caused by: Because hatchling was not found in the cache and you require hatchling, we can conclude that your requirements are unsatisfiable.

hint: Packages were unavailable because the network was disabled. When the network is disabled, registry packages may only be read from the cache.
```

</p>
</details> 

---

_Comment by @benniekiss on 2024-10-28 21:02_

may be related to or a duplicate of #8414

---

_Comment by @charliermarsh on 2024-10-28 21:09_

I think this is actually all correct behavior, though I understand why it's not intuitive.

The issue is that when you "warm" the cache, you're not actually fetching the metadata you'll need later. This part:

```
# install deps online to make sure they are cached
uv sync --no-install-project --no-dev --group build
```

Will fetch the _distributions_ you need, but not the list of valid versions and metadata, since the lockfile already knows the exact versions that it needs. So this only populates the wheels, but none of the metadata.

Later, when you run:

```
uv sync --no-binary-package=testing --group build --offline --verbose
```

We fail because we need to resolve the build requirements, which are _not_ part of the lockfile. So we're given `["hatchling"]`, and we need to fetch the list of valid `hatchling` versions and do a full resolution to figure out the build environment. At that point, we have to fetch `https://pypi.org/simple/hatchling/` which isn't in the cache.


---

_Comment by @charliermarsh on 2024-10-28 21:11_

It's possible that we could make it such that uv can use the cache as the source of truth for available versions when in `--offline` mode, but honestly the right fix is https://github.com/astral-sh/uv/issues/7052: track the build dependencies in the lockfile itself.

---

_Referenced in [astral-sh/uv#8414](../../astral-sh/uv/issues/8414.md) on 2024-10-28 21:15_

---

_Comment by @benniekiss on 2024-10-28 21:30_

very helpful explanation, thank you! I'll track that issue and close this one

---

_Closed by @benniekiss on 2024-10-28 21:30_

---
