```yaml
number: 17635
title: "Trying to build package inside venv with debug python build fails with \"the built wheel .. is not compatbile with the current Python ...\""
type: issue
state: open
author: ikrommyd
labels:
  - bug
assignees: []
created_at: 2026-01-21T00:53:25Z
updated_at: 2026-01-21T00:54:10Z
url: https://github.com/astral-sh/uv/issues/17635
synced_at: 2026-01-21T02:01:23Z
```

# Trying to build package inside venv with debug python build fails with "the built wheel .. is not compatbile with the current Python ..."

---

_@ikrommyd_

### Summary

I'm trying to install https://github.com/cms-nanoAOD/correctionlib in a local venv created with `uv venv` with `uv pip install .`. which normally works fine. However, this time I created my venv using a debug + thread sanitized local build of python using `uv venv -p /Users/iason/.pyenv/versions/3.14t-dev-tsan-debug/bin/python`.
Trying to install the package now with `uv pip install -v .` errors with 
```
The built wheel `correctionlib-2.7.1.dev14+gb6a34853d.d20260121-cp314-cp314-macosx_26_0_arm64.whl` is not compatible with the current Python 3.14 on macos aarch64
```
Using `pip` install of `uv pip` works fine with the bottom of the `pip` log being
```
  *** Making wheel...
  *** Created correctionlib-2.7.1.dev14+gb6a34853d.d20260121-cp314-cp314td-macosx_26_0_arm64.whl
  Building wheel for correctionlib (pyproject.toml) ... done
  Created wheel for correctionlib: filename=correctionlib-2.7.1.dev14+gb6a34853d.d20260121-cp314-cp314td-macosx_26_0_arm64.whl size=543674 sha256=36fa4fffa1916682dfb264bcc1950ab59094ba05ec3352d2827bf201f984aa30
  Stored in directory: /private/var/folders/2c/94kt72fs0_q_9nzr2y5jphb00000gn/T/pip-ephem-wheel-cache-fbztx37s/wheels/f0/67/3a/c6e7050eec435cb4efaaef7cd7184330f7741c0d11a62806b9
Successfully built correctionlib
Installing collected packages: correctionlib
  Attempting uninstall: correctionlib
    Found existing installation: correctionlib 2.7.1.dev14+gb6a34853d.d20260121
    Uninstalling correctionlib-2.7.1.dev14+gb6a34853d.d20260121:
      Removing file or directory /Users/iason/Dropbox/work/pyhep_dev/correctionlib/.venv/bin/correction
      Removing file or directory /Users/iason/Dropbox/work/pyhep_dev/correctionlib/.venv/lib/python3.14t/site-packages/correctionlib-2.7.1.dev14+gb6a34853d.d20260121.dist-info/
      Removing file or directory /Users/iason/Dropbox/work/pyhep_dev/correctionlib/.venv/lib/python3.14t/site-packages/correctionlib/
      Successfully uninstalled correctionlib-2.7.1.dev14+gb6a34853d.d20260121
  changing mode of /Users/iason/Dropbox/work/pyhep_dev/correctionlib/.venv/bin/correction to 755
Successfully installed correctionlib-2.7.1.dev14+gb6a34853d.d20260121
```
Here is the full uv log:
```shell
❯ uv pip install -v .
DEBUG uv 0.9.26 (ee4f00362 2026-01-15)
DEBUG Marking explicit source tree for reinstall: `/Users/iason/Dropbox/work/pyhep_dev/correctionlib`
DEBUG Acquired shared lock for `/Users/iason/.cache/uv`
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.14.2+freethreaded+debug-macos-aarch64-none` at `/Users/iason/Dropbox/work/pyhep_dev/correctionlib/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.14.2 environment at: .venv
DEBUG Acquired exclusive lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Found PEP 621 metadata for /Users/iason/Dropbox/work/pyhep_dev/correctionlib in `pyproject.toml` (correctionlib)
DEBUG No static `pyproject.toml` available for: correctionlib @ file:///Users/iason/Dropbox/work/pyhep_dev/correctionlib (DynamicField("version"))
DEBUG Acquired exclusive lock for `/Users/iason/.cache/uv/sdists-v9/path/10fb999c383fdff2`
DEBUG Computed cache info: Timestamp(SystemTime { tv_sec: 1764369213, tv_nsec: 54704464 }), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1761436714, tv_nsec: 543224148 })))}. Most recently modified: pyproject.toml
DEBUG Preparing metadata for: correctionlib @ file:///Users/iason/Dropbox/work/pyhep_dev/correctionlib
DEBUG Assessing Python executable as base candidate: /Users/iason/.pyenv/versions/3.14t-dev-tsan-debug/bin/python3.14
WARN Failed to find base Python executable: failed to read symbolic link `/Users/iason/.pyenv/versions/3.14t-dev-tsan-debug/bin/python3.14`: Invalid argument (os error 22)
DEBUG No workspace root found, using project root
DEBUG Using base executable for virtual environment: /Users/iason/.pyenv/versions/3.14t-dev-tsan-debug/bin/python3.14
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.14.2
DEBUG Solving with target Python version: >=3.14.2
DEBUG Adding direct dependency: setuptools-scm[toml]>=3.4
DEBUG Adding direct dependency: pybind11>=2.6.1
DEBUG Adding direct dependency: scikit-build-core>=0.8
DEBUG Found fresh response for: https://pypi.org/simple/pybind11/
DEBUG Found fresh response for: https://pypi.org/simple/scikit-build-core/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cd/8a/37362fc2b949d5f733a8b0f2ff51ba423914cabefe69f1d1b6aab710f5fe/pybind11-3.0.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/43/49/ec16b3db6893db788ae35f98506ff5a9c25dca7eb18cc38ada8a4c1dc944/scikit_build_core-0.11.6-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/setuptools-scm/
DEBUG Searching for a compatible version of setuptools-scm[toml] (>=3.4)
DEBUG Selecting: setuptools-scm==9.2.2 [compatible] (setuptools_scm-9.2.2-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==9.2.2: setuptools-scm==9.2.2
DEBUG Adding transitive dependency for setuptools-scm==9.2.2: setuptools-scm[toml]==9.2.2
DEBUG Searching for a compatible version of setuptools-scm (==9.2.2)
DEBUG Selecting: setuptools-scm==9.2.2 [compatible] (setuptools_scm-9.2.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3d/ea/ac2bf868899d0d2e82ef72d350d97a846110c709bacf2d968431576ca915/setuptools_scm-9.2.2-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for setuptools-scm==9.2.2: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==9.2.2: setuptools*
DEBUG Searching for a compatible version of setuptools-scm[toml] (==9.2.2)
DEBUG Selecting: setuptools-scm==9.2.2 [compatible] (setuptools_scm-9.2.2-py3-none-any.whl)
DEBUG Searching for a compatible version of pybind11 (>=2.6.1)
DEBUG Selecting: pybind11==3.0.1 [compatible] (pybind11-3.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of scikit-build-core (>=0.8)
DEBUG Selecting: scikit-build-core==0.11.6 [compatible] (scikit_build_core-0.11.6-py3-none-any.whl)
DEBUG Adding transitive dependency for scikit-build-core==0.11.6: packaging>=23.2
DEBUG Adding transitive dependency for scikit-build-core==0.11.6: pathspec>=0.10.1
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/pathspec/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/32/2b/121e912bd60eebd623f873fd090de0e84f322972ab25a7f9044c056804ed/pathspec-1.0.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==80.9.0 [compatible] (setuptools-80.9.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a3/dc/17031897dae0efacfea57dfd3a82fdd2a2aeb58e0ff71b77b87e44edc772/setuptools-80.9.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==1.0.3 [compatible] (pathspec-1.0.3-py3-none-any.whl)
DEBUG Tried 6 versions: packaging 1, pathspec 1, pybind11 1, scikit-build-core 1, setuptools 1, setuptools-scm 1
DEBUG marker environment resolution took 0.003s
DEBUG Installing in setuptools-scm==9.2.2, pybind11==3.0.1, packaging==25.0, pathspec==1.0.3, scikit-build-core==0.11.6, setuptools==80.9.0 in /Users/iason/.cache/uv/builds-v0/.tmpGeknzi
DEBUG Registry requirement already cached: setuptools-scm==9.2.2
DEBUG Registry requirement already cached: pybind11==3.0.1
DEBUG Registry requirement already cached: packaging==25.0
DEBUG Registry requirement already cached: pathspec==1.0.3
DEBUG Registry requirement already cached: scikit-build-core==0.11.6
DEBUG Registry requirement already cached: setuptools==80.9.0
DEBUG Installing build requirements: setuptools-scm==9.2.2, pybind11==3.0.1, packaging==25.0, pathspec==1.0.3, scikit-build-core==0.11.6, setuptools==80.9.0
DEBUG Creating PEP 517 build environment
DEBUG Calling `scikit_build_core.build.get_requires_for_build_wheel()`
DEBUG fatal error: /Library/Developer/CommandLineTools/usr/bin/lipo: can't figure out the architecture type of: /Users/iason/.pyenv/shims/ninja
DEBUG No workspace root found, using project root
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.14.2
DEBUG Solving with target Python version: >=3.14.2
DEBUG Adding direct dependency: setuptools-scm[toml]>=3.4
DEBUG Adding direct dependency: pybind11>=2.6.1
DEBUG Adding direct dependency: scikit-build-core>=0.8
DEBUG Adding direct dependency: setuptools-scm*
DEBUG Searching for a compatible version of setuptools-scm[toml] (>=3.4)
DEBUG Selecting: setuptools-scm==9.2.2 [compatible] (setuptools_scm-9.2.2-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==9.2.2: setuptools-scm==9.2.2
DEBUG Adding transitive dependency for setuptools-scm==9.2.2: setuptools-scm[toml]==9.2.2
DEBUG Searching for a compatible version of setuptools-scm (==9.2.2)
DEBUG Selecting: setuptools-scm==9.2.2 [compatible] (setuptools_scm-9.2.2-py3-none-any.whl)
DEBUG Adding transitive dependency for setuptools-scm==9.2.2: packaging>=20
DEBUG Adding transitive dependency for setuptools-scm==9.2.2: setuptools*
DEBUG Searching for a compatible version of setuptools-scm[toml] (==9.2.2)
DEBUG Selecting: setuptools-scm==9.2.2 [compatible] (setuptools_scm-9.2.2-py3-none-any.whl)
DEBUG Searching for a compatible version of pybind11 (>=2.6.1)
DEBUG Selecting: pybind11==3.0.1 [compatible] (pybind11-3.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of scikit-build-core (>=0.8)
DEBUG Selecting: scikit-build-core==0.11.6 [compatible] (scikit_build_core-0.11.6-py3-none-any.whl)
DEBUG Adding transitive dependency for scikit-build-core==0.11.6: packaging>=23.2
DEBUG Adding transitive dependency for scikit-build-core==0.11.6: pathspec>=0.10.1
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==80.9.0 [compatible] (setuptools-80.9.0-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==1.0.3 [compatible] (pathspec-1.0.3-py3-none-any.whl)
DEBUG Tried 6 versions: packaging 1, pathspec 1, pybind11 1, scikit-build-core 1, setuptools 1, setuptools-scm 1
DEBUG marker environment resolution took 0.000s
DEBUG Installing in setuptools-scm==9.2.2, pybind11==3.0.1, packaging==25.0, pathspec==1.0.3, scikit-build-core==0.11.6, setuptools==80.9.0 in /Users/iason/.cache/uv/builds-v0/.tmpGeknzi
DEBUG Requirement already installed: setuptools-scm==9.2.2
DEBUG Requirement already installed: pybind11==3.0.1
DEBUG Requirement already installed: packaging==25.0
DEBUG Requirement already installed: pathspec==1.0.3
DEBUG Requirement already installed: scikit-build-core==0.11.6
DEBUG Requirement already installed: setuptools==80.9.0
DEBUG No build requirements to install for build
DEBUG Calling `scikit_build_core.build.prepare_metadata_for_build_wheel()`
DEBUG *** scikit-build-core 0.11.6 using CMake 4.2.1 (metadata_wheel)
DEBUG Prepared metadata for: correctionlib @ file:///Users/iason/Dropbox/work/pyhep_dev/correctionlib
DEBUG No workspace root found, using project root
DEBUG Released lock at `/Users/iason/.cache/uv/sdists-v9/path/10fb999c383fdff2/.lock`
DEBUG Solving with installed Python version: 3.14.2
DEBUG Solving with target Python version: >=3.14.2
DEBUG Adding direct dependency: correctionlib*
DEBUG Searching for a compatible version of correctionlib @ file:///Users/iason/Dropbox/work/pyhep_dev/correctionlib (*)
DEBUG Adding transitive dependency for correctionlib==2.7.1.dev14+gb6a34853d.d20260121: numpy>=1.13.3
DEBUG Adding transitive dependency for correctionlib==2.7.1.dev14+gb6a34853d.d20260121: pydantic>=2
DEBUG Adding transitive dependency for correctionlib==2.7.1.dev14+gb6a34853d.d20260121: rich*
DEBUG Adding transitive dependency for correctionlib==2.7.1.dev14+gb6a34853d.d20260121: packaging*
DEBUG Found fresh response for: https://pypi.org/simple/rich/
DEBUG Found fresh response for: https://pypi.org/simple/pydantic/
DEBUG Found fresh response for: https://pypi.org/simple/numpy/
DEBUG Searching for a compatible version of numpy (>=1.13.3)
DEBUG Selecting: numpy==2.4.1 [installed] (installed)
DEBUG Searching for a compatible version of pydantic (>=2)
DEBUG Selecting: pydantic==2.12.5 [installed] (installed)
DEBUG Adding transitive dependency for pydantic==2.12.5: annotated-types>=0.6.0
DEBUG Adding transitive dependency for pydantic==2.12.5: pydantic-core>=2.41.5, <2.41.5+
DEBUG Adding transitive dependency for pydantic==2.12.5: typing-extensions>=4.14.1
DEBUG Adding transitive dependency for pydantic==2.12.5: typing-inspection>=0.4.2
DEBUG Found fresh response for: https://pypi.org/simple/annotated-types/
DEBUG Found fresh response for: https://pypi.org/simple/typing-inspection/
DEBUG Found fresh response for: https://pypi.org/simple/typing-extensions/
DEBUG Found fresh response for: https://pypi.org/simple/pydantic-core/
DEBUG Searching for a compatible version of pydantic-core (>=2.41.5, <2.41.5+)
DEBUG Selecting: pydantic-core==2.41.5 [installed] (installed)
DEBUG Adding transitive dependency for pydantic-core==2.41.5: typing-extensions>=4.14.1
DEBUG Searching for a compatible version of rich (*)
DEBUG Selecting: rich==14.2.0 [installed] (installed)
DEBUG Adding transitive dependency for rich==14.2.0: markdown-it-py>=2.2.0
DEBUG Adding transitive dependency for rich==14.2.0: pygments>=2.13.0, <3.0.0
DEBUG Searching for a compatible version of packaging (*)
DEBUG Selecting: packaging==25.0 [installed] (installed)
DEBUG Searching for a compatible version of annotated-types (>=0.6.0)
DEBUG Selecting: annotated-types==0.7.0 [installed] (installed)
DEBUG Searching for a compatible version of typing-extensions (>=4.14.1)
DEBUG Selecting: typing-extensions==4.15.0 [installed] (installed)
DEBUG Searching for a compatible version of typing-inspection (>=0.4.2)
DEBUG Selecting: typing-inspection==0.4.2 [installed] (installed)
DEBUG Adding transitive dependency for typing-inspection==0.4.2: typing-extensions>=4.12.0
DEBUG Found fresh response for: https://pypi.org/simple/markdown-it-py/
DEBUG Searching for a compatible version of markdown-it-py (>=2.2.0)
DEBUG Selecting: markdown-it-py==4.0.0 [installed] (installed)
DEBUG Adding transitive dependency for markdown-it-py==4.0.0: mdurl>=0.1, <1.dev0
DEBUG Found fresh response for: https://pypi.org/simple/mdurl/
DEBUG Found fresh response for: https://pypi.org/simple/pygments/
DEBUG Searching for a compatible version of pygments (>=2.13.0, <3.0.0)
DEBUG Selecting: pygments==2.19.2 [installed] (installed)
DEBUG Searching for a compatible version of mdurl (>=0.1, <1.dev0)
DEBUG Selecting: mdurl==0.1.2 [installed] (installed)
DEBUG Tried 12 versions: annotated-types 1, correctionlib 1, markdown-it-py 1, mdurl 1, numpy 1, packaging 1, pydantic 1, pydantic-core 1, pygments 1, rich 1, typing-extensions 1, typing-inspection 1
DEBUG marker environment resolution took 0.008s
Resolved 12 packages in 35.57s
DEBUG Requirement already installed: pydantic-core==2.41.5
DEBUG Requirement already installed: annotated-types==0.7.0
DEBUG Requirement already installed: pydantic==2.12.5
DEBUG Requirement already installed: pygments==2.19.2
DEBUG Requirement already installed: numpy==2.4.1
DEBUG Requirement already installed: mdurl==0.1.2
DEBUG Requirement already installed: rich==14.2.0
DEBUG Requirement already installed: typing-inspection==0.4.2
DEBUG Requirement already installed: packaging==25.0
DEBUG Must revalidate requirement: correctionlib
DEBUG Requirement already installed: markdown-it-py==4.0.0
DEBUG Requirement already installed: typing-extensions==4.15.0
DEBUG Unnecessary package: awkward==2.8.11
DEBUG Unnecessary package: awkward-cpp==51
DEBUG Unnecessary package: cachetools==6.2.4
DEBUG Unnecessary package: certifi==2026.1.4
DEBUG Unnecessary package: charset-normalizer==3.4.4
DEBUG Unnecessary package: click==8.3.1
DEBUG Unnecessary package: cloudpickle==3.1.2
DEBUG Unnecessary package: cramjam==2.11.0
DEBUG Unnecessary package: dask==2025.3.0
DEBUG Unnecessary package: dask-awkward==2025.9.0
DEBUG Unnecessary package: fsspec==2026.1.0
DEBUG Unnecessary package: idna==3.11
DEBUG Unnecessary package: iniconfig==2.3.0
DEBUG Unnecessary package: locket==1.0.0
DEBUG Unnecessary package: pandas==2.3.3
DEBUG Unnecessary package: partd==1.4.2
DEBUG Unnecessary package: pip==25.3
DEBUG Unnecessary package: pluggy==1.6.0
DEBUG Unnecessary package: pytest==9.0.2
DEBUG Unnecessary package: pytest-run-parallel==0.8.2
DEBUG Unnecessary package: python-dateutil==2.9.0.post0
DEBUG Unnecessary package: pytz==2025.2
DEBUG Unnecessary package: pyyaml==6.0.3
DEBUG Unnecessary package: requests==2.32.5
DEBUG Unnecessary package: scipy==1.17.0
DEBUG Unnecessary package: six==1.17.0
DEBUG Unnecessary package: toolz==1.1.0
DEBUG Unnecessary package: tzdata==2025.3
DEBUG Unnecessary package: uproot==5.6.9
DEBUG Unnecessary package: urllib3==2.6.3
DEBUG Unnecessary package: xxhash==3.6.0
DEBUG Acquired exclusive lock for `/Users/iason/.cache/uv/sdists-v9/path/10fb999c383fdff2`
DEBUG Computed cache info: Timestamp(SystemTime { tv_sec: 1764369213, tv_nsec: 54704464 }), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { tv_sec: 1761436714, tv_nsec: 543224148 })))}. Most recently modified: pyproject.toml
   Building correctionlib @ file:///Users/iason/Dropbox/work/pyhep_dev/correctionlib
DEBUG Building: correctionlib @ file:///Users/iason/Dropbox/work/pyhep_dev/correctionlib
DEBUG Not using uv build backend direct build of `correctionlib @ file:///Users/iason/Dropbox/work/pyhep_dev/correctionlib`, pyproject.toml does not match: The value for `build_system.build-backend` should be `"uv_build"`, not `"scikit_build_core.build"`
DEBUG Assessing Python executable as base candidate: /Users/iason/.pyenv/versions/3.14t-dev-tsan-debug/bin/python3.14
WARN Failed to find base Python executable: failed to read symbolic link `/Users/iason/.pyenv/versions/3.14t-dev-tsan-debug/bin/python3.14`: Invalid argument (os error 22)
DEBUG Creating build environment for: correctionlib @ file:///Users/iason/Dropbox/work/pyhep_dev/correctionlib
DEBUG Calling `scikit_build_core.build.build_wheel("/Users/iason/.cache/uv/builds-v0/.tmpsPhugN", {}, "/Users/iason/.cache/uv/builds-v0/.tmpGeknzi/metadata_directory/correctionlib-2.7.1.dev14+gb6a34853d.d20260121.dist-info")`
DEBUG *** scikit-build-core 0.11.6 using CMake 4.2.1 (wheel)
DEBUG *** Configuring CMake...
DEBUG fatal error: /Library/Developer/CommandLineTools/usr/bin/lipo: can't figure out the architecture type of: /Users/iason/.pyenv/shims/ninja
DEBUG loading initial cache file /var/folders/2c/94kt72fs0_q_9nzr2y5jphb00000gn/T/tmp4o9952hw/build/CMakeInit.txt
DEBUG -- The CXX compiler identification is AppleClang 17.0.0.17000603
DEBUG -- Detecting CXX compiler ABI info
DEBUG -- Detecting CXX compiler ABI info - done
DEBUG -- Check for working CXX compiler: /usr/bin/clang++ - skipped
DEBUG -- Detecting CXX compile features
DEBUG -- Detecting CXX compile features - done
DEBUG -- pybind11 v3.0.0
DEBUG -- Found Python: /Users/iason/.cache/uv/builds-v0/.tmpGeknzi/bin/python (found suitable version "3.14.2", minimum required is "3.8") found components: Interpreter Development.Module Development.Embed
DEBUG Using compatibility mode for Python, set PYBIND11_FINDPYTHON to NEW/OLD to silence this message
DEBUG -- Performing Test HAS_FLTO_THIN
DEBUG -- Performing Test HAS_FLTO_THIN - Success
DEBUG -- Performing Test CMAKE_HAVE_LIBC_PTHREAD
DEBUG -- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
DEBUG -- Found Threads: TRUE
DEBUG -- Found ZLIB: /Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/lib/libz.tbd (found version "1.2.12")
DEBUG -- Configuring done (15.0s)
DEBUG -- Generating done (0.1s)
DEBUG -- Build files have been written to: /var/folders/2c/94kt72fs0_q_9nzr2y5jphb00000gn/T/tmp4o9952hw/build
DEBUG *** Building project with Ninja...
DEBUG [1/5] Building CXX object CMakeFiles/correctionlib.dir/src/correction.cc.o
DEBUG [2/5] Building CXX object CMakeFiles/_core.dir/src/python.cc.o
DEBUG [3/5] Building CXX object CMakeFiles/correctionlib.dir/src/formula_ast.cc.o
DEBUG [4/5] Linking CXX shared library libcorrectionlib.dylib
DEBUG [5/5] Linking CXX shared module _core.cpython-314td-darwin.so
DEBUG *** Installing project into wheel...
DEBUG -- Install configuration: "Release"
DEBUG -- Installing: /var/folders/2c/94kt72fs0_q_9nzr2y5jphb00000gn/T/tmp4o9952hw/wheel/platlib/correctionlib/include/correctionlib_version.h
DEBUG -- Installing: /var/folders/2c/94kt72fs0_q_9nzr2y5jphb00000gn/T/tmp4o9952hw/wheel/platlib/correctionlib/lib/libcorrectionlib.dylib
DEBUG -- Installing: /var/folders/2c/94kt72fs0_q_9nzr2y5jphb00000gn/T/tmp4o9952hw/wheel/platlib/correctionlib/include/correction.h
DEBUG -- Installing: /var/folders/2c/94kt72fs0_q_9nzr2y5jphb00000gn/T/tmp4o9952hw/wheel/platlib/correctionlib/_core.cpython-314td-darwin.so
DEBUG -- Installing: /var/folders/2c/94kt72fs0_q_9nzr2y5jphb00000gn/T/tmp4o9952hw/wheel/platlib/correctionlib/cmake/correctionlib-targets.cmake
DEBUG -- Installing: /var/folders/2c/94kt72fs0_q_9nzr2y5jphb00000gn/T/tmp4o9952hw/wheel/platlib/correctionlib/cmake/correctionlib-targets-release.cmake
DEBUG -- Installing: /var/folders/2c/94kt72fs0_q_9nzr2y5jphb00000gn/T/tmp4o9952hw/wheel/platlib/correctionlib/cmake/correctionlibConfig.cmake
DEBUG -- Installing: /var/folders/2c/94kt72fs0_q_9nzr2y5jphb00000gn/T/tmp4o9952hw/wheel/platlib/correctionlib/cmake/correctionlibConfigVersion.cmake
DEBUG *** Making wheel...
DEBUG *** Created correctionlib-2.7.1.dev14+gb6a34853d.d20260121-cp314-cp314td-macosx_26_0_arm64.whl
DEBUG Built `correctionlib @ file:///Users/iason/Dropbox/work/pyhep_dev/correctionlib` into `correctionlib-2.7.1.dev14+gb6a34853d.d20260121-cp314-cp314td-macosx_26_0_arm64.whl`
      Built correctionlib @ file:///Users/iason/Dropbox/work/pyhep_dev/correctionlib
DEBUG Released lock at `/Users/iason/.cache/uv/sdists-v9/path/10fb999c383fdff2/.lock`
  × Failed to build `correctionlib @ file:///Users/iason/Dropbox/work/pyhep_dev/correctionlib`
  ╰─▶ The built wheel `correctionlib-2.7.1.dev14+gb6a34853d.d20260121-cp314-cp314-macosx_26_0_arm64.whl` is not compatible with the current Python 3.14 on macos aarch64
DEBUG Released lock at `/Users/iason/Dropbox/work/pyhep_dev/correctionlib/.venv/.lock`
DEBUG Released lock at `/Users/iason/.cache/uv/.lock`
```

### Platform

macOS 26 arm64

### Version

uv 0.9.26 (ee4f00362 2026-01-15)

### Python version

Python 3.14

---

_Label `bug` added by @ikrommyd on 2026-01-21 00:53_

---

_Renamed from "Trying to build package for venv with debug python build fails with "the wheel build .. is not coppatible ..."" to "Trying to build package for venv with debug python build fails with "the built wheel .. is not compatbile with the current Python ..."" by @ikrommyd on 2026-01-21 00:53_

---

_Renamed from "Trying to build package for venv with debug python build fails with "the built wheel .. is not compatbile with the current Python ..."" to "Trying to build package inside venv with debug python build fails with "the built wheel .. is not compatbile with the current Python ..."" by @ikrommyd on 2026-01-21 00:54_

---
