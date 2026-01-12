```yaml
number: 17378
title: "`uv` incorrectly rolls back dependency between build-time and run-time"
type: issue
state: closed
author: AlexanderWells-diamond
labels:
  - question
assignees: []
created_at: 2026-01-09T14:16:33Z
updated_at: 2026-01-12T11:57:32Z
url: https://github.com/astral-sh/uv/issues/17378
synced_at: 2026-01-12T16:02:50Z
```

# `uv` incorrectly rolls back dependency between build-time and run-time

---

_@AlexanderWells-diamond_

### Summary

`uv pip` is choosing a build-time dependency correctly, compiling the package, but then falls back to an older version of the build-time dependency for the final run-time installation. This causes the compiled package to be unable to locate its shared objects, as they are version-stamped. 

This behaviour is different to raw `pip`, which correctly keeps the build-time dependency at run-time. 

The problematic dependency in the below example is `epicscorelibs`. It should be `epicscorelibs==7.0.10.99.0.0`, the most recent released version at time of writing, but version `epicscorelibs==7.0.7.99.1.2` is being mistakenly installed at run-time, despite the right version being used at build-time.

**Reproducing the failure**

```
$ uv venv --seed --python 3.11
Using CPython 3.11.13 interpreter at: /usr/bin/python3.11
Creating virtual environment with seed packages at: .venv
✔ A virtual environment already exists at `.venv`. Do you want to replace it? · yes
 + pip==25.3
 + setuptools==80.9.0
 + wheel==0.45.1
Activate with: source .venv/bin/activate
$ source .venv/bin/activate
$ uv pip install -vv --no-cache --only-binary numpy --no-binary softioc softioc==4.6.1
<output cut for brevity, see excerpts below or full log in attached file>

$ uv pip freeze
epicscorelibs==7.0.7.99.1.2
epicsdbbuilder==1.5
numpy==2.4.0
pip==25.3
pvxslibs==1.4.1
pyyaml==6.0.3
setuptools==80.9.0
setuptools-dso==2.12.2
softioc==4.6.1
wheel==0.45.1

$ uv pip check
Checked 10 packages in 0.50ms
Found 1 incompatibility
The package `softioc` requires `epicscorelibs>=7.0.10.99.0.0,<7.0.10.99.1`, but `7.0.7.99.1.2` is installed

$ python -c "import softioc"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/scratch/eyh46967/dev/temp/uvtest/.venv/lib64/python3.11/site-packages/softioc/__init__.py", line 14, in <module>
    from .imports import dbLoadDatabase, registerRecordDeviceDriver
  File "/scratch/eyh46967/dev/temp/uvtest/.venv/lib64/python3.11/site-packages/softioc/imports.py", line 10, in <module>
    from . import _extension
ImportError: libdbRecStd.so.7.0.10.99.0: cannot open shared object file: No such file or directory

```
[uv_pip_install_output.txt](https://github.com/user-attachments/files/24528448/uv_pip_install_output.txt)

The traceback at the end shows Python attempting to access the 7.0.10 versions of various shared objects, but the ones that are installed are version 7.0.7 - visible with `ls .venv/lib/python3.11/site-packages/epicscorelibs/lib/`. 

The interesting excepts of this logfile, to me, are line 654:
```
TRACE Assigned packages: root==0a0.dev0, setuptools==80.9.0, wheel==0.45.1, setuptools-dso==2.12.2, epicscorelibs==7.0.10.99.0.0, numpy==2.4.0
```
Here we see that the system correctly picks up `epicscorelibs==7.0.10.99.0.0`, and installs it into the build-time environment. 

And then line 1256 shows that the run-time dependency has a lower version of `epicscorelibs` installed, although there seems to be no log explaining why this occurred:
```
Installed 7 packages in 72ms
 + epicscorelibs==7.0.7.99.1.2
 + epicsdbbuilder==1.5
 + numpy==2.4.0
 + pvxslibs==1.4.1
 + pyyaml==6.0.3
 + setuptools-dso==2.12.2
 + softioc==4.6.1
```
Finally, we can see that even `uv` knows something is wrong as the output of `uv pip check` reveals that the wrong version of `epicscorelibs` has been installed.


**Working version with pip**
```
$ uv venv --seed --python 3.11
Using CPython 3.11.13 interpreter at: /usr/bin/python3.11
Creating virtual environment with seed packages at: .venv
✔ A virtual environment already exists at `.venv`. Do you want to replace it? · yes
 + pip==25.3
 + setuptools==80.9.0
 + wheel==0.45.1
Activate with: source .venv/bin/activate
$ source .venv/bin/activate
$ pip install -vv --no-cache --only-binary numpy --no-binary softioc softioc==4.6.1
<output cut for brevity, see excerpts below or full log in attached file>

$ pip freeze
epicscorelibs==7.0.10.99.0.0
epicsdbbuilder==1.5
numpy==2.4.0
pvxslibs==1.5.0
PyYAML==6.0.3
setuptools_dso==2.12.2
softioc==4.6.1
$ pip check
No broken requirements found.

$ python -c "import softioc"
<silent return is correct behaviour>

```
[pip_install_output.txt](https://github.com/user-attachments/files/24528611/pip_install_output.txt)

Interesting lines include line 6834, which lists the installed build-time dependencies (the same as for `uv pip install`, except for the listed order:
```
  Successfully installed epicscorelibs-7.0.10.99.0.0 numpy-2.4.0 setuptools-80.9.0 setuptools_dso-2.12.2 wheel-0.45.1
``` 
And line 14219, which is when it installs the run-time dependencies:
```
Successfully installed epicscorelibs-7.0.10.99.0.0 epicsdbbuilder-1.5 numpy-2.4.0 pvxslibs-1.5.0 pyyaml-6.0.3 setuptools-dso-2.12.2 softioc-4.6.1
```
This list is similar to `uv pip install`, with the notably different change of `epicscorelibs-7.0.10.99.0.0`. Also of note is `pvxslibs-1.5.0` (compared to `pvxslibs-1.4.1` from `uv pip`). This change is understandable as `pvxslibs` has a dependency on `epicscorelibs`, and these versions are the expected ones for the given version of `epicscorelibs`.



**References**
This issue was originally reported in the[ pythonSoftIOC repo here](https://github.com/DiamondLightSource/pythonSoftIOC/issues/197)

The `epicscorelibs` dependency referenced can [be found here](https://github.com/epics-base/epicscorelibs)

### Platform

Linux 4.18.0-553.64.1.el8_10.x86_64 x86_64 GNU/Linux

### Version

uv 0.9.21

### Python version

Python 3.11, 3.12, 3.13

---

_Label `bug` added by @AlexanderWells-diamond on 2026-01-09 14:16_

---

_Comment by @coretl on 2026-01-09 16:22_

I can take a guess at the steps uv is taking from the log file:
- Sees a request to install softioc-4.6.1
- Gets the [wheel metadata from the release targetted at a different version of python](https://files.pythonhosted.org/packages/8f/46/689554640712513ddffdedd452b5ba6627f4fcd78844dd4a489c32bdd22d/softioc-4.6.1-cp310-cp310-macosx_10_9_x86_64.whl.metadata)
- Sees it needs `Requires-Dist: epicscorelibs<7.0.7.99.2,>=7.0.7.99.1.2a1` and installs `epicscorelibs==7.0.7.99.1.2` into the venv
- Gets the sdist for `softioc-4.6.1` and unpacks it
- Sees it needs `epicscorelibs>=7.0.7.99.1.1a3` in [pyproject.toml](https://github.com/DiamondLightSource/pythonSoftIOC/blob/98a76301f4e960d7a7db238ad72db72c5c10f492/pyproject.toml#L2) then installs `7.0.10.99.0.0` into the temporary build env
- Builds `softioc` from source, compiling against `epicscorelibs-7.0.10.99.0.0`, which writes this dependency into the created wheel metadata according to its [setup.py](https://github.com/DiamondLightSource/pythonSoftIOC/blob/98a76301f4e960d7a7db238ad72db72c5c10f492/setup.py#L106)
- Installs the newly built wheel into the venv **without checking its wheel metadata for dependencies**

I am guessing that pip re-checks the newly built wheel for dependencies before installing it into the venv. If that is right, could uv do the same?

I guess it is pretty rare for `setup.py` to calculate the install time dependencies based on what was present in the build environment, which would explain why this hasn't been noticed before.


---

_Comment by @zanieb on 2026-01-09 16:38_

From your description, my first guess is that this is related to metadata inconsistency. We require that metadata across wheels is consistent https://docs.astral.sh/uv/reference/internals/resolver/#metadata-consistent — so if the metadata changes between wheels, we indeed will not respect it.

---

_Comment by @coretl on 2026-01-09 16:58_

From reading that section, I agree with your conclusion. I also agree that all published wheels on PyPI of a particular version of a library should have the same metadata (which is the case for `softioc`). I think this section of the docs is relevant here:

> There are packages that have native code that links against the native code in another package, such as torch. These package may support building against a range of torch versions, but once built, they are constrained to a specific torch version, and the runtime torch version must match the build-time version. These are currently a pain point across all package managers, as all major package managers from pip to uv cache source distribution builds. uv supports multiple builds depending on the version of the already installed package using tool.uv.extra-build-dependencies with match-runtime = true.

This is the case we are hitting here, if `softioc` is built from source, then the built wheel will depend on the version of `epicscorelibs` in the build environment.

Is there any chance that uv could re-invoke its resolver if it has fetched metadata for a library but then finds it has to do a source build of that library?

---

_Comment by @zanieb on 2026-01-09 17:11_

It seems pretty unlikely, this is a fairly fundamental constraint. You might be able to declare it as an extra build dependency and use `match-runtime = true` so our resolver uses a matching version in both environments?

---

_Label `bug` removed by @konstin on 2026-01-12 09:42_

---

_Label `question` added by @konstin on 2026-01-12 09:42_

---

_Comment by @konstin on 2026-01-12 09:48_

> Is there any chance that uv could re-invoke its resolver if it has fetched metadata for a library but then finds it has to do a source build of that library?

Doing this generally (beyond `match-runtime = true`) breaks the assumptions uv (and other tools too) make that a source distribution can be used the same as a wheel when there is no matching wheel, that a wheel built from source distribution can be reused for further installation (the alternative would be rebuilding all source distributions on each installation) and that you can build a consistent cross-platform lockfile from a set of requirements even without building for all platforms.

---

_Comment by @coretl on 2026-01-12 10:59_

Are you saying that adding ` tool.uv.extra-build-dependencies` with `match-runtime = true` to the `pyproject.toml` of `softioc` will mean that when you `uv pip install --no-binary softioc` it would use the version of `epicscorelibs` from the environment rather than what is specified in the wheel? Or that the user of `softioc` would need to make a `pyproject.toml` for that project with `match-runtime = true` inside it to get this behaviour?

---

_Comment by @konstin on 2026-01-12 11:44_

`match-runtime = true` needs to be applied be the user and is built around `pyproject.toml` workflows, to be used with `uv sync` rather than with `uv pip install`. More concretely, for `match-runtime` to be applied, it needs to be in your `pyproject.toml`, it will be ignored in `pyproject.toml` files in the dependency tree.

---

_Comment by @AlexanderWells-diamond on 2026-01-12 11:57_

Thank you for the information. As there's no bug here I'll close the issue. We'll add some docs to our repository telling our `uv` users they may need to add `match-runtime = true` to their configuration.

---

_Closed by @AlexanderWells-diamond on 2026-01-12 11:57_

---
