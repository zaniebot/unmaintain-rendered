```yaml
number: 2955
title: "Install of \"youtokentome\" package failed."
type: issue
state: closed
author: AstralScribe
labels:
  - question
assignees: []
created_at: 2024-04-10T05:54:56Z
updated_at: 2024-04-10T13:51:48Z
url: https://github.com/astral-sh/uv/issues/2955
synced_at: 2026-01-12T15:58:41Z
```

# Install of "youtokentome" package failed.

---

_@AstralScribe_

Venv install done using:
```
uv venv
Using Python 3.11.8 interpreter at: /usr/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```
First command I tried:
```
 uv pip install youtokentome --verbose
```
Error:
```
INFO Found a virtualenv through VIRTUAL_ENV at: /home/mayank/Documents/dev-hub/sandbox/test/.venv
DEBUG Cached interpreter info for Python 3.11.8, skipping probing: .venv/bin/python
DEBUG Using Python 3.11.8 environment at .venv/bin/python
DEBUG Trying to lock if free: .venv/.lock
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.11.8
DEBUG Adding direct dependency: youtokentome*
TRACE Fetching metadata for youtokentome from https://pypi.org/simple/youtokentome/
TRACE cached request https://pypi.org/simple/youtokentome/ is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://pypi.org/simple/youtokentome/
TRACE Received package metadata for: youtokentome
TRACE selecting candidate for package PackageName("youtokentome") with range Range { segments: [(Unbounded, Unbounded)] } with 8 remote versions
TRACE found candidate for package PackageName("youtokentome") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "1.0.6" version
DEBUG Searching for a compatible version of youtokentome (*)
TRACE selecting candidate for package PackageName("youtokentome") with range Range { segments: [(Unbounded, Unbounded)] } with 8 remote versions
TRACE found candidate for package PackageName("youtokentome") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "1.0.6" version
DEBUG Selecting: youtokentome==1.0.6 (youtokentome-1.0.6.tar.gz)
TRACE cached request https://files.pythonhosted.org/packages/9c/78/95ea7cd878a50c905584c8a8aea3a6e0a59d7bbd10329cb03849d34674e2/youtokentome-1.0.6-cp35-cp35m-macosx_10_14_intel.whl.metadata is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9c/78/95ea7cd878a50c905584c8a8aea3a6e0a59d7bbd10329cb03849d34674e2/youtokentome-1.0.6-cp35-cp35m-macosx_10_14_intel.whl.metadata
TRACE Received built distribution metadata for: youtokentome==1.0.6
DEBUG Adding transitive dependency: click>=7.0
TRACE Fetching metadata for click from https://pypi.org/simple/click/
TRACE cached request https://pypi.org/simple/click/ is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://pypi.org/simple/click/
TRACE Received package metadata for: click
TRACE selecting candidate for package PackageName("click") with range Range { segments: [(Included("7.0"), Unbounded)] } with 53 remote versions
TRACE found candidate for package PackageName("click") with range Range { segments: [(Included("7.0"), Unbounded)] } after 1 steps: "8.1.7" version
DEBUG Searching for a compatible version of click (>=7.0)
TRACE selecting candidate for package PackageName("click") with range Range { segments: [(Included("7.0"), Unbounded)] } with 53 remote versions
TRACE found candidate for package PackageName("click") with range Range { segments: [(Included("7.0"), Unbounded)] } after 1 steps: "8.1.7" version
DEBUG Selecting: click==8.1.7 (click-8.1.7-py3-none-any.whl)
TRACE cached request https://files.pythonhosted.org/packages/00/2e/d53fa4befbf2cfa713304affc7ca780ce4fc1fd8710527771b58311a3229/click-8.1.7-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/00/2e/d53fa4befbf2cfa713304affc7ca780ce4fc1fd8710527771b58311a3229/click-8.1.7-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: click==8.1.7
Resolved 2 packages in 1ms
DEBUG Requirement already cached: click==8.1.7
DEBUG Identified uncached requirement: youtokentome==1.0.6
TRACE cached request https://files.pythonhosted.org/packages/9a/ae/f8b0d15696766eb35dda6cf84a23d42ae7f3ba37aa30e5e2287fd94ac053/youtokentome-1.0.6.tar.gz is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9a/ae/f8b0d15696766eb35dda6cf84a23d42ae7f3ba37aa30e5e2287fd94ac053/youtokentome-1.0.6.tar.gz
DEBUG Building: youtokentome==1.0.6
DEBUG Solving with target Python version 3.11.8
DEBUG Adding direct dependency: setuptools>=40.8.0
TRACE Fetching metadata for setuptools from https://pypi.org/simple/setuptools/
TRACE cached request https://pypi.org/simple/setuptools/ is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
TRACE Received package metadata for: setuptools
TRACE selecting candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } with 550 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } after 1 steps: "69.2.0" version
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
TRACE selecting candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } with 550 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } after 1 steps: "69.2.0" version
DEBUG Selecting: setuptools==69.2.0 (setuptools-69.2.0-py3-none-any.whl)
TRACE cached request https://files.pythonhosted.org/packages/92/e1/1c8bb3420105e70bdf357d57dd5567202b4ef8d27f810e98bb962d950834/setuptools-69.2.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/92/e1/1c8bb3420105e70bdf357d57dd5567202b4ef8d27f810e98bb962d950834/setuptools-69.2.0-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: setuptools==69.2.0
DEBUG Installing in setuptools==69.2.0 in /home/mayank/.cache/uv/.tmpfYZLHq/.venv
DEBUG Requirement already cached: setuptools==69.2.0
DEBUG Installing build requirement: setuptools==69.2.0
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
error: Failed to download distributions
  Caused by: Failed to fetch wheel: youtokentome==1.0.6
  Caused by: Failed to build: youtokentome==1.0.6
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/home/mayank/.cache/uv/.tmpfYZLHq/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=['wheel'])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/mayank/.cache/uv/.tmpfYZLHq/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
    self.run_setup()
  File "/home/mayank/.cache/uv/.tmpfYZLHq/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 487, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/home/mayank/.cache/uv/.tmpfYZLHq/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 5, in <module>
ModuleNotFoundError: No module named 'Cython'
---
```
Next, I tried installing wheel and Cython
```
uv pip install wheel Cython --verbose
```
```
INFO Found a virtualenv through VIRTUAL_ENV at: /home/mayank/Documents/dev-hub/sandbox/test/.venv
DEBUG Cached interpreter info for Python 3.11.8, skipping probing: .venv/bin/python
DEBUG Using Python 3.11.8 environment at .venv/bin/python
DEBUG Trying to lock if free: .venv/.lock
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.11.8
DEBUG Adding direct dependency: wheel*
DEBUG Adding direct dependency: cython*
TRACE Fetching metadata for wheel from https://pypi.org/simple/wheel/
TRACE Fetching metadata for cython from https://pypi.org/simple/cython/
TRACE cached request https://pypi.org/simple/wheel/ is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://pypi.org/simple/wheel/
TRACE Received package metadata for: wheel
TRACE selecting candidate for package PackageName("wheel") with range Range { segments: [(Unbounded, Unbounded)] } with 75 remote versions
TRACE found candidate for package PackageName("wheel") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "0.43.0" version
DEBUG Searching for a compatible version of wheel (*)
TRACE selecting candidate for package PackageName("wheel") with range Range { segments: [(Unbounded, Unbounded)] } with 75 remote versions
TRACE found candidate for package PackageName("wheel") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "0.43.0" version
DEBUG Selecting: wheel==0.43.0 (wheel-0.43.0-py3-none-any.whl)
TRACE cached request https://files.pythonhosted.org/packages/7d/cd/d7460c9a869b16c3dd4e1e403cce337df165368c71d6af229a74699622ce/wheel-0.43.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7d/cd/d7460c9a869b16c3dd4e1e403cce337df165368c71d6af229a74699622ce/wheel-0.43.0-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: wheel==0.43.0
TRACE cached request https://pypi.org/simple/cython/ is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://pypi.org/simple/cython/
TRACE Received package metadata for: cython
TRACE selecting candidate for package PackageName("cython") with range Range { segments: [(Unbounded, Unbounded)] } with 138 remote versions
TRACE found candidate for package PackageName("cython") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "3.0.10" version
DEBUG Searching for a compatible version of cython (*)
TRACE selecting candidate for package PackageName("cython") with range Range { segments: [(Unbounded, Unbounded)] } with 138 remote versions
TRACE found candidate for package PackageName("cython") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "3.0.10" version
DEBUG Selecting: cython==3.0.10 (Cython-3.0.10-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
TRACE cached request https://files.pythonhosted.org/packages/45/82/077c13035d6f45d8b8b74d67e9f73f2bfc54ef8d1f79572790f6f7d2b4f5/Cython-3.0.10-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/45/82/077c13035d6f45d8b8b74d67e9f73f2bfc54ef8d1f79572790f6f7d2b4f5/Cython-3.0.10-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
TRACE Received built distribution metadata for: cython==3.0.10
Resolved 2 packages in 16ms
DEBUG Requirement already cached: cython==3.0.10
DEBUG Requirement already cached: wheel==0.43.0
Installed 2 packages in 16ms
 + cython==3.0.10
 + wheel==0.43.0
```

Retried with the same command:
```
 uv pip install youtokentome --verbose
```
Again the error:
```
INFO Found a virtualenv through VIRTUAL_ENV at: /home/mayank/Documents/dev-hub/sandbox/test/.venv
DEBUG Cached interpreter info for Python 3.11.8, skipping probing: .venv/bin/python
DEBUG Using Python 3.11.8 environment at .venv/bin/python
DEBUG Trying to lock if free: .venv/.lock
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.11.8
DEBUG Adding direct dependency: youtokentome*
TRACE Fetching metadata for youtokentome from https://pypi.org/simple/youtokentome/
TRACE cached request https://pypi.org/simple/youtokentome/ is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://pypi.org/simple/youtokentome/
TRACE Received package metadata for: youtokentome
TRACE selecting candidate for package PackageName("youtokentome") with range Range { segments: [(Unbounded, Unbounded)] } with 8 remote versions
TRACE found candidate for package PackageName("youtokentome") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "1.0.6" version
DEBUG Searching for a compatible version of youtokentome (*)
TRACE selecting candidate for package PackageName("youtokentome") with range Range { segments: [(Unbounded, Unbounded)] } with 8 remote versions
TRACE found candidate for package PackageName("youtokentome") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "1.0.6" version
DEBUG Selecting: youtokentome==1.0.6 (youtokentome-1.0.6.tar.gz)
TRACE cached request https://files.pythonhosted.org/packages/9c/78/95ea7cd878a50c905584c8a8aea3a6e0a59d7bbd10329cb03849d34674e2/youtokentome-1.0.6-cp35-cp35m-macosx_10_14_intel.whl.metadata is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9c/78/95ea7cd878a50c905584c8a8aea3a6e0a59d7bbd10329cb03849d34674e2/youtokentome-1.0.6-cp35-cp35m-macosx_10_14_intel.whl.metadata
TRACE Received built distribution metadata for: youtokentome==1.0.6
DEBUG Adding transitive dependency: click>=7.0
TRACE Fetching metadata for click from https://pypi.org/simple/click/
TRACE cached request https://pypi.org/simple/click/ is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://pypi.org/simple/click/
TRACE Received package metadata for: click
TRACE selecting candidate for package PackageName("click") with range Range { segments: [(Included("7.0"), Unbounded)] } with 53 remote versions
TRACE found candidate for package PackageName("click") with range Range { segments: [(Included("7.0"), Unbounded)] } after 1 steps: "8.1.7" version
DEBUG Searching for a compatible version of click (>=7.0)
TRACE selecting candidate for package PackageName("click") with range Range { segments: [(Included("7.0"), Unbounded)] } with 53 remote versions
TRACE found candidate for package PackageName("click") with range Range { segments: [(Included("7.0"), Unbounded)] } after 1 steps: "8.1.7" version
DEBUG Selecting: click==8.1.7 (click-8.1.7-py3-none-any.whl)
TRACE cached request https://files.pythonhosted.org/packages/00/2e/d53fa4befbf2cfa713304affc7ca780ce4fc1fd8710527771b58311a3229/click-8.1.7-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/00/2e/d53fa4befbf2cfa713304affc7ca780ce4fc1fd8710527771b58311a3229/click-8.1.7-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: click==8.1.7
Resolved 2 packages in 1ms
DEBUG Requirement already cached: click==8.1.7
DEBUG Identified uncached requirement: youtokentome==1.0.6
DEBUG Unnecessary package: cython==3.0.10
DEBUG Unnecessary package: wheel==0.43.0
TRACE cached request https://files.pythonhosted.org/packages/9a/ae/f8b0d15696766eb35dda6cf84a23d42ae7f3ba37aa30e5e2287fd94ac053/youtokentome-1.0.6.tar.gz is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/9a/ae/f8b0d15696766eb35dda6cf84a23d42ae7f3ba37aa30e5e2287fd94ac053/youtokentome-1.0.6.tar.gz
DEBUG Building: youtokentome==1.0.6
DEBUG Solving with target Python version 3.11.8
DEBUG Adding direct dependency: setuptools>=40.8.0
TRACE Fetching metadata for setuptools from https://pypi.org/simple/setuptools/
TRACE cached request https://pypi.org/simple/setuptools/ is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
TRACE Received package metadata for: setuptools
TRACE selecting candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } with 550 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } after 1 steps: "69.2.0" version
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
TRACE selecting candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } with 550 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } after 1 steps: "69.2.0" version
DEBUG Selecting: setuptools==69.2.0 (setuptools-69.2.0-py3-none-any.whl)
TRACE cached request https://files.pythonhosted.org/packages/92/e1/1c8bb3420105e70bdf357d57dd5567202b4ef8d27f810e98bb962d950834/setuptools-69.2.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/92/e1/1c8bb3420105e70bdf357d57dd5567202b4ef8d27f810e98bb962d950834/setuptools-69.2.0-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: setuptools==69.2.0
DEBUG Installing in setuptools==69.2.0 in /home/mayank/.cache/uv/.tmpchWICp/.venv
DEBUG Requirement already cached: setuptools==69.2.0
DEBUG Installing build requirement: setuptools==69.2.0
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
error: Failed to download distributions
  Caused by: Failed to fetch wheel: youtokentome==1.0.6
  Caused by: Failed to build: youtokentome==1.0.6
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/home/mayank/.cache/uv/.tmpchWICp/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=['wheel'])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/mayank/.cache/uv/.tmpchWICp/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
    self.run_setup()
  File "/home/mayank/.cache/uv/.tmpchWICp/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 487, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/home/mayank/.cache/uv/.tmpchWICp/.venv/lib/python3.11/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 5, in <module>
ModuleNotFoundError: No module named 'Cython'
---

```

The similar situation using venv and pip normally
```
❯ python -m venv venv
❯ source venv/bin/activate
❯ pip install wheel cython
Collecting wheel
  Using cached wheel-0.43.0-py3-none-any.whl.metadata (2.2 kB)
Collecting cython
  Using cached Cython-3.0.10-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (3.2 kB)
Using cached wheel-0.43.0-py3-none-any.whl (65 kB)
Using cached Cython-3.0.10-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (3.6 MB)
Installing collected packages: wheel, cython
Successfully installed cython-3.0.10 wheel-0.43.0
❯ pip install youtokentome
Collecting youtokentome
  Using cached youtokentome-1.0.6-cp311-cp311-linux_x86_64.whl
Collecting Click>=7.0 (from youtokentome)
  Using cached click-8.1.7-py3-none-any.whl.metadata (3.0 kB)
Using cached click-8.1.7-py3-none-any.whl (97 kB)
Installing collected packages: Click, youtokentome
Successfully installed Click-8.1.7 youtokentome-1.0.6
```

Other info:
OS: Arch (linux-lts 6.6.25-1-lts)
UV version: uv 0.1.29

---

_Comment by @charliermarsh on 2024-04-10 13:48_

Can you retry with `--no-build-isolation`? E.g., ` uv pip install youtokentome --no-build-isolation` (after installing Cython in your environment).

---

_Comment by @charliermarsh on 2024-04-10 13:49_

`pip` also fails with this package under `--use-pep517`, which will eventually be the default, so it's a problem with the package unfortunately:

```
❯ pip install youtokentome --use-pep517
Collecting youtokentome
  Downloading youtokentome-1.0.6.tar.gz (86 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 86.7/86.7 kB 3.5 MB/s eta 0:00:00
  Installing build dependencies ... done
  Getting requirements to build wheel ... error
  error: subprocess-exited-with-error

  × Getting requirements to build wheel did not run successfully.
  │ exit code: 1
  ╰─> [20 lines of output]
      Traceback (most recent call last):
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.2/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.2/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.2/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 118, in get_requires_for_build_wheel
          return hook(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-xop9lly4/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=['wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-xop9lly4/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
          self.run_setup()
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-xop9lly4/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 487, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-xop9lly4/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 311, in run_setup
          exec(code, locals())
        File "<string>", line 5, in <module>
      ModuleNotFoundError: No module named 'Cython'
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
error: subprocess-exited-with-error

× Getting requirements to build wheel did not run successfully.
│ exit code: 1
╰─> See above for output.

note: This error originates from a subprocess, and is likely not a problem with pip.
```

---

_Label `question` added by @charliermarsh on 2024-04-10 13:50_

---

_Comment by @charliermarsh on 2024-04-10 13:51_

Added it to the list of broken packages: https://github.com/astral-sh/uv/issues/2252

---

_Closed by @charliermarsh on 2024-04-10 13:51_

---
