```yaml
number: 7434
title: Cache is hitting race conditions when when sharing the cache directory across multiple running docker containers
type: issue
state: open
author: henryborchers
labels:
  - question
  - cache
assignees: []
created_at: 2024-09-16T17:13:55Z
updated_at: 2025-01-27T21:10:15Z
url: https://github.com/astral-sh/uv/issues/7434
synced_at: 2026-01-10T04:27:58Z
```

# Cache is hitting race conditions when when sharing the cache directory across multiple running docker containers

---

_Issue opened by @henryborchers on 2024-09-16 17:13_

While building wheels on my CI machine, I've been running into an issue involving the cache directory trying to access and write to the same files. This only happens when I have two or more containers running with the same mounted path, the location of UV_CACHE_DIR aka cache path.  This doesn't happen every time, just when uv is trying to access the same files in the cache. For this reason the ci pipeline could be fine depending on which stages are running at the same time. 

It looks like it's possibly a race condition that wasn't a problem when I was using pip. Is there a "process locking file" located outside of the UV_CACHE_DIR that I should also be mounting so that the cache isn't written to at the same time? 

uv==2.12
build==1.2.1

Note: In this example, I'm using Python 3.8 on Linux but it's a happens with all version in Linux and Windows Docker containers.

Docker Command used:

```docker run -ti -v uvcache:/.cache/uv -e UV_CACHE_DIR=/.cache/uv some_docker_image_that_has_uv_installed```

Uv command used inside the container:
```python3.8 -m build --wheel --installer=uv```

Output 
```
+ UV_INDEX_STRATEGY=unsafe-best-match
+ python3.8 -m build --wheel --installer=uv
* Creating isolated environment: venv+uv...
* Using external uv from /usr/local/bin/uv
* Installing packages in isolated environment:
  - conan>=1.50.0,<2.0
  - setuptools>=40.8.0
  - uiucprescon.build @
  git+https://github.com/UIUCLibrary/uiucprescon_build.git@v0.2.1
  - wheel
> /usr/local/bin/uv pip install setuptools>=40.8.0 wheel "uiucprescon.build @
  git+https://github.com/UIUCLibrary/uiucprescon_build.git@v0.2.1"
  conan>=1.50.0,<2.0
< error: failed to open file `/.cache/uv/CACHEDIR.TAG`
<   Caused by: Permission denied (os error 13)

Traceback (most recent call last):
  File "/opt/python/cp38-cp38/lib/python3.8/site-packages/build/__main__.py", line 178, in _handle_build_error
    yield
  File "/opt/python/cp38-cp38/lib/python3.8/site-packages/build/__main__.py", line 429, in main
    built = build_call(
  File "/opt/python/cp38-cp38/lib/python3.8/site-packages/build/__main__.py", line 238, in build_package
    out = _build(isolation, srcdir, outdir, distribution, config_settings, skip_dependency_check, installer)
  File "/opt/python/cp38-cp38/lib/python3.8/site-packages/build/__main__.py", line 170, in _build
    return _build_in_isolated_env(srcdir, outdir, distribution, config_settings, installer)
  File "/opt/python/cp38-cp38/lib/python3.8/site-packages/build/__main__.py", line 135, in _build_in_isolated_env
    env.install(builder.build_system_requires)
  File "/opt/python/cp38-cp38/lib/python3.8/site-packages/build/env.py", line 136, in install
    self._env_backend.install_requirements(requirements)
  File "/opt/python/cp38-cp38/lib/python3.8/site-packages/build/env.py", line 301, in install_requirements
    run_subprocess([*cmd, 'install', *requirements], env={**os.environ, 'VIRTUAL_ENV': self._env_path})
  File "/opt/python/cp38-cp38/lib/python3.8/site-packages/build/_ctx.py", line 71, in run_subprocess
    subprocess.run(cmd, capture_output=True, check=True, env=env)
  File "/opt/python/cp38-cp38/lib/python3.8/subprocess.py", line 516, in run
    raise CalledProcessError(retcode, process.args,
subprocess.CalledProcessError: Command '['/usr/local/bin/uv', 'pip', 'install', 'setuptools>=40.8.0', 'wheel', 'uiucprescon.build @ git+https://github.com/UIUCLibrary/uiucprescon_build.git@v0.2.1', 'conan>=1.50.0,<2.0']' returned non-zero exit status 2.

ERROR Command '['/usr/local/bin/uv', 'pip', 'install', 'setuptools>=40.8.0', 'wheel', 'uiucprescon.build @ git+https://github.com/UIUCLibrary/uiucprescon_build.git@v0.2.1', 'conan>=1.50.0,<2.0']' returned non-zero exit status 2.


```

---

_Comment by @charliermarsh on 2024-09-16 18:56_

We very intentionally do not lock the cache, it's designed to be concurrency safe. My guess is that error is something to do with the file being created by a different user...?


---

_Label `cache` added by @charliermarsh on 2024-09-16 21:27_

---

_Comment by @charliermarsh on 2024-09-16 21:28_

Are both containers running as the same user?

---

_Comment by @henryborchers on 2024-09-16 22:47_

They should be running as the same user. They are based on the same image.

---

_Comment by @henryborchers on 2024-09-17 19:18_

To give you a little more context, these Docker containers are spun up by Jenkins as part of the pipeline. When building a wheel with an extension, I build the wheel for each supported version of Python in their own Docker container (based on the same image that has all the supported versions of Python installed) running in parallel. When these containers run, they run with the exact same settings, same environment variables, same user, and same path to cache mounted on the host system. 

When the containers run one at a time, everything runs fine. It uses the cache without an issue.  However, when multiple containers are accessing the same cache directory, I get the permission errors.

---

_Comment by @zanieb on 2024-09-17 19:30_

Is the error always when trying to write the `CACHEDIR.TAG` or do you see this for other files too?

---

_Comment by @henryborchers on 2024-09-18 13:25_

Other files as well. If you want I can put the stack trace for those here too

---

_Comment by @henryborchers on 2024-09-19 13:08_

Actually, I think it might be only CACHEDIR.TAG that I'm running into. At least when using Linux.

---

_Comment by @zanieb on 2024-09-19 14:40_

Let's see if https://github.com/astral-sh/uv/pull/7550 resolves this then

---

_Comment by @charliermarsh on 2024-10-08 22:42_

Any luck after #7550?

---

_Label `question` added by @charliermarsh on 2024-10-08 22:42_

---

_Comment by @henryborchers on 2024-10-09 13:48_

I have had only limited attempts to try it due to other issues that pulled my attention away. I'm hoping to update the instances of uv on my pipelines this week though.

---

_Comment by @henryborchers on 2024-10-09 19:12_

It doesn't look like it fixed it.

uv 0.4.20
python 3.10
OS: Windows Server 2019
Command used: `tox run -v --workdir=./.tox -e py310`

```
error: Failed to write to the client cache
  Caused by: Failed to persist temporary file to c:\users\containeradministrator\appdata\local\uv\simple-v13\index\59a64b868d00c9da\packaging.rkyv: Access is denied. (os error 5)
py310: exit 2 (0.17 seconds) C:\jenkins-prod\workspace\closed_source_pykdu_compress_dev@7> C:\pipx\venvs\tox\Scripts\uv.exe pip install pytest>=6.0 -v pid=37856
  py310: FAIL code 2 (0.72 seconds)
  evaluation failed :( (1.31 seconds)

```

---

_Comment by @henryborchers on 2024-10-21 18:48_

This seems to be running into an issue on windows these days.

---

_Comment by @Slarag on 2025-01-23 08:46_

I'm seeing the same issue. I'm trying to switch some Gitlab CI pipelines from pip to uv. I have three jobs that run in parallel. The jobs use tox environments and they are running on the same Windows machine (Gitlab Shell Executor, no docker).

When all three jobs run in parallel one of them fails (see error message below) most of the time when running `uv pip install tox tox-uv`. So the error happens before this job starts tox. When I restart that job manually after the other two have finished, it runs as expected. Sometimes no error occurs and all three pass.

- Python 3.11.6
- uv==0.5.23
- tox==4.24.1
- tox-uv==1.20.1

.gitlab-ci.yml
```yaml
stages:
  - lint
  - docs

variables:
  PIP_INDEX_URL: "..."
  PIP_CACHE_DIR: "C:\\Cache\\pip"
  UV_DEFAULT_INDEX: "..."
  UV_CACHE_DIR: "C:\\cache\\uv"


before_script:
  - python -m venv _venv
  - .\_venv\Scripts\Activate.ps1
  - python --version
  - python -m pip install pip --upgrade
  - python -m pip install uv
  - uv pip install tox tox-uv

tc_format_check:
  stage: lint
  script: tox -e tc-format-check
  tags:
    - mytag

static_code_analysis:
  allow_failure: true
  stage: lint
  script: tox -e static-code-analysis
  tags:
    - mytag

typecheck:
  allow_failure: true
  stage: lint
  script: tox -e types
  tags:
    - mytag   
```

tox.ini
```ini
[tox]
minversion = 3.18
envlist = codestyle, docstyle, errors, types
isolated_build = True
toxworkdir = _tox

[testenv]
; install_command = pip install --cache-dir c:\cache\pip
extras = test
passenv =
    CI
    USERNAME
    GIT_*
deps:
    sphinx
    myst-parser
    sphinx-multiversion
commands =
    test-py3{7,8,9,10, 11}: pytest {posargs:tests}

[testenv:static-code-analysis]
description = Check code and tests for PEP 8 compliance and code complexity.
skip_install = true
envdir = {toxworkdir}/static-code-analysis
deps =
    ruff
commands =
    ruff check {posargs:src/ tests/}

[testenv:tc-format-check]
description = Check test functions.
skip_install = true
envdir = {toxworkdir}/pylint
deps =
    pylint >= 3.0.2
commands =
    pylint {posargs: tests/}

[testenv:types]
description = Run static type checker.
envdir = {toxworkdir}/lint
deps =
    mypy
    types-requests
commands =
    mypy --check-untyped-defs --no-implicit-optional --follow-imports=skip --disallow-untyped-calls --ignore-missing-imports src\
```

error message
```
$ python -m pip install uv
...
Installing collected packages: uv
Successfully installed uv-0.5.23
$ uv pip install tox tox-uv
Using Python 3.11.6 environment at: _venv
error: Failed to write to the client cache
  Caused by: Failed to retrieve temporary file while trying to persist to C:\cache\uv\wheels-v3\index\cfa3261714f6a532\tox-uv\tox_uv-1.20.1-py3-none-any.msgpack
Cleaning up project directory and file based variables
ERROR: Job failed: exit status 2
```

---

_Comment by @henryborchers on 2025-01-23 14:50_

@Slarag thank you for confirming this. I was starting to think I was the only person experiencing this and/or I was taking crazy pills.

---

_Comment by @konstin on 2025-01-24 16:18_

Can you share the owner and permissions on the file (just like a `ls -lah`) and the user ids inside the containers?

---

_Comment by @zanieb on 2025-01-24 20:48_

I think it's basically expected that we can't perform proper synchronization across containers? I don't think the file locks can be properly held. 

---

_Comment by @Slarag on 2025-01-25 08:23_

@zanieb  In my case I'm not using containers at all. The Gitlab shell executor just runs different powershell processes on the same machine. And the jobs just access the same cache directory on that machine.

I'm not shure how the shell executor fully works internally, but there's no isolation or so. The jobs can in theory access each others files and have all the access rights of the assigned Windows user. So it's just like three simulaneous processes running uv.

> Generally it’s unsafe to run jobs with shell executors. The jobs are run with the user’s permissions (gitlab-runner) and can “steal” code from other projects that are run on this server. Depending on your configuration, the job could execute arbitrary commands on the server as a highly privileged user. Use it only for running builds from users you trust on a server you trust and own. 

https://docs.gitlab.com/runner/executors/shell.html

Could it be a problem that it's different uv executables acessing the same cache (uv installed in each separate venv by `pip install uv`)?

---

_Comment by @jan11011977 on 2025-01-27 13:42_

Hello,

We are seeing the same issue as well.

We are not using Docker containers. Simply running multiple instances of UV with a clean cache will repro the issue. The problem happens when two processes decide to build the same package at the same time. One process will end up reading while another is writing. On Linux this works, but on Windows due to the way file locking works, this causes issues.

Someone on my team actually spent a day investigating this issue but it appeared to be nontrivial to fix.

---

_Comment by @charliermarsh on 2025-01-27 18:36_

I just reproduced the "Failed to retrieve temporary file" thing on my Windows machine by building in parallel with setuptools -- is that what you're seeing @jan11011977?

---

_Comment by @charliermarsh on 2025-01-27 21:10_

I'm going to track this in https://github.com/astral-sh/uv/issues/11002 since the Windows issues are unrelated to the original report here (Docker).

---
