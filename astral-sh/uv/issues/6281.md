```yaml
number: 6281
title: "Unable to install/build the `llvmlite` package"
type: issue
state: closed
author: taranlu-houzz
labels:
  - question
assignees: []
created_at: 2024-08-20T23:11:40Z
updated_at: 2024-12-22T15:32:09Z
url: https://github.com/astral-sh/uv/issues/6281
synced_at: 2026-01-10T04:36:20Z
```

# Unable to install/build the `llvmlite` package

---

_Issue opened by @taranlu-houzz on 2024-08-20 23:11_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I ran into this while trying to install an internal package from a private repo that has a dep (or rather, dep of a dep, etc.) on `llvmlite`. I am able to successfully install the package when using `pipx`, but `uv` fails to install it.

```
❌ 2 ❯ uv tool install --python 3.11 --python-preference only-managed --extra-index-url 'my-url' --prerelease allow --refresh my-package
Resolved 26 packages in 808ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: llvmlite==0.35.0
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
/Users/taran/.cache/uv/builds-v0/.tmpsZC6qP/bin/python /Users/taran/.cache/uv/built-wheels-v3/index/6dbd262e64c3fb74/llvmlite/0.35.0/aQd_pIr8Ymw1uotL255dv/llvmlite-0.35
.0.tar.gz/ffi/build.py
LLVM version...
--- stderr:
Traceback (most recent call last):
  File "/Users/taran/.cache/uv/built-wheels-v3/index/6dbd262e64c3fb74/llvmlite/0.35.0/aQd_pIr8Ymw1uotL255dv/llvmlite-0.35.0.tar.gz/ffi/build.py", line 105, in main_posi
x
    out = subprocess.check_output([llvm_config, '--version'])
          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/taran/.local/share/uv/python/cpython-3.11.9-macos-aarch64-none/lib/python3.11/subprocess.py", line 466, in check_output
    return run(*popenargs, stdout=PIPE, timeout=timeout, check=True,
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/taran/.local/share/uv/python/cpython-3.11.9-macos-aarch64-none/lib/python3.11/subprocess.py", line 548, in run
    with Popen(*popenargs, **kwargs) as process:
         ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/taran/.local/share/uv/python/cpython-3.11.9-macos-aarch64-none/lib/python3.11/subprocess.py", line 1026, in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
  File "/Users/taran/.local/share/uv/python/cpython-3.11.9-macos-aarch64-none/lib/python3.11/subprocess.py", line 1955, in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
FileNotFoundError: [Errno 2] No such file or directory: 'llvm-config'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/Users/taran/.cache/uv/built-wheels-v3/index/6dbd262e64c3fb74/llvmlite/0.35.0/aQd_pIr8Ymw1uotL255dv/llvmlite-0.35.0.tar.gz/ffi/build.py", line 191, in <module>
    main()
  File "/Users/taran/.cache/uv/built-wheels-v3/index/6dbd262e64c3fb74/llvmlite/0.35.0/aQd_pIr8Ymw1uotL255dv/llvmlite-0.35.0.tar.gz/ffi/build.py", line 185, in main
    main_posix('osx', '.dylib')
  File "/Users/taran/.cache/uv/built-wheels-v3/index/6dbd262e64c3fb74/llvmlite/0.35.0/aQd_pIr8Ymw1uotL255dv/llvmlite-0.35.0.tar.gz/ffi/build.py", line 107, in main_posix
    raise RuntimeError("%s failed executing, please point LLVM_CONFIG "
RuntimeError: llvm-config failed executing, please point LLVM_CONFIG to the path for llvm-config
error: command '/Users/taran/.cache/uv/builds-v0/.tmpsZC6qP/bin/python' failed with exit code 1
---

```

It seems like the issue occurs when trying to install the `llvmlite` package, which it tries to build (the error below is a little different than the initial error).
```
❌ 2 ❯ uv tool install --python 3.11 --python-preference only-managed --no-binary --refresh --prerelease allow --verbose llvmlite
DEBUG uv 0.3.0
DEBUG Searching for Python 3.11 in managed installations
DEBUG Searching for managed installations at `/Users/taran/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.11.9-macos-aarch64-none`
DEBUG Found `cpython-3.11.9-macos-aarch64-none` at `/Users/taran/.local/share/uv/python/cpython-3.11.9-macos-aarch64-none/bin/python3` (managed installations)
DEBUG Using request timeout of 30s
DEBUG Using request timeout of 30s
DEBUG Acquired lock for `/Users/taran/.local/share/uv/tools`
DEBUG Using existing environment for tool `llvmlite`: /Users/taran/.local/share/uv/tools/llvmlite
DEBUG Found existing environment for `llvmlite`
DEBUG At least one requirement is not satisfied: llvmlite
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.9
DEBUG Adding direct dependency: llvmlite*
DEBUG Found stale response for: https://pypi.org/simple/llvmlite/
DEBUG Sending revalidation request for: https://pypi.org/simple/llvmlite/
DEBUG Found not-modified response for: https://pypi.org/simple/llvmlite/
DEBUG Searching for a compatible version of llvmlite (*)
DEBUG Selecting: llvmlite==0.43.0 [compatible] (llvmlite-0.43.0.tar.gz)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/23/ff/6ca7e98998b573b4bd6566f15c35e5c8bea829663a6df0c7aa55ab559da9/llvmlite-0.43.0-cp310-cp310-m
acosx_10_9_x86_64.whl.metadata
DEBUG Tried 1 versions: llvmlite 1
DEBUG Split specific environment resolution took 0.032s
Resolved 1 package in 33ms
DEBUG Must revalidate requirement: llvmlite
DEBUG Acquired lock for `/Users/taran/.cache/uv/built-wheels-v3/pypi/llvmlite/0.43.0`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9f/3d/f513755f285db51ab363a53e898b85562e950f79a2e6767a364530c2f645/llvmlite-0.43.0.tar.gz
DEBUG Building: llvmlite==0.43.0
INFO Ignoring empty directory
DEBUG Solving with installed Python version: 3.11.9
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Found stale response for: https://pypi.org/simple/setuptools/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools/
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==73.0.1 [compatible] (setuptools-73.0.1.tar.gz)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/07/6a/0270e295bf30c37567736b7fca10167640898214ff911273af37ddb95770/setuptools-73.0.1-py3-none-an
y.whl.metadata
DEBUG Tried 1 versions: setuptools 1
DEBUG Split specific environment resolution took 0.008s
DEBUG Installing in setuptools==73.0.1 in /Users/taran/.cache/uv/builds-v0/.tmp5621wb
DEBUG Must revalidate requirement: setuptools
DEBUG Downloading and building requirement for build: setuptools==73.0.1
DEBUG Acquired lock for `/Users/taran/.cache/uv/built-wheels-v3/pypi/setuptools/73.0.1`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8d/37/f4d4ce9bc15e61edba3179f9b0f763fc6d439474d28511b11f0d95bab7a2/setuptools-73.0.1.tar.gz
DEBUG Installing build requirement: setuptools==73.0.1
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG Calling `setuptools.build_meta:__legacy__.build_wheel("/Users/taran/.cache/uv/built-wheels-v3/pypi/llvmlite/0.43.0/H_0AbqOoYKQrOJ2gTkAhX/.tmpmg60Nv", {}, None)`
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: llvmlite==0.43.0
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
/Users/taran/.cache/uv/builds-v0/.tmp5621wb/bin/python /Users/taran/.cache/uv/built-wheels-v3/pypi/llvmlite/0.43.0/H_0AbqOoYKQrOJ2gTkAhX/llvmlite-0.43.0.tar.gz/ffi/build.py
LLVM version...
--- stderr:
Traceback (most recent call last):
  File "/Users/taran/.cache/uv/built-wheels-v3/pypi/llvmlite/0.43.0/H_0AbqOoYKQrOJ2gTkAhX/llvmlite-0.43.0.tar.gz/ffi/build.py", line 235, in <module>
    main()
  File "/Users/taran/.cache/uv/built-wheels-v3/pypi/llvmlite/0.43.0/H_0AbqOoYKQrOJ2gTkAhX/llvmlite-0.43.0.tar.gz/ffi/build.py", line 229, in main
    main_posix('osx', '.dylib')
  File "/Users/taran/.cache/uv/built-wheels-v3/pypi/llvmlite/0.43.0/H_0AbqOoYKQrOJ2gTkAhX/llvmlite-0.43.0.tar.gz/ffi/build.py", line 142, in main_posix
    raise RuntimeError(msg) from None
RuntimeError: Could not find a `llvm-config` binary. There are a number of reasons this could occur, please see: https://llvmlite.readthedocs.io/en/latest/admin-guide/install.html#using-pip for help.
error: command '/Users/taran/.cache/uv/builds-v0/.tmp5621wb/bin/python' failed with exit code 1
---
```

Platform: `macOS 14.6 (23G80)`
uv version: `uv 0.3.0 (dd1934c9c 2024-08-20)`

---

_Comment by @charliermarsh on 2024-08-20 23:17_

Thanks, will take a look.

---

_Comment by @charliermarsh on 2024-08-20 23:22_

What's the result of running `pip install llvmlite==0.35.0 --use-pep517 --no-cache --force-reinstall`?

---

_Comment by @taranlu-houzz on 2024-08-20 23:26_

```
↓2 ~/Documents/uv_llvmlite_bug via 🐍 v3.11.7 (.venv)
❯ pip install llvmlite==0.35.0 --use-pep517 --no-cache --force-reinstall
Collecting llvmlite==0.35.0
  Downloading llvmlite-0.35.0.tar.gz (121 kB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: llvmlite
  Building wheel for llvmlite (pyproject.toml) ... error
  error: subprocess-exited-with-error

  × Building wheel for llvmlite (pyproject.toml) did not run successfully.
  │ exit code: 1
  ╰─> [29 lines of output]
      running bdist_wheel
      /Users/taran/Documents/uv_llvmlite_bug/.venv/bin/python3.11 /private/var/folders/_s/t531j8b96sg3gzbpsc40s_5c0000gn/T/pip-install-zfgo_lkj/llvmlite_8f49a3f59eef4b7
195a32f3df02acf1f/ffi/build.py
      LLVM version... Traceback (most recent call last):
        File "/private/var/folders/_s/t531j8b96sg3gzbpsc40s_5c0000gn/T/pip-install-zfgo_lkj/llvmlite_8f49a3f59eef4b7195a32f3df02acf1f/ffi/build.py", line 105, in main_p
osix
          out = subprocess.check_output([llvm_config, '--version'])
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/taran/.local/share/mise/installs/python/3.11.7/lib/python3.11/subprocess.py", line 466, in check_output
          return run(*popenargs, stdout=PIPE, timeout=timeout, check=True,
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/taran/.local/share/mise/installs/python/3.11.7/lib/python3.11/subprocess.py", line 548, in run
          with Popen(*popenargs, **kwargs) as process:
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/taran/.local/share/mise/installs/python/3.11.7/lib/python3.11/subprocess.py", line 1026, in __init__
          self._execute_child(args, executable, preexec_fn, close_fds,
        File "/Users/taran/.local/share/mise/installs/python/3.11.7/lib/python3.11/subprocess.py", line 1950, in _execute_child
          raise child_exception_type(errno_num, err_msg, err_filename)
      FileNotFoundError: [Errno 2] No such file or directory: 'llvm-config'

      During handling of the above exception, another exception occurred:

      Traceback (most recent call last):
        File "/private/var/folders/_s/t531j8b96sg3gzbpsc40s_5c0000gn/T/pip-install-zfgo_lkj/llvmlite_8f49a3f59eef4b7195a32f3df02acf1f/ffi/build.py", line 191, in <modul
e>
          main()
        File "/private/var/folders/_s/t531j8b96sg3gzbpsc40s_5c0000gn/T/pip-install-zfgo_lkj/llvmlite_8f49a3f59eef4b7195a32f3df02acf1f/ffi/build.py", line 185, in main
          main_posix('osx', '.dylib')
        File "/private/var/folders/_s/t531j8b96sg3gzbpsc40s_5c0000gn/T/pip-install-zfgo_lkj/llvmlite_8f49a3f59eef4b7195a32f3df02acf1f/ffi/build.py", line 107, in main_p
osix
          raise RuntimeError("%s failed executing, please point LLVM_CONFIG "
      RuntimeError: llvm-config failed executing, please point LLVM_CONFIG to the path for llvm-config
      error: command '/Users/taran/Documents/uv_llvmlite_bug/.venv/bin/python3.11' failed with exit code 1
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for llvmlite
Failed to build llvmlite
ERROR: ERROR: Failed to build installable wheels for some pyproject.toml based projects (llvmlite)
```

---

_Comment by @charliermarsh on 2024-08-20 23:31_

It's very likely a problem with the package itself then. I'm having trouble reproducing this because that llvmlite version requires a very old LLVM, and newer versions work fine:

```
LLVM_CONFIG=/opt/homebrew/opt/llvm@14/bin/llvm-config uv pip install https://files.pythonhosted.org/packages/9f/3d/f513755f285db51ab363a53e898b85562e950f79a2e6767a364530c2f645/llvmlite-0.43.0.tar.gz --no-cache --force-reinstall
```

Can you try setting `LLVM_CONFIG` explicitly?

---

_Comment by @taranlu-houzz on 2024-08-21 00:05_

Hmm, maybe I am doing something wrong? This is the full output of the actual command:

`uv tool install --python 3.11 --python-preference only-managed --extra-index-url '<internal repo>' --prerelease allow --refresh --verbose my-package`

<details>
  <summary>Output</summary>

```
DEBUG uv 0.3.0
DEBUG Searching for Python 3.11 in managed installations
DEBUG Searching for managed installations at `/Users/taran/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.11.9-macos-aarch64-none`
DEBUG Found `cpython-3.11.9-macos-aarch64-none` at `/Users/taran/.local/share/uv/python/cpython-3.11.9-macos-aarch64-none/bin/python3` (managed installations)
DEBUG Using request timeout of 30s
DEBUG Using request timeout of 30s
DEBUG Acquired lock for `/Users/taran/.local/share/uv/tools`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.9
DEBUG Adding direct dependency: my-package*
DEBUG Found stale response for: <internal repo>/my-package/
DEBUG Sending revalidation request for: <internal repo>/my-package/
DEBUG Found modified response for: <internal repo>/my-package/
DEBUG Searching for a compatible version of my-package (*)
DEBUG Selecting: my-package==1.0.0a56 [compatible] (my-package-1.0.0a56-py3-none-any.whl)
DEBUG Found stale response for: <internal repo>/packages/my-package-1.0.0a56-py3-none-any.whl#sha256=fd77ed2898c6153fc8b89851e6790f29c76fd77a429a93e117180f71a0afbea0
DEBUG Sending revalidation request for: <internal repo>/packages/my-package-1.0.0a56-py3-none-any.whl#sha256=fd77ed2898c6153fc8b89851e6790f29c76fd77a429a93e117180f71a0afbea0
DEBUG Found modified response for: <internal repo>/packages/my-package-1.0.0a56-py3-none-any.whl#sha256=fd77ed2898c6153fc8b89851e6790f29c76fd77a429a93e117180f71a0afbea0
WARN Server returned unusable 304 for: <internal repo>/packages/my-package-1.0.0a56-py3-none-any.whl#sha256=fd77ed2898c6153fc8b89851e6790f29c76fd77a429a93e117180f71a0afbea0
DEBUG Adding transitive dependency for my-package==1.0.0a56: pillow>=10.4.0
DEBUG Adding transitive dependency for my-package==1.0.0a56: bpy==4.2.0
DEBUG Adding transitive dependency for my-package==1.0.0a56: largestinteriorrectangle>=0.2.1
DEBUG Adding transitive dependency for my-package==1.0.0a56: numpy>=2.0.0
DEBUG Adding transitive dependency for my-package==1.0.0a56: opencv-python>=4.10.0.84
DEBUG Adding transitive dependency for my-package==1.0.0a56: rich>=13.7.1
DEBUG Adding transitive dependency for my-package==1.0.0a56: typer>=0.12.3
DEBUG Adding transitive dependency for my-package==1.0.0a56: typer[all]>=0.12.3
DEBUG Adding transitive dependency for my-package==1.0.0a56: wand>=0.6.13
DEBUG Adding transitive dependency for my-package==1.0.0a56: wrapt>=1.16.0
DEBUG Found stale response for: <internal repo>/bpy/
DEBUG Sending revalidation request for: <internal repo>/bpy/
DEBUG Found stale response for: <internal repo>/largestinteriorrectangle/
DEBUG Sending revalidation request for: <internal repo>/largestinteriorrectangle/
DEBUG Found stale response for: <internal repo>/wand/
DEBUG Sending revalidation request for: <internal repo>/wand/
DEBUG Found stale response for: <internal repo>/typer/
DEBUG Sending revalidation request for: <internal repo>/typer/
DEBUG Found stale response for: <internal repo>/rich/
DEBUG Sending revalidation request for: <internal repo>/rich/
DEBUG Found stale response for: <internal repo>/pillow/
DEBUG Sending revalidation request for: <internal repo>/pillow/
DEBUG Found stale response for: <internal repo>/opencv-python/
DEBUG Sending revalidation request for: <internal repo>/opencv-python/
DEBUG Found stale response for: <internal repo>/wrapt/
DEBUG Sending revalidation request for: <internal repo>/wrapt/
DEBUG Found stale response for: <internal repo>/numpy/
DEBUG Sending revalidation request for: <internal repo>/numpy/
DEBUG Found not-modified response for: <internal repo>/largestinteriorrectangle/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9a/06/d5cf3f16076e10debef57d4a29583a443117e07696a77fd33acb6ee24ede/largestinteriorrectangle-0.2.1-py3-none-any.whl.metadata
DEBUG Found not-modified response for: <internal repo>/bpy/
DEBUG Searching for a compatible version of bpy (==4.2.0)
DEBUG Selecting: bpy==4.2.0 [compatible] (bpy-4.2.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ea/5a/d085d433ad26687d7ba5be235c57679780a3b41316cd7ece1ee65694ebad/bpy-4.2.0-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Adding transitive dependency for bpy==4.2.0: cython*
DEBUG Adding transitive dependency for bpy==4.2.0: numpy*
DEBUG Adding transitive dependency for bpy==4.2.0: requests*
DEBUG Adding transitive dependency for bpy==4.2.0: zstandard*
DEBUG Found stale response for: <internal repo>/zstandard/
DEBUG Sending revalidation request for: <internal repo>/zstandard/
DEBUG Found stale response for: <internal repo>/requests/
DEBUG Sending revalidation request for: <internal repo>/requests/
DEBUG Found not-modified response for: <internal repo>/typer/
DEBUG Found stale response for: <internal repo>/cython/
DEBUG Sending revalidation request for: <internal repo>/cython/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ae/cc/15083dcde1252a663398b1b2a173637a3ec65adadfb95137dc95df1e6adc/typer-0.12.4-py3-none-any.whl.metadata
DEBUG Found not-modified response for: <internal repo>/rich/
DEBUG Found not-modified response for: <internal repo>/pillow/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/87/67/a37f6214d0e9fe57f6ae54b2956d550ca8365857f42a1ce0392bb21d9410/rich-13.7.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pillow (>=10.4.0)
DEBUG Selecting: pillow==10.4.0 [compatible] (pillow-10.4.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f4/5f/491dafc7bbf5a3cc1845dc0430872e8096eb9e2b6f8161509d124594ec2d/pillow-10.4.0-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Searching for a compatible version of largestinteriorrectangle (>=0.2.1)
DEBUG Selecting: largestinteriorrectangle==0.2.1 [compatible] (largestinteriorrectangle-0.2.1-py3-none-any.whl)
DEBUG Adding transitive dependency for largestinteriorrectangle==0.2.1: numba*
DEBUG Found not-modified response for: <internal repo>/wrapt/
DEBUG Found stale response for: <internal repo>/numba/
DEBUG Sending revalidation request for: <internal repo>/numba/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0f/16/ea627d7817394db04518f62934a5de59874b587b792300991b3c347ff5e0/wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Found not-modified response for: <internal repo>/numpy/
DEBUG Found not-modified response for: <internal repo>/wand/
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Found not-modified response for: <internal repo>/opencv-python/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/59/d5/1bdd7c9662d5e9078e25ba0eb69bdb122859295746d40ab8dfef3a7b4d42/Wand-0.6.13-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d5/43/4ff735420b31cd454e4b3acdd0ba7570b453aede6fa16cf7a11cc8780d1b/numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl.metadata
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/66/82/564168a349148298aca281e342551404ef5521f33fba17b388ead0a84dc5/opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl.metadata
DEBUG Adding transitive dependency for opencv-python==4.10.0.84: numpy{python_full_version >= '3.10'}>=1.21.2
DEBUG Adding transitive dependency for opencv-python==4.10.0.84: numpy{python_full_version >= '3.10' and platform_system == 'Darwin'}>=1.21.4
DEBUG Adding transitive dependency for opencv-python==4.10.0.84: numpy{python_full_version >= '3.11'}>=1.23.5
DEBUG Adding transitive dependency for opencv-python==4.10.0.84: numpy{python_full_version >= '3.7'}>=1.17.0
DEBUG Adding transitive dependency for opencv-python==4.10.0.84: numpy{python_full_version >= '3.8'}>=1.17.3
DEBUG Adding transitive dependency for opencv-python==4.10.0.84: numpy{python_full_version >= '3.9'}>=1.19.3
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Adding transitive dependency for numpy==2.1.0: numpy==2.1.0
DEBUG Adding transitive dependency for numpy==2.1.0: numpy{python_full_version >= '3.9'}==2.1.0
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Adding transitive dependency for numpy==2.1.0: numpy==2.1.0
DEBUG Adding transitive dependency for numpy==2.1.0: numpy{python_full_version >= '3.10' and platform_system == 'Darwin'}==2.1.0
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Adding transitive dependency for numpy==2.1.0: numpy==2.1.0
DEBUG Adding transitive dependency for numpy==2.1.0: numpy{python_full_version >= '3.7'}==2.1.0
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Adding transitive dependency for numpy==2.1.0: numpy==2.1.0
DEBUG Adding transitive dependency for numpy==2.1.0: numpy{python_full_version >= '3.8'}==2.1.0
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Adding transitive dependency for numpy==2.1.0: numpy==2.1.0
DEBUG Adding transitive dependency for numpy==2.1.0: numpy{python_full_version >= '3.11'}==2.1.0
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Adding transitive dependency for numpy==2.1.0: numpy==2.1.0
DEBUG Adding transitive dependency for numpy==2.1.0: numpy{python_full_version >= '3.10'}==2.1.0
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Adding transitive dependency for rich==13.7.1: markdown-it-py>=2.2.0
DEBUG Adding transitive dependency for rich==13.7.1: pygments>=2.13.0, <3.0.0
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Adding transitive dependency for typer==0.12.4: click>=8.0.0
DEBUG Adding transitive dependency for typer==0.12.4: typing-extensions>=3.7.4.3
DEBUG Adding transitive dependency for typer==0.12.4: shellingham>=1.3.0
DEBUG Adding transitive dependency for typer==0.12.4: rich>=10.11.0
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Adding transitive dependency for typer==0.12.4: typer==0.12.4
DEBUG Adding transitive dependency for typer==0.12.4: typer[all]==0.12.4
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found stale response for: <internal repo>/shellingham/
DEBUG Sending revalidation request for: <internal repo>/shellingham/
DEBUG Found stale response for: <internal repo>/markdown-it-py/
DEBUG Sending revalidation request for: <internal repo>/markdown-it-py/
DEBUG Found stale response for: <internal repo>/pygments/
DEBUG Sending revalidation request for: <internal repo>/pygments/
DEBUG Found stale response for: <internal repo>/click/
DEBUG Sending revalidation request for: <internal repo>/click/
DEBUG Found stale response for: <internal repo>/typing-extensions/
DEBUG Sending revalidation request for: <internal repo>/typing-extensions/
DEBUG Found not-modified response for: <internal repo>/cython/
DEBUG Found not-modified response for: <internal repo>/zstandard/
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Found not-modified response for: <internal repo>/requests/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e8/46/66d5b55f4d737dd6ab75851b224abf0afe5774976fe511a54d2eb9063a41/zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/43/39/bdbec9142bc46605b54d674bf158a78b191c2b75be527c6dcf3e6dfe90b8/Cython-3.0.11-py2.py3-none-any.whl.metadata
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f9/9b/335f9764261e915ed497fcdeb11df5dfd6f7bf257d4a6a2a686d80da4d54/requests-2.32.3-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for requests==2.32.3: charset-normalizer>=2, <4
DEBUG Adding transitive dependency for requests==2.32.3: idna>=2.5, <4
DEBUG Adding transitive dependency for requests==2.32.3: urllib3>=1.21.1, <3
DEBUG Adding transitive dependency for requests==2.32.3: certifi>=2017.4.17
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found not-modified response for: <internal repo>/numba/
DEBUG Found stale response for: <internal repo>/idna/
DEBUG Sending revalidation request for: <internal repo>/idna/
DEBUG Found stale response for: <internal repo>/certifi/
DEBUG Sending revalidation request for: <internal repo>/certifi/
DEBUG Searching for a compatible version of numba (*)
DEBUG Found stale response for: <internal repo>/urllib3/
DEBUG Sending revalidation request for: <internal repo>/urllib3/
DEBUG Selecting: numba==0.60.0 [compatible] (numba-0.60.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found stale response for: <internal repo>/charset-normalizer/
DEBUG Sending revalidation request for: <internal repo>/charset-normalizer/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9a/51/a4dc2c01ce7a850b8e56ff6d5381d047a5daea83d12bad08aa071d34b2ee/numba-0.60.0-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Adding transitive dependency for numba==0.60.0: llvmlite>=0.43.0.dev0, <0.44
DEBUG Adding transitive dependency for numba==0.60.0: numpy>=1.22, <2.1
DEBUG Searching for a compatible version of numba (<0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.60.0rc1 [compatible] (numba-0.60.0rc1-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1d/2e/7494468ed966af8d98784ff79186407f7d593cf378d2a130f1ff9117c70d/numba-0.60.0rc1-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Adding transitive dependency for numba==0.60.0rc1: llvmlite>=0.43.0.dev0, <0.44
DEBUG Adding transitive dependency for numba==0.60.0rc1: numpy>=1.22, <2.1
DEBUG Searching for a compatible version of numba (<0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.59.1 [compatible] (numba-0.59.1-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/70/7d/0d1419479997319ca72ef735791c2ee50819f9c200adea96142ee7499fae/numba-0.59.1-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Adding transitive dependency for numba==0.59.1: llvmlite>=0.42.0.dev0, <0.43
DEBUG Adding transitive dependency for numba==0.59.1: numpy>=1.22, <1.27
DEBUG Searching for a compatible version of numba (<0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.59.0 [compatible] (numba-0.59.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ef/20/94ef7b3afee76f47f3ad2d9dfc64f5cb29a365df00e5c563e7518e761bc0/numba-0.59.0-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Adding transitive dependency for numba==0.59.0: llvmlite>=0.42.0.dev0, <0.43
DEBUG Adding transitive dependency for numba==0.59.0: numpy>=1.22, <1.27
DEBUG Searching for a compatible version of numba (<0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.59.0rc1 [compatible] (numba-0.59.0rc1-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Prefetching 5 numba versions
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/60/aa67255e6e166ef6541d007f22470fc459b8c7b66fb6790fd735d0bcd951/numba-0.58.1-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/52/62/d390a5bd04094c090ea2a9f4423ac5bf9819d9563c87f4917ed9ae6fe5d2/numba-0.59.0rc1-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Adding transitive dependency for numba==0.59.0rc1: llvmlite>=0.42.0.dev0, <0.43
DEBUG Adding transitive dependency for numba==0.59.0rc1: numpy>=1.22, <1.27
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1b/2e/1d80831b015606a6743ea4bf60ab1c91e7130ff1155381524a1dab0e8334/numba-0.58.0-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Searching for a compatible version of numba (<0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.58.1 [compatible] (numba-0.58.1-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d5/a0/9c4bbce73217d53180a1d779759d98aaf6293385463b755f394b917d334f/numba-0.58.0rc2-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5e/c7/3269460a1828ec98d970053768e796b0d1a957c867576364dbc20db8dbb9/numba-0.58.0rc1-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Adding transitive dependency for numba==0.58.1: llvmlite>=0.41.0.dev0, <0.42
DEBUG Adding transitive dependency for numba==0.58.1: numpy>=1.22, <1.27
DEBUG Searching for a compatible version of numba (<0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.58.0 [compatible] (numba-0.58.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Adding transitive dependency for numba==0.58.0: llvmlite>=0.41.0.dev0, <0.42
DEBUG Adding transitive dependency for numba==0.58.0: numpy>=1.21, <1.26
DEBUG Searching for a compatible version of numba (<0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.58.0rc2 [compatible] (numba-0.58.0rc2-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found stale response for: <internal repo>/llvmlite/
DEBUG Sending revalidation request for: <internal repo>/llvmlite/
DEBUG Adding transitive dependency for numba==0.58.0rc2: llvmlite>=0.41.0.dev0, <0.42
DEBUG Adding transitive dependency for numba==0.58.0rc2: numpy>=1.21, <1.26
DEBUG Searching for a compatible version of numba (<0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.58.0rc1 [compatible] (numba-0.58.0rc1-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Adding transitive dependency for numba==0.58.0rc1: llvmlite>=0.41.0.dev0, <0.42
DEBUG Adding transitive dependency for numba==0.58.0rc1: numpy>=1.21, <1.26
DEBUG Searching for a compatible version of numba (<0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.57.1 [compatible] (numba-0.57.1-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ce/eb/c982ad64cc2a4cc0a6b95ea94da5566874a6eaffc585c789ef2dd77fc06a/numba-0.57.1-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/76/3d/0abf11b5a9d1f0bd276191adb902ceb9472f7c80796ff85e8d3fe6b270e0/numba-0.57.1rc1-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6d/07/0659e378163e17398254d779ca10c948713f996d87121fb328f13d1affdf/numba-0.57.0-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/63/9d/13bc90e0b4d8ecb9e07c9a5e094a1db4fd89beb67420013d473f4e32f4ba/numba-0.57.0rc1-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1a/66/de416cd8364c7e5cba8da9272809676e907e7045cdcb750f6ff5fff70c29/numba-0.56.4-cp310-cp310-macosx_10_14_x86_64.whl.metadata
DEBUG Prefetching 9 numba versions
DEBUG Adding transitive dependency for numba==0.57.1: llvmlite>=0.40.0.dev0, <0.41
DEBUG Adding transitive dependency for numba==0.57.1: numpy>=1.21, <1.25
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b8/1b/2b2bba63f18b89da7d2a4e884e0800342484aa11dff59598f82c5b278b66/numba-0.56.3-cp310-cp310-macosx_10_14_x86_64.whl.metadata
DEBUG Searching for a compatible version of numba (<0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.57.1rc1 [compatible] (numba-0.57.1rc1-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/03/92/ecf69b298baf7af49243f2e30e75c401929430511c1909928e7daa4daf4d/numba-0.56.2-cp310-cp310-macosx_10_14_x86_64.whl.metadata
DEBUG Adding transitive dependency for numba==0.57.1rc1: llvmlite>=0.40.0.dev0, <0.41
DEBUG Adding transitive dependency for numba==0.57.1rc1: numpy>=1.21, <1.25
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b8/79/e867e49151ae6adbd23f5566a722feb0b7cc65b60b1556b63ad55be37857/numba-0.56.0-cp310-cp310-macosx_10_14_x86_64.whl.metadata
DEBUG Searching for a compatible version of numba (<0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.57.0 [compatible] (numba-0.57.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9f/b0/4122ae6e79a8fdebe5769badf6fcfeb59f2fa3c23195872fb0beb96b4ddc/numba-0.56.0rc1-cp310-cp310-macosx_10_11_x86_64.whl.metadata
DEBUG Adding transitive dependency for numba==0.57.0: llvmlite>=0.40.0.dev0, <0.41
DEBUG Adding transitive dependency for numba==0.57.0: numpy>=1.21, <1.25
DEBUG Searching for a compatible version of numba (<0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.57.0rc1 [compatible] (numba-0.57.0rc1-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Adding transitive dependency for numba==0.57.0rc1: llvmlite>=0.40.0.dev0, <0.41
DEBUG Adding transitive dependency for numba==0.57.0rc1: numpy>=1.21, <1.25
DEBUG Searching for a compatible version of numba (<0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.56.4 [compatible] (numba-0.56.4.tar.gz)
DEBUG Adding transitive dependency for numba==0.56.4: llvmlite>=0.39.0.dev0, <0.40
DEBUG Adding transitive dependency for numba==0.56.4: numpy>=1.18, <1.24
DEBUG Adding transitive dependency for numba==0.56.4: setuptools*
DEBUG Searching for a compatible version of numba (<0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.56.3 [compatible] (numba-0.56.3.tar.gz)
DEBUG Adding transitive dependency for numba==0.56.3: llvmlite>=0.39.0.dev0, <0.40
DEBUG Adding transitive dependency for numba==0.56.3: numpy>=1.18, <1.24
DEBUG Adding transitive dependency for numba==0.56.3: setuptools*
DEBUG Searching for a compatible version of numba (<0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.56.2 [compatible] (numba-0.56.2.tar.gz)
DEBUG Adding transitive dependency for numba==0.56.2: llvmlite>=0.39.0.dev0, <0.40
DEBUG Adding transitive dependency for numba==0.56.2: numpy>=1.18, <1.24
DEBUG Adding transitive dependency for numba==0.56.2: setuptools<60
DEBUG Searching for a compatible version of numba (<0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.56.0 [compatible] (numba-0.56.0.tar.gz)
DEBUG Adding transitive dependency for numba==0.56.0: llvmlite>=0.39.0.dev0, <0.40
DEBUG Adding transitive dependency for numba==0.56.0: numpy>=1.18, <1.23
DEBUG Adding transitive dependency for numba==0.56.0: setuptools*
DEBUG Searching for a compatible version of numba (<0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.56.0rc1 [compatible] (numba-0.56.0rc1.tar.gz)
DEBUG Adding transitive dependency for numba==0.56.0rc1: llvmlite>=0.39.0.dev0, <0.40
DEBUG Adding transitive dependency for numba==0.56.0rc1: numpy>=1.18, <1.23
DEBUG Adding transitive dependency for numba==0.56.0rc1: setuptools*
DEBUG Searching for a compatible version of numba (<0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG No compatible version found for: Python
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found stale response for: <internal repo>/setuptools/
DEBUG Sending revalidation request for: <internal repo>/setuptools/
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numba (<0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numba (<0.55.1 | >0.55.1, <0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numba (<0.55.0 | >0.55.0, <0.55.1 | >0.55.1, <0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8a/24/0686f0d139cbcfd6f3d16710498f94513e2338f7b01ddc6f06626ddf8cb2/numpy-2.1.0rc1-cp311-cp311-macosx_14_0_arm64.whl.metadata
DEBUG Prefetching 4 numpy versions
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Prefetching 4 typer versions
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d1/d8/597b4b2e396a77cbec677c9de33bb1789d5c3b66d653cb723d00eb331e99/numpy-2.0.1-cp311-cp311-macosx_14_0_arm64.whl.metadata
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Prefetching 4 opencv-python versions
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/01/4a/611a907421d8098d5edc8c2b10c3583796ee8da4156f8f7de52c2f4c9d90/numpy-2.0.0-cp311-cp311-macosx_14_0_arm64.whl.metadata
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/20/b5/11cf2e34fbb11b937e006286ab5b8cfd334fde1c8fa4dd7f491226931180/typer-0.12.3-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/2e/54923a9c4416a1ca23dcd5c023b6ad5e20dfa31366ed3bd894dee7741859/typer-0.12.2-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/45/12/0111044f478fb35338f1372e397327d3dac7b35d4cc0e1cca1d3e2be6f20/typer-0.12.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d1/2c/285651e5dbdbd197234c9d000af710f128bce6f851213ec503e399b8f6c0/opencv_python-4.10.0.82-cp37-abi3-macosx_11_0_arm64.whl.metadata
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/77/df/b56175c3fb5bc058774bdcf35f5a71cf9c3c5b909f98a1c688eb71cd3b1f/opencv_python-4.9.0.80-cp37-abi3-macosx_11_0_arm64.whl.metadata
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a1/f6/57de91ea40c670527cd47a6548bf2cbedc68cec57c041793b256356abad7/opencv_python-4.8.1.78-cp37-abi3-macosx_11_0_arm64.whl.metadata
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Prefetching 4 rich versions
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Prefetching 4 wand versions
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/be/be/1520178fa01eabe014b16e72a952b9f900631142ccd03dc36cf93e30c1ce/rich-13.7.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/be/2a/4e62ff633612f746f88618852a626bbe24226eba5e7ac90e91dcfd6a414e/rich-13.6.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c1/d1/23ba6235ed82883bb416f57179d1db2c05f3fb8e5d83c18660f9ab6f09c9/rich-13.5.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/2c/67/16814b85c6ca13273107ea0b30e64483e8525964c296671071f660e0e510/Wand-0.6.12-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/bb/f0/a3614d7b2bfdff543b8f864209055c29c29f4e9e3b2e167a324bb59019d0/Wand-0.6.10-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/fa/a8/f76bbafa1a42f1ed45ece3c2b2e4f70a5d6cdb5f38bb0780db8efcdc58ab/Wand-0.6.11-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cb/70/9399c0fbb416df7861bd9f00327fbb7806ef12e4da3acbcd5e6ac378da21/wrapt-1.16.0rc2-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/12/92/dfb31d96332bf7ffee6f2022d33f8f1c35563a0961bcf8f32054723cea85/wrapt-1.16.0rc1-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Prefetching 4 wrapt versions
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6b/b0/bde5400fdf6d18cb7ef527831de0f86ac206c4da1670b67633e5a547b05f/wrapt-1.15.0-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b6/83/b0a63fc7b315edd46821a1a381d18765c1353d201246da44558175cddd56/Cython-3.0.10-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/fc/84/5748f255f1689c95cf8dd0f2789480f7261ec0c82880a6a1f4daead6894d/Cython-3.0.9-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e3/7f/f584f5d15323feb897d42ef0e9d910649e2150d7a30cf7e7a8cc1d236e6f/Cython-3.0.8-py2.py3-none-any.whl.metadata
DEBUG Prefetching 5 cython versions
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Prefetching 3 requests versions
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/da/6a/5ec6c7d2239240f937eb0dbe6d436fe5f09677e26a957b56b0dbf61e6dba/Cython-3.0.7-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c3/20/748e38b466e0819491f0ce6e90ebe4184966ee304fe483e2c414b0f4ef07/requests-2.32.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/70/8e/0e2d847013cb52cd35b38c009bb167a1a26b2ce6cd6965bf26b47bc0bf44/requests-2.31.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/54/fc/c1b1a1e140451f3362789f546731b3ef36c78668be19d7fc6fbd4326b535/zstandard-0.22.0-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Prefetching 5 zstandard versions
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/41/ca/26d887cb8807f61256604ff6d73f455c47b5f1272f76a4dbab789d349cad/zstandard-0.21.0-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Searching for a compatible version of numba (<0.55.0rc1 | >0.55.0rc1, <0.55.0 | >0.55.0, <0.55.1 | >0.55.1, <0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/73/eb/6f5c09799fbf9525fd1a374c11b0116ae77e926fe26d2bce9054036396b2/zstandard-0.20.0-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ea/07/f26a540019dc16fe34740db50d81e61f61f5dc18f17f41b3f9f2423b5bb0/zstandard-0.19.0-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numba (<0.54.1 | >0.54.1, <0.55.0rc1 | >0.55.0rc1, <0.55.0 | >0.55.0, <0.55.1 | >0.55.1, <0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Searching for a compatible version of numba (<0.54.1rc1 | >0.54.1rc1, <0.54.1 | >0.54.1, <0.55.0rc1 | >0.55.0rc1, <0.55.0 | >0.55.0, <0.55.1 | >0.55.1, <0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numba (<0.54.0 | >0.54.0, <0.54.1rc1 | >0.54.1rc1, <0.54.1 | >0.54.1, <0.55.0rc1 | >0.55.0rc1, <0.55.0 | >0.55.0, <0.55.1 | >0.55.1, <0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numba (<0.54.0rc3 | >0.54.0rc3, <0.54.0 | >0.54.0, <0.54.1rc1 | >0.54.1rc1, <0.54.1 | >0.54.1, <0.55.0rc1 | >0.55.0rc1, <0.55.0 | >0.55.0, <0.55.1 | >0.55.1, <0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numba (<0.54.0rc2 | >0.54.0rc2, <0.54.0rc3 | >0.54.0rc3, <0.54.0 | >0.54.0, <0.54.1rc1 | >0.54.1rc1, <0.54.1 | >0.54.1, <0.55.0rc1 | >0.55.0rc1, <0.55.0 | >0.55.0, <0.55.1 | >0.55.1, <0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG No compatible version found for: Python
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/47/51/4b3add94e7cf07a46ad15bbcd6034d423055caf23bea37eb0b26b4e6a800/numpy-2.0.0rc2-cp311-cp311-macosx_14_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d1/08/c8cca7a4269610bec74d18c473eda34d8e5147cda5180da37e2a3429b064/numpy-2.0.0rc1-cp311-cp311-macosx_14_0_arm64.whl.metadata
DEBUG Found not-modified response for: <internal repo>/markdown-it-py/
DEBUG Prefetching 9 numpy versions
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Prefetching 9 typer versions
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b4/28/b9051cde1ec22f5e8075c597d1e97f63d3178f7bfc50f04e306a73d42b96/numpy-2.0.0b1-cp311-cp311-macosx_14_0_arm64.whl.metadata
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1a/2e/151484f49fd03944c4a3ad9c418ed193cfd02724e138ac8a9505d056c582/numpy-1.26.4-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Prefetching 8 opencv-python versions
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Prefetching 9 rich versions
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Prefetching 9 wand versions
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/55/78/f85aab3bda3ddffe6ce8c590190b5f0d2e61dfd2fb7a8f446dcb4f8c12c7/numpy-1.26.3-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ca/51/a626892d0749af127b2a640797d1a1bfc6a33e529fb90066e9974c3ad8e1/typer-0.12.1.dev1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/25/85/e2de8edd0a9698dd69ece8ba657716a6d1e543b652871ff1b7891b937a77/typer-0.12.1.dev2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d8/21/6a9c5c3363e30345346f59d236fd416730c2ac7802abe3609fed89d46ac1/typer-0.12.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3d/92/3fa7ae1ee9dcb0b283e9a11c9df8b0cbb362ebb0ecc7fb7171ac2e3689f9/typer-0.12.dev2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6c/a9/d99bd061356f6a8e2aa110b2ce48ced1208906bb3ca3ea3766f6073bf42b/typer-0.12.dev1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/32/a6/4321f0f30ee11d6d85f49251d417f4e885fe7638b5ac50b7e3c80cccf141/opencv_python-4.8.0.76-cp37-abi3-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b1/ca/69f7f2204d5779ad939123cce9735f76db8f50e2b482e45b413c585b0b27/opencv_python-4.8.0.74-cp37-abi3-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/16/91/b55ca570e3a96d241a7e76bcfe2f73d219cda01411bbd4214e7d12d5f98c/opencv_python-4.7.0.72-cp37-abi3-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c6/00/858e894a2127b3a1b5479812b1bd3669bca64175dff4fc7c778e0a6ee565/opencv_python-4.6.0.66-cp37-abi3-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8d/5f/21a93b2ec205f4b79853ff6e838e3c99064d5dbe85ec6b05967506f14af0/rich-13.5.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/83/d5/311cc7acdc4dfb146e419c5975c23ffca4e3023a6f41588af35d19718e0c/rich-13.5.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cf/2c/aa1418e8fafb5489c6fbfb36ba6afc1be72b196419bdea0c501487743740/rich-13.5.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ea/93/c68645c689d10a035010e3ae314b6b2855d040ce0d11fdfdfbb8be416581/rich-13.4.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/84/6a/13ce8a674c331686f138d3fba6a461f49426e59a0806932027ae25254a87/Wand-0.6.9-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/fc/1e/482e5eec0b89b593e81d78f819a9412849814e22225842b598908e7ac560/rich-13.4.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/83/b1/5e085e8dcd7402734ebb0b79cdbd8e5bdf66155dfdc7b4c879485d03cdca/Wand-0.6.8-py2.py3-none-any.whl.metadata
DEBUG Prefetching 9 wrapt versions
DEBUG Searching for a compatible version of cython (*)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/25/1c/55c69811efdfc7623b70168f25f9d22b593047ba0bd4b99c066a2a5a2c12/Wand-0.6.7-py2.py3-none-any.whl.metadata
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/98/08/096b76e9211ca5ef338791100b76375555cb4082a53496b1c1d5897ee13c/Wand-0.6.5-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d7/f6/05f043c099639b9017b7244791048a4d146dfea45b41a199aed373246d50/Wand-0.6.6-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5e/3e/42da597c06e757b5036f28c1972fe88a0f5117f84324346d5fbd27d34eeb/wrapt-1.15.0rc1-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/6e/79/aec8185eefe20e8f49e5adeb0c2e20e016d5916d10872c17705ddac41be2/wrapt-1.14.1-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4a/20/bcded6b17aa6dec73521b60a77dffbff15cf8aedea9388663296506b7b1e/wrapt-1.14.0-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e2/eb/740b3703f0af59654eec3eed0aa70b23e89def1857dd5caeba006633bcfe/wrapt-1.14.0rc1-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/86/9ca2f9c095a0e7389cc0fa3df0995073de34628aa15a9b09829dd598e8e5/wrapt-1.13.3-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/de/08/98f58494beb69392491b688264ed08259ce453d624059dcefc3fe37e5b5d/Cython-3.0.6-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/fb/fe/e213d8e9cb21775bb8f9c92ff97861504129e23e33d118be1a90ca26a13e/Cython-3.0.5-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/8a/fd/6cb52d1f5771e3a7c05361caab83c78e7aa4f70acace1b9974f1ed84b805/Cython-3.0.4-py2.py3-none-any.whl.metadata
DEBUG Prefetching 10 cython versions
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a5/70/11e973497f9f635654ef1fefffd0573f83813a65cd9221215f08192fb531/Cython-3.0.3-py2.py3-none-any.whl.metadata
DEBUG Prefetching 8 requests versions
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/03/e9/9cc0c4f0d8a566089d096254cd25168a0db02dd047863a7f995d8d3eefa7/Cython-3.0.2-py2.py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/96/80/034ffeca15c0f4e01b7b9c6ad0fb704b44e190cde4e757edbd60be404c41/requests-2.30.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cf/e1/2aa539876d9ed0ddc95882451deb57cfd7aa8dbf0b8dbce68e045549ba56/requests-2.29.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ca/91/6d9b8ccacd0412c08820f72cebaa4f0c0441b5cda699c90f618b6f8a1b42/requests-2.28.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d2/f4/274d1dbe96b41cf4e0efb70cbced278ffd61b5c7bb70338b62af94ccb25b/requests-2.28.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/41/5b/2209eba8133fc081d3ffff02e1f6376e3117e52bb16f674721a83e67e68e/requests-2.28.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c6/8d/a0f6e793ef95aca4e53389bfd271ec29b6d1e705e746553eb032626d8a8c/zstandard-0.18.0-cp310-cp310-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/86/b3/85fe4eb7b132874cb644f7358652c7708bc48e3b60f8ff9cc28ab60a34ed/zstandard-0.17.0-cp310-cp310-macosx_10_9_x86_64.whl.metadata
DEBUG Prefetching 10 zstandard versions
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/43/c1/adffc388027293dc11e37a24c61809d7c113f57bb2e73b8810e8165b86bb/zstandard-0.16.0-cp310-cp310-macosx_10_9_x86_64.whl.metadata
DEBUG Searching for a compatible version of numba (<0.53.1 | >0.53.1, <0.54.0rc2 | >0.54.0rc2, <0.54.0rc3 | >0.54.0rc3, <0.54.0 | >0.54.0, <0.54.1rc1 | >0.54.1rc1, <0.54.1 | >0.54.1, <0.55.0rc1 | >0.55.0rc1, <0.55.0 | >0.55.0, <0.55.1 | >0.55.1, <0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/18/6b/22e32b245f782f95e1c53fd37c232063ff8e08a5060cdc81955f8e2b8a9b/zstandard-0.15.2-cp35-cp35m-macosx_10_9_x86_64.whl.metadata
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1c/9d/ffad757bf05de76fc1aad0decf7ac1fbb45655a037bd91d2b2d7e047813c/zstandard-0.15.1-cp35-cp35m-macosx_10_9_x86_64.whl.metadata
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numba (<0.53.0 | >0.53.0, <0.53.1 | >0.53.1, <0.54.0rc2 | >0.54.0rc2, <0.54.0rc3 | >0.54.0rc3, <0.54.0 | >0.54.0, <0.54.1rc1 | >0.54.1rc1, <0.54.1 | >0.54.1, <0.55.0rc1 | >0.55.0rc1, <0.55.0 | >0.55.0, <0.55.1 | >0.55.1, <0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numba (<0.53.0rc3 | >0.53.0rc3, <0.53.0 | >0.53.0, <0.53.1 | >0.53.1, <0.54.0rc2 | >0.54.0rc2, <0.54.0rc3 | >0.54.0rc3, <0.54.0 | >0.54.0, <0.54.1rc1 | >0.54.1rc1, <0.54.1 | >0.54.1, <0.55.0rc1 | >0.55.0rc1, <0.55.0 | >0.55.0, <0.55.1 | >0.55.1, <0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numba (<0.53.0rc2 | >0.53.0rc2, <0.53.0rc3 | >0.53.0rc3, <0.53.0 | >0.53.0, <0.53.1 | >0.53.1, <0.54.0rc2 | >0.54.0rc2, <0.54.0rc3 | >0.54.0rc3, <0.54.0 | >0.54.0, <0.54.1rc1 | >0.54.1rc1, <0.54.1 | >0.54.1, <0.55.0rc1 | >0.55.0rc1, <0.55.0 | >0.55.0, <0.55.1 | >0.55.1, <0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numba (<0.53.0rc1.post1 | >0.53.0rc1.post1, <0.53.0rc2 | >0.53.0rc2, <0.53.0rc3 | >0.53.0rc3, <0.53.0 | >0.53.0, <0.53.1 | >0.53.1, <0.54.0rc2 | >0.54.0rc2, <0.54.0rc3 | >0.54.0rc3, <0.54.0 | >0.54.0, <0.54.1rc1 | >0.54.1rc1, <0.54.1 | >0.54.1, <0.55.0rc1 | >0.55.0rc1, <0.55.0 | >0.55.0, <0.55.1 | >0.55.1, <0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numba (<0.52.0 | >0.52.0, <0.53.0rc1.post1 | >0.53.0rc1.post1, <0.53.0rc2 | >0.53.0rc2, <0.53.0rc3 | >0.53.0rc3, <0.53.0 | >0.53.0, <0.53.1 | >0.53.1, <0.54.0rc2 | >0.54.0rc2, <0.54.0rc3 | >0.54.0rc3, <0.54.0 | >0.54.0, <0.54.1rc1 | >0.54.1rc1, <0.54.1 | >0.54.1, <0.55.0rc1 | >0.55.0rc1, <0.55.0 | >0.55.0, <0.55.1 | >0.55.1, <0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Searching for a compatible version of numpy (>=2.0.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of typer (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (>=0.12.3)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of typer[all] (==0.12.4)
DEBUG Selecting: typer==0.12.4 [compatible] (typer-0.12.4-py3-none-any.whl)
DEBUG Searching for a compatible version of opencv-python (>=4.10.0.84)
DEBUG Selecting: opencv-python==4.10.0.84 [compatible] (opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (>=1.19.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (>=1.21.2)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.9'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/14/00/4431a5ea3388fcecf781aa1c1c4f660d4e7ff5a2d400741f3106b6fbb482/numba-0.52.0rc2-cp36-cp36m-macosx_10_14_x86_64.whl.metadata
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (>=1.21.4)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (>=1.17.3)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.10' and platform_system == 'Darwin'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.8'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (>=1.17.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (>=1.23.5)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.7'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of numpy{python_full_version >= '3.11'} (==2.1.0)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Searching for a compatible version of rich (>=13.7.1)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Searching for a compatible version of wand (>=0.6.13)
DEBUG Selecting: wand==0.6.13 [compatible] (Wand-0.6.13-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of wrapt (>=1.16.0)
DEBUG Selecting: wrapt==1.16.0 [compatible] (wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of cython (*)
DEBUG Selecting: cython==3.0.11 [compatible] (Cython-3.0.11-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of requests (*)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Searching for a compatible version of zstandard (*)
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Searching for a compatible version of numba (<0.52.0rc3 | >0.52.0rc3, <0.52.0 | >0.52.0, <0.53.0rc1.post1 | >0.53.0rc1.post1, <0.53.0rc2 | >0.53.0rc2, <0.53.0rc3 | >0.53.0rc3, <0.53.0 | >0.53.0, <0.53.1 | >0.53.1, <0.54.0rc2 | >0.54.0rc2, <0.54.0rc3 | >0.54.0rc3, <0.54.0 | >0.54.0, <0.54.1rc1 | >0.54.1rc1, <0.54.1 | >0.54.1, <0.55.0rc1 | >0.55.0rc1, <0.55.0 | >0.55.0, <0.55.1 | >0.55.1, <0.55.2 | >0.55.2, <0.56.0rc1 | >0.56.0rc1, <0.56.0 | >0.56.0, <0.56.2 | >0.56.2, <0.56.3 | >0.56.3, <0.56.4 | >0.56.4, <0.57.0rc1 | >0.57.0rc1, <0.57.0 | >0.57.0, <0.57.1rc1 | >0.57.1rc1, <0.57.1 | >0.57.1, <0.58.0rc1 | >0.58.0rc1, <0.58.0rc2 | >0.58.0rc2, <0.58.0 | >0.58.0, <0.58.1 | >0.58.1, <0.59.0rc1 | >0.59.0rc1, <0.59.0 | >0.59.0, <0.59.1 | >0.59.1, <0.60.0rc1 | >0.60.0rc1, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.52.0rc2 [compatible] (numba-0.52.0rc2.tar.gz)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c2/19/1722feae3fd48fa4d89f10ba364e9f6e06a405b9862a4beb4d211dc79e0d/numba-0.51.2-cp36-cp36m-macosx_10_14_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/fa/61/c3a10f4d66d12f9b8afa2513e0e3640080b4b597ff1dc601d810d611b6ee/numba-0.51.1-cp36-cp36m-macosx_10_14_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/97/c2/c0d0ae4fc3a157aad51027b0699e4ff7f29e782730dc2ec0af8fad63c9e4/numba-0.51.0-cp36-cp36m-macosx_10_14_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4b/23/3bfb72cc7427f75f11fc367d93c24cc78bd37c9a462697940b40622cf7e0/numba-0.51.0rc1-cp36-cp36m-macosx_10_14_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/bb/c5/f736704ed0129b4351a725a7f9fe926d028adfb684ecf74bf6e6262cc341/numba-0.50.1-cp36-cp36m-macosx_10_14_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/29/73/4d04b2657dbb32526c5516ef153a7ff83608f94645e1426a78b55669f404/numba-0.50.0-cp36-cp36m-macosx_10_14_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/12/bb/0e1c02430e91c206aebff7859bde94d4cd9b5ef278d6873cf2081276bead/numba-0.50.0rc1-cp36-cp36m-macosx_10_14_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ac/7d/e01bcbf4a35cbb07ce04aaa545030bde231331aaabe7c44726b099f8fd86/numba-0.49.1-cp36-cp36m-macosx_10_14_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c3/c9/c55393c40e429d08a3ca5c4de1a6a947a9bc212b8ff7899bc61747aaf934/numba-0.49.1rc1-cp36-cp36m-macosx_10_14_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5f/8e/cb7bb3e872aa4c6a6603c15b37964521d16a46dcb2e40171d6bdaafff62a/numba-0.49.0-cp36-cp36m-macosx_10_14_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/84/36/f3b918f9db04919bcc5c3ce5bb3613920f5341cd8309c826bec787213347/numba-0.47.0-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/25/59/6a67846fdec2688a4b8f1a3a55ab77e47c109615200f1b58e4cd6001f48e/numba-0.48.0-1-cp36-cp36m-manylinux1_i686.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3c/8c/1a7f86bfc6f06a633f4047a1795791900ae48442194f38ee63aad2196179/numba-0.45.1-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cd/0f/2f22d8ee83c7d7d3b6d773f8fe6eb09d37e25a8551ce01caeebeb38acce5/numba-0.46.0-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cb/97/6a9a67892301a25d62563abd8b648547eb008c0429fab1e9f22d690168cd/numba-0.45.0-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Prefetching 32 numba versions
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d1/73/9c93d7c8ef6832ea240a24a781c98f05ed0b2550bd818e4c6a35eb3bc999/numba-0.44.0-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/58/a7/6d345a3c4b11536db7c01e8af84383c96d80805ed55d2d795fa8465130d5/numba-0.43.1-1-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d0/8f/e91c4e961defbbc7fb00c8b4549f38bcefea385fbccef67ef28574e46384/numba-0.44.1-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Adding transitive dependency for numba==0.52.0rc2: llvmlite>=0.35.0.dev0, <0.36
DEBUG Adding transitive dependency for numba==0.52.0rc2: numpy>=1.15
DEBUG Adding transitive dependency for numba==0.52.0rc2: setuptools*
DEBUG Searching for a compatible version of markdown-it-py (>=2.2.0)
DEBUG Selecting: markdown-it-py==3.0.0 [compatible] (markdown_it_py-3.0.0-py3-none-any.whl)
DEBUG Adding transitive dependency for markdown-it-py==3.0.0: mdurl>=0.1, <1.dev0
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/5c/6e/6ae3753b971d55c6211646be12ea9b9bfeee4c0b62beb5f398c4a69996c0/numba-0.42.1-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/62/16/c0e5f8207c6cf32d1527f202a4838a3820d4cb1c51d525709a4e7b66fe35/numba-0.40.1-cp27-cp27m-macosx_10_6_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/67/70/f90b24b2de827ce15ead79d36287b63a7b10bba8c4a39f3c2bb884dc9590/numba-0.41.0-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1c/15/dfcf128e1c885099581b04e4bd82c536977712741eabead2471cbfb82803/numba-0.40.0-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/c6/90/f9dcb254c193c10b5c7e2de77a0f278d9e526f1c446f2d3aea231a3a5bec/numba-0.43.0-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/62/1d/69e7627e17fbfe5ff8b11c75256685e8cfbc56d089ca04624774b0ffefd3/numba-0.39.0-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e3/b3/3ddf8f049c6bcc73738dec87e47ca0b3d516f1f43f746953bd7c52688d4b/numba-0.38.1-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a9/f4/c32ab347918e6064b34c66287f625fa3bb041052182079582b3c9548780d/numba-0.42.0-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/28/f6/a837ce0a18eaa13eb4461e624dfbcefe5cf3a8008885777011dd0eabb005/numba-0.38.0-cp27-cp27m-macosx_10_9_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/21/65/26ee33cc61212ca9fce3d590e98a645943c2b8507636904a7683044a4b15/numba-0.37.0-cp27-cp27m-macosx_10_6_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f1/f6/376f11e1269d4e560b42f8aba72aebff12e56a340c5010116f5dec4e8e2d/numba-0.36.2-cp27-cp27m-macosx_10_6_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/55/a6/0357fcc173ac738dd929076c146e8fdd3a5add34ee00fc556689285afee7/numba-0.36.1-cp27-cp27m-macosx_10_6_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/bc/29/4d008f78a8627dabc602c8667bd8cadea264b66501572d0c5d9eb874f65e/numba-0.35.0-cp27-cp27m-macosx_10_7_x86_64.whl.metadata
DEBUG Found stale response for: <internal repo>/mdurl/
DEBUG Sending revalidation request for: <internal repo>/mdurl/
DEBUG Found not-modified response for: <internal repo>/shellingham/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl.metadata
DEBUG Found not-modified response for: <internal repo>/typing-extensions/
DEBUG Found not-modified response for: <internal repo>/pygments/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pygments (>=2.13.0, <3.0.0)
DEBUG Selecting: pygments==2.18.0 [compatible] (pygments-2.18.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f7/3f/01c8b82017c199075f8f788d0d906b9ffbbc5a47dc9918a945e13d5a2bda/pygments-2.18.0-py3-none-any.whl.metadata
DEBUG Found not-modified response for: <internal repo>/click/
DEBUG Searching for a compatible version of click (>=8.0.0)
DEBUG Selecting: click==8.1.7 [compatible] (click-8.1.7-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/00/2e/d53fa4befbf2cfa713304affc7ca780ce4fc1fd8710527771b58311a3229/click-8.1.7-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of typing-extensions (>=3.7.4.3)
DEBUG Selecting: typing-extensions==4.12.2 [compatible] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of shellingham (>=1.3.0)
DEBUG Selecting: shellingham==1.5.4 [compatible] (shellingham-1.5.4-py2.py3-none-any.whl)
DEBUG Found not-modified response for: <internal repo>/charset-normalizer/
DEBUG Searching for a compatible version of charset-normalizer (>=2, <4)
DEBUG Selecting: charset-normalizer==3.3.2 [compatible] (charset_normalizer-3.3.2-cp311-cp311-macosx_11_0_arm64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/dd/51/68b61b90b24ca35495956b718f35a9756ef7d3dd4b3c1508056fa98d1a1b/charset_normalizer-3.3.2-cp311-cp311-macosx_11_0_arm64.whl.metadata
DEBUG Found not-modified response for: <internal repo>/idna/
DEBUG Searching for a compatible version of idna (>=2.5, <4)
DEBUG Selecting: idna==3.7 [compatible] (idna-3.7-py3-none-any.whl)
DEBUG Found not-modified response for: <internal repo>/setuptools/
DEBUG Found not-modified response for: <internal repo>/mdurl/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e5/3e/741d8c82801c347547f8a2a06aa57dbb1992be9e948df2ea0eda2c8b79e8/idna-3.7-py3-none-any.whl.metadata
DEBUG Found not-modified response for: <internal repo>/llvmlite/
DEBUG Found not-modified response for: <internal repo>/urllib3/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/07/6a/0270e295bf30c37567736b7fca10167640898214ff911273af37ddb95770/setuptools-73.0.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/84/06/09488c0bef4776a4750c5c579b4f0f5ec5ffdfdb6cf8afe1b6aa8f48aed6/llvmlite-0.35.0-cp36-cp36m-macosx_10_9_x86_64.whl.metadata
DEBUG Searching for a compatible version of urllib3 (>=1.21.1, <3)
DEBUG Selecting: urllib3==2.2.2 [compatible] (urllib3-2.2.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ca/1c/89ffc63a9605b583d5df2be791a27bc1a42b7c32bab68d3c8f2f73a98cd4/urllib3-2.2.2-py3-none-any.whl.metadata
DEBUG Found not-modified response for: <internal repo>/certifi/
DEBUG Searching for a compatible version of certifi (>=2017.4.17)
DEBUG Selecting: certifi==2024.7.4 [compatible] (certifi-2024.7.4-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1c/d5/c84e1a17bf61d4df64ca866a1c9a913874b4e9bdc131ec689a0ad013fb36/certifi-2024.7.4-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of llvmlite (>=0.35.0.dev0, <0.36)
DEBUG Selecting: llvmlite==0.35.0 [compatible] (llvmlite-0.35.0.tar.gz)
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==73.0.1 [compatible] (setuptools-73.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of mdurl (>=0.1, <1.dev0)
DEBUG Selecting: mdurl==0.1.2 [compatible] (mdurl-0.1.2-py3-none-any.whl)
DEBUG Tried 195 versions: numba 35, cython 16, numpy 16, opencv-python 16, requests 16, rich 16, typer 16, wand 16, wrapt 16, zstandard 16, bpy 1, certifi 1, charset-normalizer 1, click 1, my-package 1, idna 1, largestinteriorrectangle 1, llvmlite 1, markdown-it-py 1, mdurl 1, pillow 1, pygments 1, setuptools 1, shellingham 1, typing-extensions 1, urllib3 1
DEBUG Split specific environment resolution took 0.742s
Resolved 26 packages in 743ms
DEBUG Creating environment for tool `my-package`: /Users/taran/.local/share/uv/tools/my-package
DEBUG Using request timeout of 30s
DEBUG Must revalidate requirement: bpy
DEBUG Must revalidate requirement: certifi
DEBUG Must revalidate requirement: charset-normalizer
DEBUG Must revalidate requirement: click
DEBUG Must revalidate requirement: cython
DEBUG Must revalidate requirement: my-package
DEBUG Must revalidate requirement: idna
DEBUG Must revalidate requirement: largestinteriorrectangle
DEBUG Must revalidate requirement: llvmlite
DEBUG Must revalidate requirement: markdown-it-py
DEBUG Must revalidate requirement: mdurl
DEBUG Must revalidate requirement: numba
DEBUG Must revalidate requirement: numpy
DEBUG Must revalidate requirement: opencv-python
DEBUG Must revalidate requirement: pillow
DEBUG Must revalidate requirement: pygments
DEBUG Must revalidate requirement: requests
DEBUG Must revalidate requirement: rich
DEBUG Must revalidate requirement: setuptools
DEBUG Must revalidate requirement: shellingham
DEBUG Must revalidate requirement: typer
DEBUG Must revalidate requirement: typing-extensions
DEBUG Must revalidate requirement: urllib3
DEBUG Must revalidate requirement: wand
DEBUG Must revalidate requirement: wrapt
DEBUG Must revalidate requirement: zstandard
DEBUG Acquired lock for `/Users/taran/.cache/uv/built-wheels-v3/index/6dbd262e64c3fb74/numba/0.52.0rc2`
DEBUG Acquired lock for `/Users/taran/.cache/uv/built-wheels-v3/index/6dbd262e64c3fb74/llvmlite/0.35.0`
DEBUG No cache entry for: <internal repo>/packages/my-package-1.0.0a56-py3-none-any.whl#sha256=fd77ed2898c6153fc8b89851e6790f29c76fd77a429a93e117180f71a0afbea0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ea/5a/d085d433ad26687d7ba5be235c57679780a3b41316cd7ece1ee65694ebad/bpy-4.2.0-cp311-cp311-macosx_11_0_arm64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/66/82/564168a349148298aca281e342551404ef5521f33fba17b388ead0a84dc5/opencv_python-4.10.0.84-cp37-abi3-macosx_11_0_arm64.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f4/5f/491dafc7bbf5a3cc1845dc0430872e8096eb9e2b6f8161509d124594ec2d/pillow-10.4.0-cp311-cp311-macosx_11_0_arm64.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d5/43/4ff735420b31cd454e4b3acdd0ba7570b453aede6fa16cf7a11cc8780d1b/numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f7/3f/01c8b82017c199075f8f788d0d906b9ffbbc5a47dc9918a945e13d5a2bda/pygments-2.18.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/07/6a/0270e295bf30c37567736b7fca10167640898214ff911273af37ddb95770/setuptools-73.0.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e8/46/66d5b55f4d737dd6ab75851b224abf0afe5774976fe511a54d2eb9063a41/zstandard-0.23.0-cp311-cp311-macosx_11_0_arm64.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/43/39/bdbec9142bc46605b54d674bf158a78b191c2b75be527c6dcf3e6dfe90b8/Cython-3.0.11-py2.py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/87/67/a37f6214d0e9fe57f6ae54b2956d550ca8365857f42a1ce0392bb21d9410/rich-13.7.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1c/d5/c84e1a17bf61d4df64ca866a1c9a913874b4e9bdc131ec689a0ad013fb36/certifi-2024.7.4-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ca/1c/89ffc63a9605b583d5df2be791a27bc1a42b7c32bab68d3c8f2f73a98cd4/urllib3-2.2.2-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/59/d5/1bdd7c9662d5e9078e25ba0eb69bdb122859295746d40ab8dfef3a7b4d42/Wand-0.6.13-py2.py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/dd/51/68b61b90b24ca35495956b718f35a9756ef7d3dd4b3c1508056fa98d1a1b/charset_normalizer-3.3.2-cp311-cp311-macosx_11_0_arm64.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e5/3e/741d8c82801c347547f8a2a06aa57dbb1992be9e948df2ea0eda2c8b79e8/idna-3.7-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/00/2e/d53fa4befbf2cfa713304affc7ca780ce4fc1fd8710527771b58311a3229/click-8.1.7-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f9/9b/335f9764261e915ed497fcdeb11df5dfd6f7bf257d4a6a2a686d80da4d54/requests-2.32.3-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ae/cc/15083dcde1252a663398b1b2a173637a3ec65adadfb95137dc95df1e6adc/typer-0.12.4-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9a/06/d5cf3f16076e10debef57d4a29583a443117e07696a77fd33acb6ee24ede/largestinteriorrectangle-0.2.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0f/16/ea627d7817394db04518f62934a5de59874b587b792300991b3c347ff5e0/wrapt-1.16.0-cp311-cp311-macosx_11_0_arm64.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/3f/1c/21afa102d1f0df32be447a4dd7e2e54e670eb416f398ad449819570d2e52/numba-0.52.0rc2.tar.gz
DEBUG Building: numba==0.52.0rc2
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cd/3e/631087324bd3ffaf9a9c1c9d7d6675863e41afb31c818dd700019193e6b4/llvmlite-0.35.0.tar.gz
DEBUG Building: llvmlite==0.35.0
INFO Ignoring empty directory
DEBUG Solving with installed Python version: 3.11.9
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==73.0.1 [compatible] (setuptools-73.0.1-py3-none-any.whl)
DEBUG Tried 1 versions: setuptools 1
DEBUG Split specific environment resolution took 0.000s
INFO Ignoring empty directory
DEBUG Installing in setuptools==73.0.1 in /Users/taran/.cache/uv/builds-v0/.tmpJtg2wP
DEBUG Must revalidate requirement: setuptools
DEBUG Downloading and building requirement for build: setuptools==73.0.1
DEBUG Installing build requirement: setuptools==73.0.1
DEBUG Installing in setuptools==73.0.1 in /Users/taran/.cache/uv/builds-v0/.tmpe3CgfW
DEBUG Must revalidate requirement: setuptools
DEBUG Downloading and building requirement for build: setuptools==73.0.1
DEBUG Installing build requirement: setuptools==73.0.1
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.11.9
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Adding direct dependency: numpy>=1.11
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==73.0.1 [compatible] (setuptools-73.0.1-py3-none-any.whl)
DEBUG Searching for a compatible version of numpy (>=1.11)
DEBUG Selecting: numpy==2.1.0 [compatible] (numpy-2.1.0-cp311-cp311-macosx_14_0_arm64.whl)
DEBUG Tried 2 versions: numpy 1, setuptools 1
DEBUG Split specific environment resolution took 0.000s
DEBUG Installing in numpy==2.1.0, setuptools==73.0.1 in /Users/taran/.cache/uv/builds-v0/.tmpJtg2wP
DEBUG Must revalidate requirement: numpy
DEBUG Requirement already installed: setuptools==73.0.1
DEBUG Downloading and building requirement for build: numpy==2.1.0
DEBUG Installing build requirement: numpy==2.1.0
DEBUG Calling `setuptools.build_meta:__legacy__.build_wheel("/Users/taran/.cache/uv/built-wheels-v3/index/6dbd262e64c3fb74/numba/0.52.0rc2/WCC1sq2NkIA3pOBDuowdW/.tmpzvygKD", {}, None)`
DEBUG Calling `setuptools.build_meta:__legacy__.build_wheel("/Users/taran/.cache/uv/built-wheels-v3/index/6dbd262e64c3fb74/llvmlite/0.35.0/aQd_pIr8Ymw1uotL255dv/.tmpvaFDLw", {}, None)`
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: llvmlite==0.35.0
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:
running bdist_wheel
/Users/taran/.cache/uv/builds-v0/.tmpe3CgfW/bin/python /Users/taran/.cache/uv/built-wheels-v3/index/6dbd262e64c3fb74/llvmlite/0.35.0/aQd_pIr8Ymw1uotL255dv/llvmlite-0.35.0.tar.gz/ffi/build.py
LLVM version...
--- stderr:
Traceback (most recent call last):
  File "/Users/taran/.cache/uv/built-wheels-v3/index/6dbd262e64c3fb74/llvmlite/0.35.0/aQd_pIr8Ymw1uotL255dv/llvmlite-0.35.0.tar.gz/ffi/build.py", line 105, in main_posix
    out = subprocess.check_output([llvm_config, '--version'])
          ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/taran/.local/share/uv/python/cpython-3.11.9-macos-aarch64-none/lib/python3.11/subprocess.py", line 466, in check_output
    return run(*popenargs, stdout=PIPE, timeout=timeout, check=True,
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/taran/.local/share/uv/python/cpython-3.11.9-macos-aarch64-none/lib/python3.11/subprocess.py", line 548, in run
    with Popen(*popenargs, **kwargs) as process:
         ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/taran/.local/share/uv/python/cpython-3.11.9-macos-aarch64-none/lib/python3.11/subprocess.py", line 1026, in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
  File "/Users/taran/.local/share/uv/python/cpython-3.11.9-macos-aarch64-none/lib/python3.11/subprocess.py", line 1955, in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
FileNotFoundError: [Errno 2] No such file or directory: 'llvm-config'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/Users/taran/.cache/uv/built-wheels-v3/index/6dbd262e64c3fb74/llvmlite/0.35.0/aQd_pIr8Ymw1uotL255dv/llvmlite-0.35.0.tar.gz/ffi/build.py", line 191, in <module>
    main()
  File "/Users/taran/.cache/uv/built-wheels-v3/index/6dbd262e64c3fb74/llvmlite/0.35.0/aQd_pIr8Ymw1uotL255dv/llvmlite-0.35.0.tar.gz/ffi/build.py", line 185, in main
    main_posix('osx', '.dylib')
  File "/Users/taran/.cache/uv/built-wheels-v3/index/6dbd262e64c3fb74/llvmlite/0.35.0/aQd_pIr8Ymw1uotL255dv/llvmlite-0.35.0.tar.gz/ffi/build.py", line 107, in main_posix
    raise RuntimeError("%s failed executing, please point LLVM_CONFIG "
RuntimeError: llvm-config failed executing, please point LLVM_CONFIG to the path for llvm-config
error: command '/Users/taran/.cache/uv/builds-v0/.tmpe3CgfW/bin/python' failed with exit code 1
---
```

</details>

For some reason, `uv` is choosing that very old version of `llvmlite` as a dep for `numba`, but when I install via `pipx` it chooses a much more recent version: `0.43.0`.

---

_Comment by @joennlae on 2024-08-21 08:26_

I am not sure if this is an `uv` issue. I have this issue with v2.3.7 and v3.0. 

I was able to fix it by pinning the numba version to `numba>=0.60.0`

---

_Comment by @Garfield100 on 2024-08-21 09:07_

I have a similar problem to you @joennlae with the llvm version. I would prefer not installing this old llvm version and it would be impossible to get my whole student team to do it :sweat_smile: 
The llvmlite docs say to use binary wheels to install it: https://llvmlite.readthedocs.io/en/latest/admin-guide/install.html#
How does one specify this to uv?

**edit:** I believe `uv add --no-build-package llvmlite llvmlite` worked

---

_Comment by @taranlu-houzz on 2024-08-21 17:33_

@joennlae @charliermarsh Unless I am using `uv tool install` incorrectly, I think that it is incorrectly resolving to an old version, since I am able to correctly install via `pipx`.

`pipx` command that works: `pipx install my-package --pip-args='--index-url=http://<internal repo>:<port> --trusted-host=<internal repo> --pre' --python $(which python3.11)`

`uv` command that has an issue: `uv tool install --python 3.11 --python-preference only-managed --extra-index-url '<internal repo>' --prerelease allow --refresh --verbose my-package`

I guess one other difference is that I am using the `uv` managed version of Python vs. one that was installed via `mise`.


---

_Comment by @charliermarsh on 2024-08-21 21:34_

Does `llvmlite` exist on your internal index?

---

_Comment by @taranlu-houzz on 2024-08-21 21:40_

No, `llvmlite` is not on the internal index.

---

_Comment by @charliermarsh on 2024-08-22 01:54_

@taranlu-houzz -- Just to clarify, in your previous message, you used `my-package` -- is that `llvmlite`, or this is a separate issue?

---

_Comment by @taranlu-houzz on 2024-08-22 05:27_

I replaced the name of the actual package with `my-package` and it has a dependency (actually, dep of a dep) that needs `llvmlite`. Sorry for the confusion.

---

_Comment by @thisismygitrepo on 2024-08-24 03:57_

Same on `Windows`.  

```shell

Resolved 174 packages in 125ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: llvmlite==0.34.0
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit code: 1
--- stdout:
running bdist_wheel
C:\Users\alex\AppData\Local\uv\cache\builds-v0\.tmptz0R7c\Scripts\python.exe C:\Users\alex\AppData\Local\uv\cache\built-wheels-v3\pypi\llvmlite\0.34.0\aL2IMNnikQ99dItFJEjv2\llvmlite-0.34.0.tar.gz\ffi\build.py
Trying generator 'Visual Studio 15 2017 Win64'
--- stderr:
Traceback (most recent call last):
  File "C:\Users\alex\AppData\Local\uv\cache\built-wheels-v3\pypi\llvmlite\0.34.0\aL2IMNnikQ99dItFJEjv2\llvmlite-0.34.0.tar.gz\ffi\build.py", line 191, in <module>
    main()
  File "C:\Users\alex\AppData\Local\uv\cache\built-wheels-v3\pypi\llvmlite\0.34.0\aL2IMNnikQ99dItFJEjv2\llvmlite-0.34.0.tar.gz\ffi\build.py", line 179, in main
    main_win32()
  File "C:\Users\alex\AppData\Local\uv\cache\built-wheels-v3\pypi\llvmlite\0.34.0\aL2IMNnikQ99dItFJEjv2\llvmlite-0.34.0.tar.gz\ffi\build.py", line 88, in main_win32
    generator = find_win32_generator()
                ^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Users\alex\AppData\Local\uv\cache\built-wheels-v3\pypi\llvmlite\0.34.0\aL2IMNnikQ99dItFJEjv2\llvmlite-0.34.0.tar.gz\ffi\build.py", line 76, in find_win32_generator
    try_cmake(cmake_dir, build_dir, generator)
  File "C:\Users\alex\AppData\Local\uv\cache\built-wheels-v3\pypi\llvmlite\0.34.0\aL2IMNnikQ99dItFJEjv2\llvmlite-0.34.0.tar.gz\ffi\build.py", line 28, in try_cmake
    subprocess.check_call(['cmake', '-G', generator, cmake_dir])
  File "C:\Users\alex\AppData\Local\Programs\Python\Python311\Lib\subprocess.py", line 408, in check_call
    retcode = call(*popenargs, **kwargs)
              ^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Users\alex\AppData\Local\Programs\Python\Python311\Lib\subprocess.py", line 389, in call
    with Popen(*popenargs, **kwargs) as p:
         ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "C:\Users\alex\AppData\Local\Programs\Python\Python311\Lib\subprocess.py", line 1026, in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
  File "C:\Users\alex\AppData\Local\Programs\Python\Python311\Lib\subprocess.py", line 1538, in _execute_child
    hp, ht, pid, tid = _winapi.CreateProcess(executable, args,
                       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
FileNotFoundError: [WinError 2] The system cannot find the file specified
error: command 'C:\\Users\\alex\\AppData\\Local\\uv\\cache\\builds-v0\\.tmptz0R7c\\Scripts\\python.exe' failed with exit code 1

```

This is my first use of `uv` and the error gives me impression I'm doing something wrong because of that weird python.exe path provided. I thought I'm using my correct `vevnv` which has different path. 

---

_Comment by @derek-pyne on 2024-08-28 20:11_

Had this same behaviour where `uv` was trying to install an old version of `llvmlite` which was a dependency of one of my dependencies (in my case: umap-learn). This dependency doesn't need the old version of llvmlite.

These dependencies were resolvable with poetry (I'm migrating).

Tried to recreate with a smaller project without success

Solution was to explicitly add `llvmlite` as a dependency (ie. `uv add llvmlite`). This makes sure that it uses a recent version of `llvmlite` (>=0.43.0)

---

_Comment by @zanieb on 2024-08-28 20:13_

👍 @derek-pyne you can also use 

```
[tool.uv]
constraint-dependencies = ["llvm>0.43.0"]
```

to add a constraint without adding it as a dependency of your project.

---

_Comment by @derek-pyne on 2024-08-28 20:15_

ahhh, even better!

---

_Label `question` added by @zanieb on 2024-08-28 20:22_

---

_Comment by @taranlu-houzz on 2024-08-28 20:24_

@zanieb That is a cool feature, but I think this may still be a bug. Is there a way to apply your suggestion when using `uv install`? I'd rather not have to add a workaround on the package side of things since `pipx` is still able to correctly install the package with the correct (newer) version of `llvmlite`).

---

_Comment by @charliermarsh on 2024-08-28 20:28_

If you run `uv tool install numba` or similar, and we get an older version of `llvmlite`, that is not necessarily a bug. In fact it's probably not? My guess is that we just ended up with a different (but valid) resolution that pip. Most likely, some other package in the resolution was installed at a higher version than the corresponding package in pip.

---

_Comment by @taranlu-houzz on 2024-08-28 20:36_

@charliermarsh So, this is the dep tree (via `pipdeptree`) that is installed in the `pipx` created virtualenv:

```
├── largestinteriorrectangle [required: >=0.2.1, installed: 0.2.1]
│   └── numba [required: Any, installed: 0.60.0]
│       ├── llvmlite [required: >=0.43.0dev0,<0.44, installed: 0.43.0]
│       └── numpy [required: >=1.22,<2.1, installed: 2.0.1]
```

I would think that the version requirements of `numba` for `llvmlite` would make it invalid to try to install `0.35.0`, right?

---

_Comment by @taranlu-houzz on 2024-08-28 20:38_

Is it possible that `uv` is choosing to install an older version of `numba` since the dep requirement is `Any`, and perhaps that version relies on the old `llvmlite`?

---

_Comment by @charliermarsh on 2024-08-28 20:43_

Can you run uv with `--verbose`, and we can look at the decisions it's making and why?

---

_Comment by @taranlu-houzz on 2024-08-28 20:48_

Yes, that is what the output in this post is from: https://github.com/astral-sh/uv/issues/6281#issuecomment-2299964691

> Hmm, maybe I am doing something wrong? This is the full output of the actual command:
> 
> `uv tool install --python 3.11 --python-preference only-managed --extra-index-url '<internal repo>' --prerelease allow --refresh --verbose my-package`
> 
> Output
> For some reason, `uv` is choosing that very old version of `llvmlite` as a dep for `numba`, but when I install via `pipx` it chooses a much more recent version: `0.43.0`.

At the time, I think I was using either `uv` version `0.3.0` or `0.3.1`. Should I update and try again?

---

_Comment by @charliermarsh on 2024-08-28 20:58_

Thanks! It's roughly what I was describing above... With that set of dependencies, we end up solving for `numpy==2.1.0`, while `pip` ends up with `numpy==2.0.1`. If you trace the log closely, you'll see that we pick `numpy==2.1.0`, then try various `numba` versions until we hit `numba==0.52.0rc2`, which depends on `numpy>=1.15`. (All the later `numba` versions have an upper-bound on NumPy). Once we pick _that_, we have to pick an `llvmlite` version that meets `llvmlite>=0.35.0.dev0, <0.36`.

In the absence of more constraints from the user, both resolutions are valid. We can't really "know" that you would prefer a more recent `llvmlite` and `numba`, but a less recent `numpy`, unless you give us more information.

I might suggest adding `numpy>=2, <2.1` to your requirements? Or adding that as a constraint file.

---

_Comment by @joennlae on 2024-08-28 21:04_

My story:

So my python version is `3.11`.
I fixed `numpy<2.0`. Only the `numba` version above `numba>=0.57` can be installed in `python 3.11` (https://github.com/numba/numba/blob/288a38bbd5a15418a211bf067878dfdf3c139509/setup.py#L23) except for `numba==0.51` (which did not set an upper limit yet).

`Numba` for me is installed because of `librosa` there they fix `numba>=0.51` (https://github.com/librosa/librosa/blob/b2cd8fb803345685152a2b41e752ad09f33d261b/setup.cfg#L93)

So it jumped down to `numba=0.51`. I can not reproduce it anymore. So, for me, it is fixed now.


---

_Comment by @taranlu-houzz on 2024-08-28 21:21_

@charliermarsh Ah, okay, I think that makes sense. Can the constraints file be used directly with the `uv tool install` command for testing?

---

_Comment by @charliermarsh on 2024-09-08 18:23_

@taranlu-houzz -- You can pass the file like `uv tool install --with-requirements constraints.txt <tool>`.

---

_Comment by @taranlu-houzz on 2024-09-12 18:31_

@charliermarsh Sort of a follow-up to this: Is it possible to layer on addition deps to an already installed tool using the method you mentioned above? For example, I have installed `coverage` using `uv tool install coverage --with pytest --with pytest-spec --with pytest-sugar` so that I can run `uvx coverage run -m pytest` on any project.

For one of my projects, I have an additional dep on `matplotlib` in order to run the tests. I tried using `uv tool run --with-requirements constraints.txt coverage run -m pytest`, but it seems that it cannot find `pytest` in the generated venv.

---

_Comment by @charliermarsh on 2024-09-13 01:55_

I think `uv tool run --with-requirements constraints.txt coverage run -m pytest` should work. Could you share a sequence of commands I can use to reproduce?

---

_Comment by @taranlu-houzz on 2024-09-13 21:10_

@charliermarsh I'm guessing that it throws out the venv and creates a new one with the specified requirements and runs the tool using that (but that disregards the additional packages from the initial tool installation):
```
↓2 ~
❯ uv tool uninstall coverage
Uninstalled 2 executables: coverage, coverage3

↓2 ~
❯ uv tool run coverage run -m pytest --version
No module named 'pytest'

↓2 ~
❌ 1 ❯ uv tool install coverage --with pytest --with pytest-spec --with pytest-sugar
Resolved 8 packages in 4ms
Installed 8 packages in 17ms
 + coverage==7.6.1
 + iniconfig==2.0.0
 + packaging==24.1
 + pluggy==1.5.0
 + pytest==8.3.3
 + pytest-spec==4.0.0
 + pytest-sugar==1.0.0
 + termcolor==2.4.0
Installed 2 executables: coverage, coverage3

↓2 ~
❯ uv tool run coverage run -m pytest --version
pytest 8.3.3
/Users/taran/.local/share/uv/tools/coverage/lib/python3.12/site-packages/coverage/control.py:894: CoverageWarning: No data was collected. (no-data-collected)
  self._warn("No data was collected.", slug="no-data-collected")

↓2 ~
❯ echo "matplotlib>=3.9.1" > constraints.txt

↓2 ~
❯ touch test_uv_tool_run_with_requirements.py

↓2 ~ via 🐍 v3.12.1
❯ uv tool run --with-requirements constraints.txt coverage run -m pytest ./test_uv_tool_run_with_requirements.py
No module named 'pytest'

↓2 ~ via 🐍 v3.12.1
❌ 1 ❯ uv tool run coverage run -m pytest ./test_uv_tool_run_with_requirements.py
Test session starts (platform: darwin, Python 3.12.5, pytest 8.3.3, pytest-sugar 1.0.0)
rootdir: /Users/taran
plugins: sugar-1.0.0, spec-4.0.0
collecting ...
―――――――――――――――――――――――――――――――――――――――――――――――――――――――― ERROR collecting test_uv_tool_run_with_requirements.py ――――――――――――――――――――――――――――――――――――――――――――――――――――――――
ImportError while importing test module '/Users/taran/test_uv_tool_run_with_requirements.py'.
Hint: make sure your test modules/packages have valid Python names.
Traceback:
.local/share/uv/python/cpython-3.12.5-macos-aarch64-none/lib/python3.12/importlib/__init__.py:90: in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
test_uv_tool_run_with_requirements.py:1: in <module>
    import matplotlib.pyplot as plt
E   ModuleNotFoundError: No module named 'matplotlib'
collected 0 items / 1 error

======================================================================= short test summary info ========================================================================
FAILED test_uv_tool_run_with_requirements.py
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! Interrupted: 1 error during collection !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

Results (0.11s):

↓2 ~ via 🐍 v3.12.1
❌ 2 ❯ cat test_uv_tool_run_with_requirements.py
import matplotlib.pyplot as plt


def test_basic_plot():
    # Create a simple plot
    fig, ax = plt.subplots()
    ax.plot([1, 2, 3], [1, 4, 9])

    # Check if the figure and axes are created
    assert fig is not None
    assert ax is not None

↓2 ~ via 🐍 v3.12.1
❯ uv --version
uv 0.4.9 (77d278f68 2024-09-10)
```

---

_Comment by @charliermarsh on 2024-09-13 22:16_

Sorry, yes, I think that's right. We don't reuse an installed tool if that tool environment doesn't satisfy the request (i.e., `uv tool run` does not modify existing tools). It's possible that we could layer the `--with` requirements on top of the existing tools, though it would lead to some confusing interactions (i.e., any `--with` requirements from the already-installed tool would also be present).

---

_Comment by @taranlu-houzz on 2024-09-13 22:27_

Hmm, I don't know if it would really be better to allow for that added complexity... Maybe a different workflow would be better on my end. I was mainly just investigating some options for pulling more project dev deps into a single, unified location.

---

_Closed by @zanieb on 2024-10-21 21:19_

---

_Comment by @kym6464 on 2024-12-22 15:32_

I ran into this issue when trying to install `llvmlite` on Python 3.13 and the solution was to downgrade to Python 3.12. Potentially related issue https://github.com/numba/llvmlite/issues/1084

---
