---
number: 4053
title: Non-deterministic? behavior if a requirements.txt contains a dependency which is also specified to be installed editable
type: issue
state: closed
author: Julian
labels:
  - bug
assignees: []
created_at: 2024-06-05T17:04:46Z
updated_at: 2024-06-10T19:02:18Z
url: https://github.com/astral-sh/uv/issues/4053
synced_at: 2026-01-10T01:23:34Z
---

# Non-deterministic? behavior if a requirements.txt contains a dependency which is also specified to be installed editable

---

_Issue opened by @Julian on 2024-06-05 17:04_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Consider a trivial (empty) package `foo` with a requirements.txt file containing just `file:.`, i.e. "install this package".

During development, one of course often wants to install the package under development in editable mode, effectively attempting to override the requirement. Note that in pip, that does not work (and I personally generally resort to simply running 2 commands).

More specifically, the following command results in the successive output when using pip:

```
⊙  rm -rf venv && python3.12 -m venv venv && venv=$PWD/venv/bin/python && echo $venv && $venv -m pip install -r requirements.txt -e . && printf 'Empty:\n' && printf '\n' >foo/__init__.py && $venv -c 'import foo' && printf 'Modifying...\n' && printf 'print("MODIFIED")\n' >foo/__init__.py && $venv -c 'import foo' && cd /tmp && $venv -c 'import foo' && cd -         
/Users/julian/Desktop/foo/venv/bin/python
Obtaining file:///Users/julian/Desktop/foo
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... done
  Installing backend dependencies ... done
  Preparing editable metadata (pyproject.toml) ... done
Processing /Users/julian/Desktop/foo
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
ERROR: Cannot install foo 0.1.dev1+g81dbecd (from .) and foo 0.1.dev1+g81dbecd (from /Users/julian/Desktop/foo) because these package versions have conflicting dependencies.

The conflict is caused by:
    The user requested foo 0.1.dev1+g81dbecd (from /Users/julian/Desktop/foo)
    The user requested foo 0.1.dev1+g81dbecd (from .)

To fix this you could try to:
1. loosen the range of package versions you've specified
2. remove package versions to allow pip attempt to solve the dependency conflict
```

I believe there is an issue somewhere on the pip tracker to loosen this constraint, as of course it *is* possible to simultaneously satisfy the two requirements, but I haven't gone and found it at the minute.

With uv however, no error is emitted, which made me think this works in uv, but it has subtly non-deterministic behavior (or at least I can't tell what it's doing, and it doesn't appear that the package is installed in editable mode). Specifically here's the behavior I get from running something like the above commands twice, where we're installing simultaneously, modifying the file, then checking to see if Python sees our modifications:

```
⊙  rm -rf venv && uv venv venv && venv=$PWD/venv/bin/python && echo $venv && uv pip install --verbose --python $venv -r requirements.txt -e . && printf 'Empty:\n' && printf '\n' >foo/__init__.py && $venv -c 'import foo' && printf 'Modifying...\n' && printf 'print("FIRST")\n' >foo/__init__.py && $venv -c 'import foo' && printf 'print("SECOND")\n' >foo/__init__.py && cd /tmp && $venv -c 'import foo' && cd -
Using Python 3.12.3 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtualenv at: venv
Activate with: source venv/bin/activate
/Users/julian/Desktop/foo/venv/bin/python
DEBUG Checking for Python interpreter at path `venv/bin/python`
DEBUG Using Python 3.12.3 environment at venv/bin/python
DEBUG Acquired lock for `venv`
DEBUG At least one requirement is not satisfied: file:///Users/julian/Desktop/foo
DEBUG Using registry request timeout of 30s
DEBUG Found PEP 621 metadata for /Users/julian/Desktop/foo in `pyproject.toml` (foo)
DEBUG Found PEP 621 metadata for /Users/julian/Desktop/foo in `pyproject.toml` (foo)
DEBUG Acquired lock for `/Users/julian/Library/Caches/uv/built-wheels-v3/editable/865209b8dda329ac`
DEBUG Preparing metadata for: foo @ file:///Users/julian/Desktop/foo
DEBUG No static `PKG-INFO` available for: foo @ file:///Users/julian/Desktop/foo (MissingPkgInfo)
DEBUG No static `pyproject.toml` available for: foo @ file:///Users/julian/Desktop/foo (DynamicPyprojectToml(DynamicField("version")))
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: hatch-vcs*
DEBUG Found fresh response for: https://pypi.org/simple/hatch-vcs/
DEBUG Found fresh response for: https://pypi.org/simple/hatchling/
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.24.2 (hatchling-1.24.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/82/0f/6cbd9976160bc334add63bc2e7a58b1433a31b34b7cda6c5de6dd983d9a7/hatch_vcs-0.4.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5f/20/b890241570ac3ba6508febab3104eeb1acd7ffba1bd3447190f56151f1bc/hatchling-1.24.2-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for hatchling==1.24.2: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.24.2: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.24.2: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.24.2: trove-classifiers*
DEBUG Searching for a compatible version of hatch-vcs (*)
DEBUG Selecting: hatch-vcs==0.4.0 (hatch_vcs-0.4.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: hatchling>=1.1.0
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: setuptools-scm>=6.4.0
DEBUG Found fresh response for: https://pypi.org/simple/pluggy/
DEBUG Found fresh response for: https://pypi.org/simple/pathspec/
DEBUG Found fresh response for: https://pypi.org/simple/trove-classifiers/
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/setuptools-scm/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/49/df/1fceb2f8900f8639e278b056416d49134fb8d84c5942ffaa01ad34782422/packaging-24.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.5.22 (trove_classifiers-2024.5.22-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/00/87/c8ccf265dea5f96a1e930e69b97214255bbe284ca78e450aeaba7d021bdb/trove_classifiers-2024.5.22-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of setuptools-scm (>=6.4.0)
DEBUG Selecting: setuptools-scm==8.1.0 (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a0/b9/1906bfeb30f2fc13bb39bf7ddb8749784c05faadbd18a21cf141ba37bff2/setuptools_scm-8.1.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==70.0.0 (setuptools-70.0.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/de/88/70c5767a0e43eb4451c2200f07d042a4bcd7639276003a9c54a68cfcc1f8/setuptools-70.0.0-py3-none-any.whl.metadata
DEBUG Tried 9 versions: hatch-vcs 1, hatchling 1, packaging 1, pathspec 1, pluggy 1, root 1, setuptools 1, setuptools-scm 1, trove-classifiers 1
DEBUG Installing in hatch-vcs==0.4.0, hatchling==1.24.2, packaging==24.0, pathspec==0.12.1, pluggy==1.5.0, setuptools==70.0.0, setuptools-scm==8.1.0, trove-classifiers==2024.5.22 in /Users/julian/Library/Caches/uv/.tmphH0mHN/.venv
DEBUG Requirement already cached: hatch-vcs==0.4.0
DEBUG Requirement already cached: hatchling==1.24.2
DEBUG Requirement already cached: packaging==24.0
DEBUG Requirement already cached: pathspec==0.12.1
DEBUG Requirement already cached: pluggy==1.5.0
DEBUG Requirement already cached: setuptools==70.0.0
DEBUG Requirement already cached: setuptools-scm==8.1.0
DEBUG Requirement already cached: trove-classifiers==2024.5.22
DEBUG Installing build requirements: hatch-vcs==0.4.0, hatchling==1.24.2, packaging==24.0, pathspec==0.12.1, pluggy==1.5.0, setuptools==70.0.0, setuptools-scm==8.1.0, trove-classifiers==2024.5.22
DEBUG Calling `hatchling.build.get_requires_for_build_editable()`
DEBUG Installing extra requirements for build backend
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: hatch-vcs*
DEBUG Adding direct dependency: editables>=0.3, <1.dev0
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.24.2 (hatchling-1.24.2-py3-none-any.whl)
DEBUG Adding transitive dependency for hatchling==1.24.2: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.24.2: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.24.2: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.24.2: trove-classifiers*
DEBUG Searching for a compatible version of hatch-vcs (*)
DEBUG Selecting: hatch-vcs==0.4.0 (hatch_vcs-0.4.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: hatchling>=1.1.0
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: setuptools-scm>=6.4.0
DEBUG Found stale response for: https://pypi.org/simple/editables/
DEBUG Sending revalidation request for: https://pypi.org/simple/editables/
DEBUG Found not-modified response for: https://pypi.org/simple/editables/
DEBUG Searching for a compatible version of editables (>=0.3, <1.dev0)
DEBUG Selecting: editables==0.5 (editables-0.5-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6b/be/0f2f4a5e8adc114a02b63d92bf8edbfa24db6fc602fca83c885af2479e0e/editables-0.5-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.5.22 (trove_classifiers-2024.5.22-py3-none-any.whl)
DEBUG Searching for a compatible version of setuptools-scm (>=6.4.0)
DEBUG Selecting: setuptools-scm==8.1.0 (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==70.0.0 (setuptools-70.0.0-py3-none-any.whl)
DEBUG Tried 10 versions: editables 1, hatch-vcs 1, hatchling 1, packaging 1, pathspec 1, pluggy 1, root 1, setuptools 1, setuptools-scm 1, trove-classifiers 1
DEBUG Installing in editables==0.5, hatch-vcs==0.4.0, hatchling==1.24.2, packaging==24.0, pathspec==0.12.1, pluggy==1.5.0, setuptools==70.0.0, setuptools-scm==8.1.0, trove-classifiers==2024.5.22 in /Users/julian/Library/Caches/uv/.tmphH0mHN/.venv
DEBUG Requirement already cached: editables==0.5
DEBUG Requirement already installed: hatch-vcs==0.4.0
DEBUG Requirement already installed: hatchling==1.24.2
DEBUG Requirement already installed: packaging==24.0
DEBUG Requirement already installed: pathspec==0.12.1
DEBUG Requirement already installed: pluggy==1.5.0
DEBUG Requirement already installed: setuptools==70.0.0
DEBUG Requirement already installed: setuptools-scm==8.1.0
DEBUG Requirement already installed: trove-classifiers==2024.5.22
DEBUG Installing build requirement: editables==0.5
DEBUG Calling `hatchling.build.prepare_metadata_for_build_editable()`
DEBUG Prepared metadata for: foo @ file:///Users/julian/Desktop/foo
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/Users/julian/Library/Caches/uv/built-wheels-v3/path/865209b8dda329ac`
DEBUG Preparing metadata for: foo @ file:///Users/julian/Desktop/foo
DEBUG No static `PKG-INFO` available for: foo @ file:///Users/julian/Desktop/foo (MissingPkgInfo)
DEBUG No static `pyproject.toml` available for: foo @ file:///Users/julian/Desktop/foo (DynamicPyprojectToml(DynamicField("version")))
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: hatch-vcs*
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.24.2 (hatchling-1.24.2-py3-none-any.whl)
DEBUG Adding transitive dependency for hatchling==1.24.2: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.24.2: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.24.2: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.24.2: trove-classifiers*
DEBUG Searching for a compatible version of hatch-vcs (*)
DEBUG Selecting: hatch-vcs==0.4.0 (hatch_vcs-0.4.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: hatchling>=1.1.0
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: setuptools-scm>=6.4.0
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.5.22 (trove_classifiers-2024.5.22-py3-none-any.whl)
DEBUG Searching for a compatible version of setuptools-scm (>=6.4.0)
DEBUG Selecting: setuptools-scm==8.1.0 (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==70.0.0 (setuptools-70.0.0-py3-none-any.whl)
DEBUG Tried 9 versions: hatch-vcs 1, hatchling 1, packaging 1, pathspec 1, pluggy 1, root 1, setuptools 1, setuptools-scm 1, trove-classifiers 1
DEBUG Installing in hatch-vcs==0.4.0, hatchling==1.24.2, packaging==24.0, pathspec==0.12.1, pluggy==1.5.0, setuptools==70.0.0, setuptools-scm==8.1.0, trove-classifiers==2024.5.22 in /Users/julian/Library/Caches/uv/.tmpyILCIY/.venv
DEBUG Requirement already cached: hatch-vcs==0.4.0
DEBUG Requirement already cached: hatchling==1.24.2
DEBUG Requirement already cached: packaging==24.0
DEBUG Requirement already cached: pathspec==0.12.1
DEBUG Requirement already cached: pluggy==1.5.0
DEBUG Requirement already cached: setuptools==70.0.0
DEBUG Requirement already cached: setuptools-scm==8.1.0
DEBUG Requirement already cached: trove-classifiers==2024.5.22
DEBUG Installing build requirements: hatch-vcs==0.4.0, hatchling==1.24.2, packaging==24.0, pathspec==0.12.1, pluggy==1.5.0, setuptools==70.0.0, setuptools-scm==8.1.0, trove-classifiers==2024.5.22
DEBUG Calling `hatchling.build.get_requires_for_build_wheel()`
DEBUG Calling `hatchling.build.prepare_metadata_for_build_wheel()`
DEBUG Prepared metadata for: foo @ file:///Users/julian/Desktop/foo
DEBUG No workspace root found, using project root
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: foo*
DEBUG Adding direct dependency: foo*
DEBUG Searching for a compatible version of foo @ file:///Users/julian/Desktop/foo (*)
DEBUG Tried 2 versions: foo 1, root 1
Resolved 1 package in 1.04s
DEBUG Identified uncached requirement: foo @ file:///Users/julian/Desktop/foo
DEBUG Acquired lock for `/Users/julian/Library/Caches/uv/built-wheels-v3/path/865209b8dda329ac`
DEBUG Building: foo @ file:///Users/julian/Desktop/foo
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: hatch-vcs*
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.24.2 (hatchling-1.24.2-py3-none-any.whl)
DEBUG Adding transitive dependency for hatchling==1.24.2: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.24.2: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.24.2: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.24.2: trove-classifiers*
DEBUG Searching for a compatible version of hatch-vcs (*)
DEBUG Selecting: hatch-vcs==0.4.0 (hatch_vcs-0.4.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: hatchling>=1.1.0
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: setuptools-scm>=6.4.0
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.5.22 (trove_classifiers-2024.5.22-py3-none-any.whl)
DEBUG Searching for a compatible version of setuptools-scm (>=6.4.0)
DEBUG Selecting: setuptools-scm==8.1.0 (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==70.0.0 (setuptools-70.0.0-py3-none-any.whl)
DEBUG Tried 9 versions: hatch-vcs 1, hatchling 1, packaging 1, pathspec 1, pluggy 1, root 1, setuptools 1, setuptools-scm 1, trove-classifiers 1
DEBUG Installing in hatch-vcs==0.4.0, hatchling==1.24.2, packaging==24.0, pathspec==0.12.1, pluggy==1.5.0, setuptools==70.0.0, setuptools-scm==8.1.0, trove-classifiers==2024.5.22 in /Users/julian/Library/Caches/uv/.tmptcdfFf/.venv
DEBUG Requirement already cached: hatch-vcs==0.4.0
DEBUG Requirement already cached: hatchling==1.24.2
DEBUG Requirement already cached: packaging==24.0
DEBUG Requirement already cached: pathspec==0.12.1
DEBUG Requirement already cached: pluggy==1.5.0
DEBUG Requirement already cached: setuptools==70.0.0
DEBUG Requirement already cached: setuptools-scm==8.1.0
DEBUG Requirement already cached: trove-classifiers==2024.5.22
DEBUG Installing build requirements: hatch-vcs==0.4.0, hatchling==1.24.2, packaging==24.0, pathspec==0.12.1, pluggy==1.5.0, setuptools==70.0.0, setuptools-scm==8.1.0, trove-classifiers==2024.5.22
DEBUG Calling `hatchling.build.get_requires_for_build_wheel()`
DEBUG Calling `hatchling.build.build_wheel("/Users/julian/Library/Caches/uv/built-wheels-v3/path/865209b8dda329ac/ahCsxAvuODcIU4xUw99Pw/.tmpLAhMzR", {}, None)`
DEBUG Finished building: foo @ file:///Users/julian/Desktop/foo
Downloaded 1 package in 352ms
Installed 1 package in 0.70ms
 + foo==0.1.dev1+g81dbecd (from file:///Users/julian/Desktop/foo)
Empty:
Modifying...
FIRST
SECOND
~/Desktop/foo

~/Desktop/foo is a git repository on main 
⊙  rm -rf venv && uv venv venv && venv=$PWD/venv/bin/python && echo $venv && uv pip install --verbose --python $venv -r requirements.txt -e . && printf 'Empty:\n' && printf '\n' >foo/__init__.py && $venv -c 'import foo' && printf 'Modifying...\n' && printf 'print("1")\n' >foo/__init__.py && $venv -c 'import foo' && printf 'print("2")\n' >foo/__init__.py && cd /tmp && $venv -c 'import foo' && cd -     
Using Python 3.12.3 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtualenv at: venv
Activate with: source venv/bin/activate
/Users/julian/Desktop/foo/venv/bin/python
DEBUG Checking for Python interpreter at path `venv/bin/python`
DEBUG Using Python 3.12.3 environment at venv/bin/python
DEBUG Acquired lock for `venv`
DEBUG At least one requirement is not satisfied: file:///Users/julian/Desktop/foo
DEBUG Using registry request timeout of 30s
DEBUG Found PEP 621 metadata for /Users/julian/Desktop/foo in `pyproject.toml` (foo)
DEBUG Found PEP 621 metadata for /Users/julian/Desktop/foo in `pyproject.toml` (foo)
DEBUG Acquired lock for `/Users/julian/Library/Caches/uv/built-wheels-v3/editable/865209b8dda329ac`
DEBUG Preparing metadata for: foo @ file:///Users/julian/Desktop/foo
DEBUG No static `PKG-INFO` available for: foo @ file:///Users/julian/Desktop/foo (MissingPkgInfo)
DEBUG No static `pyproject.toml` available for: foo @ file:///Users/julian/Desktop/foo (DynamicPyprojectToml(DynamicField("version")))
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: hatch-vcs*
DEBUG Found fresh response for: https://pypi.org/simple/hatchling/
DEBUG Found fresh response for: https://pypi.org/simple/hatch-vcs/
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.24.2 (hatchling-1.24.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5f/20/b890241570ac3ba6508febab3104eeb1acd7ffba1bd3447190f56151f1bc/hatchling-1.24.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/82/0f/6cbd9976160bc334add63bc2e7a58b1433a31b34b7cda6c5de6dd983d9a7/hatch_vcs-0.4.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for hatchling==1.24.2: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.24.2: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.24.2: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.24.2: trove-classifiers*
DEBUG Searching for a compatible version of hatch-vcs (*)
DEBUG Selecting: hatch-vcs==0.4.0 (hatch_vcs-0.4.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: hatchling>=1.1.0
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: setuptools-scm>=6.4.0
DEBUG Found fresh response for: https://pypi.org/simple/pluggy/
DEBUG Found fresh response for: https://pypi.org/simple/pathspec/
DEBUG Found fresh response for: https://pypi.org/simple/trove-classifiers/
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/00/87/c8ccf265dea5f96a1e930e69b97214255bbe284ca78e450aeaba7d021bdb/trove_classifiers-2024.5.22-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/setuptools-scm/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a0/b9/1906bfeb30f2fc13bb39bf7ddb8749784c05faadbd18a21cf141ba37bff2/setuptools_scm-8.1.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/49/df/1fceb2f8900f8639e278b056416d49134fb8d84c5942ffaa01ad34782422/packaging-24.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.5.22 (trove_classifiers-2024.5.22-py3-none-any.whl)
DEBUG Searching for a compatible version of setuptools-scm (>=6.4.0)
DEBUG Selecting: setuptools-scm==8.1.0 (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==70.0.0 (setuptools-70.0.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/de/88/70c5767a0e43eb4451c2200f07d042a4bcd7639276003a9c54a68cfcc1f8/setuptools-70.0.0-py3-none-any.whl.metadata
DEBUG Tried 9 versions: hatch-vcs 1, hatchling 1, packaging 1, pathspec 1, pluggy 1, root 1, setuptools 1, setuptools-scm 1, trove-classifiers 1
DEBUG Installing in hatch-vcs==0.4.0, hatchling==1.24.2, packaging==24.0, pathspec==0.12.1, pluggy==1.5.0, setuptools==70.0.0, setuptools-scm==8.1.0, trove-classifiers==2024.5.22 in /Users/julian/Library/Caches/uv/.tmpGuDG1Z/.venv
DEBUG Requirement already cached: hatch-vcs==0.4.0
DEBUG Requirement already cached: hatchling==1.24.2
DEBUG Requirement already cached: packaging==24.0
DEBUG Requirement already cached: pathspec==0.12.1
DEBUG Requirement already cached: pluggy==1.5.0
DEBUG Requirement already cached: setuptools==70.0.0
DEBUG Requirement already cached: setuptools-scm==8.1.0
DEBUG Requirement already cached: trove-classifiers==2024.5.22
DEBUG Installing build requirements: hatch-vcs==0.4.0, hatchling==1.24.2, packaging==24.0, pathspec==0.12.1, pluggy==1.5.0, setuptools==70.0.0, setuptools-scm==8.1.0, trove-classifiers==2024.5.22
DEBUG Calling `hatchling.build.get_requires_for_build_editable()`
DEBUG Installing extra requirements for build backend
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: hatch-vcs*
DEBUG Adding direct dependency: editables>=0.3, <1.dev0
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.24.2 (hatchling-1.24.2-py3-none-any.whl)
DEBUG Adding transitive dependency for hatchling==1.24.2: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.24.2: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.24.2: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.24.2: trove-classifiers*
DEBUG Searching for a compatible version of hatch-vcs (*)
DEBUG Selecting: hatch-vcs==0.4.0 (hatch_vcs-0.4.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: hatchling>=1.1.0
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: setuptools-scm>=6.4.0
DEBUG Found fresh response for: https://pypi.org/simple/editables/
DEBUG Searching for a compatible version of editables (>=0.3, <1.dev0)
DEBUG Selecting: editables==0.5 (editables-0.5-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6b/be/0f2f4a5e8adc114a02b63d92bf8edbfa24db6fc602fca83c885af2479e0e/editables-0.5-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.5.22 (trove_classifiers-2024.5.22-py3-none-any.whl)
DEBUG Searching for a compatible version of setuptools-scm (>=6.4.0)
DEBUG Selecting: setuptools-scm==8.1.0 (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==70.0.0 (setuptools-70.0.0-py3-none-any.whl)
DEBUG Tried 10 versions: editables 1, hatch-vcs 1, hatchling 1, packaging 1, pathspec 1, pluggy 1, root 1, setuptools 1, setuptools-scm 1, trove-classifiers 1
DEBUG Installing in editables==0.5, hatch-vcs==0.4.0, hatchling==1.24.2, packaging==24.0, pathspec==0.12.1, pluggy==1.5.0, setuptools==70.0.0, setuptools-scm==8.1.0, trove-classifiers==2024.5.22 in /Users/julian/Library/Caches/uv/.tmpGuDG1Z/.venv
DEBUG Requirement already cached: editables==0.5
DEBUG Requirement already installed: hatch-vcs==0.4.0
DEBUG Requirement already installed: hatchling==1.24.2
DEBUG Requirement already installed: packaging==24.0
DEBUG Requirement already installed: pathspec==0.12.1
DEBUG Requirement already installed: pluggy==1.5.0
DEBUG Requirement already installed: setuptools==70.0.0
DEBUG Requirement already installed: setuptools-scm==8.1.0
DEBUG Requirement already installed: trove-classifiers==2024.5.22
DEBUG Installing build requirement: editables==0.5
DEBUG Calling `hatchling.build.prepare_metadata_for_build_editable()`
DEBUG Prepared metadata for: foo @ file:///Users/julian/Desktop/foo
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/Users/julian/Library/Caches/uv/built-wheels-v3/path/865209b8dda329ac`
DEBUG Preparing metadata for: foo @ file:///Users/julian/Desktop/foo
DEBUG No static `PKG-INFO` available for: foo @ file:///Users/julian/Desktop/foo (MissingPkgInfo)
DEBUG No static `pyproject.toml` available for: foo @ file:///Users/julian/Desktop/foo (DynamicPyprojectToml(DynamicField("version")))
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: hatch-vcs*
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.24.2 (hatchling-1.24.2-py3-none-any.whl)
DEBUG Adding transitive dependency for hatchling==1.24.2: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.24.2: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.24.2: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.24.2: trove-classifiers*
DEBUG Searching for a compatible version of hatch-vcs (*)
DEBUG Selecting: hatch-vcs==0.4.0 (hatch_vcs-0.4.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: hatchling>=1.1.0
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: setuptools-scm>=6.4.0
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.5.22 (trove_classifiers-2024.5.22-py3-none-any.whl)
DEBUG Searching for a compatible version of setuptools-scm (>=6.4.0)
DEBUG Selecting: setuptools-scm==8.1.0 (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==70.0.0 (setuptools-70.0.0-py3-none-any.whl)
DEBUG Tried 9 versions: hatch-vcs 1, hatchling 1, packaging 1, pathspec 1, pluggy 1, root 1, setuptools 1, setuptools-scm 1, trove-classifiers 1
DEBUG Installing in hatch-vcs==0.4.0, hatchling==1.24.2, packaging==24.0, pathspec==0.12.1, pluggy==1.5.0, setuptools==70.0.0, setuptools-scm==8.1.0, trove-classifiers==2024.5.22 in /Users/julian/Library/Caches/uv/.tmpiDZtHF/.venv
DEBUG Requirement already cached: hatch-vcs==0.4.0
DEBUG Requirement already cached: hatchling==1.24.2
DEBUG Requirement already cached: packaging==24.0
DEBUG Requirement already cached: pathspec==0.12.1
DEBUG Requirement already cached: pluggy==1.5.0
DEBUG Requirement already cached: setuptools==70.0.0
DEBUG Requirement already cached: setuptools-scm==8.1.0
DEBUG Requirement already cached: trove-classifiers==2024.5.22
DEBUG Installing build requirements: hatch-vcs==0.4.0, hatchling==1.24.2, packaging==24.0, pathspec==0.12.1, pluggy==1.5.0, setuptools==70.0.0, setuptools-scm==8.1.0, trove-classifiers==2024.5.22
DEBUG Calling `hatchling.build.get_requires_for_build_wheel()`
DEBUG Calling `hatchling.build.prepare_metadata_for_build_wheel()`
DEBUG Prepared metadata for: foo @ file:///Users/julian/Desktop/foo
DEBUG No workspace root found, using project root
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: foo*
DEBUG Adding direct dependency: foo*
DEBUG Searching for a compatible version of foo @ file:///Users/julian/Desktop/foo (*)
DEBUG Tried 2 versions: foo 1, root 1
Resolved 1 package in 780ms
DEBUG Identified uncached requirement: foo @ file:///Users/julian/Desktop/foo
DEBUG Acquired lock for `/Users/julian/Library/Caches/uv/built-wheels-v3/path/865209b8dda329ac`
DEBUG Building: foo @ file:///Users/julian/Desktop/foo
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: hatch-vcs*
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.24.2 (hatchling-1.24.2-py3-none-any.whl)
DEBUG Adding transitive dependency for hatchling==1.24.2: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.24.2: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.24.2: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.24.2: trove-classifiers*
DEBUG Searching for a compatible version of hatch-vcs (*)
DEBUG Selecting: hatch-vcs==0.4.0 (hatch_vcs-0.4.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: hatchling>=1.1.0
DEBUG Adding transitive dependency for hatch-vcs==0.4.0: setuptools-scm>=6.4.0
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.0 (packaging-24.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.5.22 (trove_classifiers-2024.5.22-py3-none-any.whl)
DEBUG Searching for a compatible version of setuptools-scm (>=6.4.0)
DEBUG Selecting: setuptools-scm==8.1.0 (setuptools_scm-8.1.0-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==8.1.0: setuptools*
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==70.0.0 (setuptools-70.0.0-py3-none-any.whl)
DEBUG Tried 9 versions: hatch-vcs 1, hatchling 1, packaging 1, pathspec 1, pluggy 1, root 1, setuptools 1, setuptools-scm 1, trove-classifiers 1
DEBUG Installing in hatch-vcs==0.4.0, hatchling==1.24.2, packaging==24.0, pathspec==0.12.1, pluggy==1.5.0, setuptools==70.0.0, setuptools-scm==8.1.0, trove-classifiers==2024.5.22 in /Users/julian/Library/Caches/uv/.tmpdGWv66/.venv
DEBUG Requirement already cached: hatch-vcs==0.4.0
DEBUG Requirement already cached: hatchling==1.24.2
DEBUG Requirement already cached: packaging==24.0
DEBUG Requirement already cached: pathspec==0.12.1
DEBUG Requirement already cached: pluggy==1.5.0
DEBUG Requirement already cached: setuptools==70.0.0
DEBUG Requirement already cached: setuptools-scm==8.1.0
DEBUG Requirement already cached: trove-classifiers==2024.5.22
DEBUG Installing build requirements: hatch-vcs==0.4.0, hatchling==1.24.2, packaging==24.0, pathspec==0.12.1, pluggy==1.5.0, setuptools==70.0.0, setuptools-scm==8.1.0, trove-classifiers==2024.5.22
DEBUG Calling `hatchling.build.get_requires_for_build_wheel()`
DEBUG Calling `hatchling.build.build_wheel("/Users/julian/Library/Caches/uv/built-wheels-v3/path/865209b8dda329ac/FbM6kNSTVxMSFcuMCja34/.tmpuvtH9o", {}, None)`
DEBUG Finished building: foo @ file:///Users/julian/Desktop/foo
Downloaded 1 package in 359ms
Installed 1 package in 0.80ms
 + foo==0.1.dev1+g81dbecd (from file:///Users/julian/Desktop/foo)
Empty:
Modifying...
1
SECOND
```

Note that the first time around everything seems fine (and the package seems like modifications are being seen), but the second time around, some caching seems to be visible, and our modification *isn't* seen / Python seems to see an old version of the package. In transparency, I haven't tried to diagnose this, so it's possible this is say, a hatch bug (which happens to be the build system I'm using in the above trivial package). But I thought I'd raise it in case you have thoughts, and also because if I had to wager, it'd seem slightly more likely to be uv than hatch that was in control there.

I'll note also that I *don't* seem to observe this behavior if I don't install out of a requirements.txt... specifically if in the above commands I use `uv pip install 'file:.' -e .`, I seem to consistently see the package "properly" installed editable.

(Thanks as usual for a great project!)

---

_Comment by @charliermarsh on 2024-06-10 17:47_

I think this is probably is non-deterministic somewhere.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-10 17:50_

---

_Label `bug` added by @charliermarsh on 2024-06-10 17:51_

---

_Comment by @charliermarsh on 2024-06-10 18:13_

So in my own testing, it does deterministically install as _not_ editable above.

---

_Comment by @charliermarsh on 2024-06-10 18:14_

However, when I do install as editable, I sometimes don't see changes reflected, but I think it's due to CPython's bytecode caching.

The command I'm testing with (from `./scripts/packages/black_editable` in this repo):

```
rm -rf venv && uv venv venv && venv=$PWD/venv/bin/python && rm black/__init__.py && touch black/__init__.py && echo $venv && uv pip install --verbose --python $venv -r requirements.txt -e . && printf 'Empty:\n' && printf '\n' >black/__init__.py && $venv -B -I -c 'import black' && printf 'Modifying...\n' && printf 'print("1")\n' >black/__init__.py && $venv -B -I -c 'import black' && printf 'print("2")\n' >black/__init__.py && $venv -B -I -c 'import black'
```

This consistently gives:

```
Empty:
Modifying...
```

If you install as editable in the requirements file, and omits the `-B`, then you see:

```
Empty:
Modifying...
1
1
```

If you include the `-B`:

```
Empty:
Modifying...
1
2
```


---

_Comment by @charliermarsh on 2024-06-10 18:15_

I think I'm going to change this to prefer editables though.

---

_Referenced in [astral-sh/uv#4208](../../astral-sh/uv/pulls/4208.md) on 2024-06-10 18:40_

---

_Closed by @charliermarsh on 2024-06-10 19:02_

---
