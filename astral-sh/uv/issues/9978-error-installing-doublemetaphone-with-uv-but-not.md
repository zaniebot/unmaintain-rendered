```yaml
number: 9978
title: "Error installing `DoubleMetaphone` with uv but not with poetry or pip"
type: issue
state: closed
author: lewis-wf
labels: []
assignees: []
created_at: 2024-12-17T18:31:30Z
updated_at: 2024-12-17T18:38:31Z
url: https://github.com/astral-sh/uv/issues/9978
synced_at: 2026-01-12T16:00:03Z
```

# Error installing `DoubleMetaphone` with uv but not with poetry or pip

---

_@lewis-wf_

**Context**
* MacOS 15.2
* `uv` 0.5.9
* Xcode is installed
* Trying to install https://pypi.org/project/DoubleMetaphone/

**Failure**
While DoubleMetaphone will build and install successfully with `poetry` and `pip`, it will not build with `uv`. The error I get is:
```
  × Failed to download and build `doublemetaphone==1.1`
  ╰─▶ Build backend failed to build wheel through `build_wheel` (exit status: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_py
      creating build/lib.macosx-11.0-arm64-cpython-313/doublemetaphone
      copying doublemetaphone/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/doublemetaphone
      running build_ext
      building 'doublemetaphone.doublemetaphone' extension
      creating build/temp.macosx-11.0-arm64-cpython-313/doublemetaphone
      clang++ -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -Idoublemetaphone -I/Users/lewis/.cache/uv/builds-v0/.tmp6NpIYn/include
      -I/Users/lewis/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 -c doublemetaphone/double_metaphone.cc -o build/temp.macosx-11.0-arm64-cpython-313/doublemetaphone/double_metaphone.o
      clang++ -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -Idoublemetaphone -I/Users/lewis/.cache/uv/builds-v0/.tmp6NpIYn/include
      -I/Users/lewis/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 -c doublemetaphone/doublemetaphone.cpp -o build/temp.macosx-11.0-arm64-cpython-313/doublemetaphone/doublemetaphone.o
      clang++ -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -bundle -undefined dynamic_lookup -arch arm64 -mmacosx-version-min=11.0
      -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -LModules/_hacl build/temp.macosx-11.0-arm64-cpython-313/doublemetaphone/double_metaphone.o build/temp.macosx-11.0-arm64-cpython-313/doublemetaphone/doublemetaphone.o -L/install/lib -o
      build/lib.macosx-11.0-arm64-cpython-313/doublemetaphone/doublemetaphone.cpython-313-darwin.so

      [stderr]
      doublemetaphone/double_metaphone.cc:170:10: warning: illegal character encoding in character literal [-Winvalid-source-encoding]
        170 |     case '<C7>':
            |          ^
      doublemetaphone/double_metaphone.cc:634:10: warning: illegal character encoding in character literal [-Winvalid-source-encoding]
        634 |     case '<D1>':
            |          ^
      2 warnings generated.
      Compiling with an SDK that doesn't seem to exist: /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk
      Please check your Xcode installation
      clang++: warning: no such sysroot directory: '/Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk' [-Wmissing-sysroot]
      ld: warning: search path 'Modules/_hacl' not found
      ld: warning: search path '/install/lib' not found
      ld: library 'c++' not found
      clang++: error: linker command failed with exit code 1 (use -v to see invocation)
      error: command '/usr/bin/clang++' failed with exit code 1

  help: `doublemetaphone` (v1.1) was included because `api` (v0.1.0) depends on `probablepeople` (v0.5.6) which depends on `doublemetaphone`
```

---

_Comment by @lewis-wf on 2024-12-17 18:33_

Following the debug steps in the docs, here is pip doing it successfully:
```
pip install --force-reinstall --no-cache --use-pep517 doublemetaphone==1.1
Collecting doublemetaphone==1.1
  Downloading DoubleMetaphone-1.1.tar.gz (34 kB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: doublemetaphone
  Building wheel for doublemetaphone (pyproject.toml) ... done
  Created wheel for doublemetaphone: filename=DoubleMetaphone-1.1-cp312-cp312-macosx_14_0_arm64.whl size=30435 sha256=7537a7ff48ce1dd5a8ae0f808c2a6bc9a701cec42c0eb9ecd6faa00702dbf9c6
  Stored in directory: /private/var/folders/05/7nf7jgtn6hsdq7h4j73p_7fw0000gn/T/pip-ephem-wheel-cache-56g6rb73/wheels/79/43/f1/cf254c5a1d392c65fd5878831853c02037a1732a236fd18cd3
Successfully built doublemetaphone
Installing collected packages: doublemetaphone
  Attempting uninstall: doublemetaphone
    Found existing installation: DoubleMetaphone 1.1
    Uninstalling DoubleMetaphone-1.1:
      Successfully uninstalled DoubleMetaphone-1.1
Successfully installed doublemetaphone-1.1
```

---

_Comment by @lewis-wf on 2024-12-17 18:35_

Here is the full verbose debug output:
```
DEBUG Acquired lock for `/Users/lewis/.cache/uv/sdists-v6/pypi/doublemetaphone/1.1`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8a/fb/054ad89223951abedde4351c5ff8ba009dcd576d8ecafed357762c7f2d9d/DoubleMetaphone-1.1.tar.gz
DEBUG Building: doublemetaphone==1.1
DEBUG Assessing Python executable as base candidate: /Users/lewis/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python3.13
DEBUG Using base executable for virtual environment: /Users/lewis/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/bin/python3.13
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.1
DEBUG Solving with target Python version: >=3.13.1
DEBUG Adding direct dependency: setuptools*
DEBUG Adding direct dependency: wheel*
DEBUG Adding direct dependency: cython*
DEBUG Found stale response for: https://pypi.org/simple/wheel/
DEBUG Sending revalidation request for: https://pypi.org/simple/wheel/
DEBUG Found stale response for: https://pypi.org/simple/setuptools/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools/
DEBUG Found stale response for: https://pypi.org/simple/cython/
DEBUG Sending revalidation request for: https://pypi.org/simple/cython/
DEBUG Found not-modified response for: https://pypi.org/simple/wheel/
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==75.6.0 [compatible] (setuptools-75.6.0-py3-none-any.whl)
DEBUG Found not-modified response for: https://pypi.org/simple/cython/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/55/21/47d163f615df1d30c094f6c8bbb353619274edccf0327b185cc2493c2c33/setuptools-75.6.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of wheel (*)
DEBUG Selecting: wheel==0.45.1 [compatible] (wheel-0.45.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0b/2c/87f3254fd8ffd29e4c02732eee68a83a1d3c346ae39bc6822dcbcb697f2b/wheel-0.45.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/43/39/bdbec9142bc46605b54d674bf158a78b191c2b75be527c6dcf3e6dfe90b8/Cython-3.0.11-py2.py3-none-any.whl.metadata
DEBUG Tried 3 versions: cython 1, setuptools 1, wheel 1
DEBUG marker environment resolution took 0.049s
DEBUG Installing in wheel==0.45.1, setuptools==75.6.0, cython==3.0.11 in /Users/lewis/.cache/uv/builds-v0/.tmp5ouczZ
DEBUG Requirement already cached: wheel==0.45.1
DEBUG Requirement already cached: setuptools==75.6.0
DEBUG Requirement already cached: cython==3.0.11
DEBUG Installing build requirements: wheel==0.45.1, setuptools==75.6.0, cython==3.0.11
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG running egg_info
DEBUG writing DoubleMetaphone.egg-info/PKG-INFO
DEBUG writing dependency_links to DoubleMetaphone.egg-info/dependency_links.txt
DEBUG writing top-level names to DoubleMetaphone.egg-info/top_level.txt
DEBUG reading manifest file 'DoubleMetaphone.egg-info/SOURCES.txt'
DEBUG reading manifest template 'MANIFEST.in'
DEBUG adding license file 'LICENSE'
DEBUG writing manifest file 'DoubleMetaphone.egg-info/SOURCES.txt'
DEBUG Calling `setuptools.build_meta:__legacy__.build_wheel("/Users/lewis/.cache/uv/builds-v0/.tmphoRnGI", {}, None)`
DEBUG running bdist_wheel
DEBUG running build
DEBUG running build_py
DEBUG copying doublemetaphone/__init__.py -> build/lib.macosx-11.0-arm64-cpython-313/doublemetaphone
DEBUG running build_ext
DEBUG building 'doublemetaphone.doublemetaphone' extension
DEBUG clang++ -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -Idoublemetaphone -I/Users/lewis/.cache/uv/builds-v0/.tmp5ouczZ/include -I/Users/lewis/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 -c doublemetaphone/double_metaphone.cc -o build/temp.macosx-11.0-arm64-cpython-313/doublemetaphone/double_metaphone.o
DEBUG doublemetaphone/double_metaphone.cc:170:10: warning: illegal character encoding in character literal [-Winvalid-source-encoding]
DEBUG   170 |     case '<C7>':
DEBUG       |          ^
DEBUG doublemetaphone/double_metaphone.cc:634:10: warning: illegal character encoding in character literal [-Winvalid-source-encoding]
DEBUG   634 |     case '<D1>':
DEBUG       |          ^
DEBUG 2 warnings generated.
DEBUG clang++ -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -Idoublemetaphone -I/Users/lewis/.cache/uv/builds-v0/.tmp5ouczZ/include -I/Users/lewis/.local/share/uv/python/cpython-3.13.1-macos-aarch64-none/include/python3.13 -c doublemetaphone/doublemetaphone.cpp -o build/temp.macosx-11.0-arm64-cpython-313/doublemetaphone/doublemetaphone.o
DEBUG Compiling with an SDK that doesn't seem to exist: /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk
DEBUG Please check your Xcode installation
DEBUG clang++ -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new -bundle -undefined dynamic_lookup -arch arm64 -mmacosx-version-min=11.0 -isysroot /Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk -LModules/_hacl build/temp.macosx-11.0-arm64-cpython-313/doublemetaphone/double_metaphone.o build/temp.macosx-11.0-arm64-cpython-313/doublemetaphone/doublemetaphone.o -L/install/lib -o build/lib.macosx-11.0-arm64-cpython-313/doublemetaphone/doublemetaphone.cpython-313-darwin.so
DEBUG clang++: warning: no such sysroot directory: '/Applications/Xcode_15.2.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX14.2.sdk' [-Wmissing-sysroot]
DEBUG ld: warning: search path 'Modules/_hacl' not found
DEBUG ld: warning: search path '/install/lib' not found
DEBUG ld: library 'c++' not found
DEBUG clang++: error: linker command failed with exit code 1 (use -v to see invocation)
DEBUG error: command '/usr/bin/clang++' failed with exit code 1
DEBUG Released lock at `/Users/lewis/.cache/uv/sdists-v6/pypi/doublemetaphone/1.1/.lock`
```

---

_Comment by @charliermarsh on 2024-12-17 18:36_

We fixed this in the last release, but you need to reinstall Python. Like `uv python install 3.13 --reinstall`.

---

_Comment by @lewis-wf on 2024-12-17 18:38_

Fantastic, that fixed it. Thank you for such a brilliant project 🔥 

---

_Closed by @lewis-wf on 2024-12-17 18:38_

---
