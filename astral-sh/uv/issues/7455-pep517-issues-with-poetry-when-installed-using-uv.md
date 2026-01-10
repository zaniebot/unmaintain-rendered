---
number: 7455
title: "PEP517 issues with poetry when installed using `uv tool`"
type: issue
state: closed
author: mil-ad
labels:
  - needs-mre
assignees: []
created_at: 2024-09-17T12:39:14Z
updated_at: 2024-09-19T09:10:00Z
url: https://github.com/astral-sh/uv/issues/7455
synced_at: 2026-01-10T01:24:15Z
---

# PEP517 issues with poetry when installed using `uv tool`

---

_Issue opened by @mil-ad on 2024-09-17 12:39_

I installed `poetry` via `uv tool install poetry` and it mostly works well. However when I do `poetry self add "keyrings.google-artifactregistry-auth@latest"` I get the error trace below.

When I install the same version of poetry with `pipx` it works fine.

```
Skipping wheel rapidfuzz-3.5.2-pp39-pypy39_pp73-win_amd64.whl as this is not supported by the current environment
[virtualenv] find interpreter for spec PythonSpec(path=/home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/bin/python3.11)
[virtualenv] filesystem is case-sensitive
[filelock:filelock] Attempting to acquire lock 139740252152144 on /home/milad_cohere_com/.local/share/virtualenv/py_info/1/4e6340e03a55d43873df94f39e6b12c9babf5b3e77835a7c3e4d41c2726985ee.lock
[filelock:filelock] Lock 139740252152144 acquired on /home/milad_cohere_com/.local/share/virtualenv/py_info/1/4e6340e03a55d43873df94f39e6b12c9babf5b3e77835a7c3e4d41c2726985ee.lock
[virtualenv] got python info of %s from (PosixPath('/home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/bin/python3.11'), PosixPath('/home/milad_cohere_com/.local/share/virtualenv/py_info/1/4e6340e03a55d43873df94f39e6b12c9babf5b3e77835a7c3e4d41c2726985ee.json'))
[filelock:filelock] Attempting to release lock 139740252152144 on /home/milad_cohere_com/.local/share/virtualenv/py_info/1/4e6340e03a55d43873df94f39e6b12c9babf5b3e77835a7c3e4d41c2726985ee.lock
[filelock:filelock] Lock 139740252152144 released on /home/milad_cohere_com/.local/share/virtualenv/py_info/1/4e6340e03a55d43873df94f39e6b12c9babf5b3e77835a7c3e4d41c2726985ee.lock
[virtualenv] proposed PythonInfo(spec=CPython3.11.7.final.0-64, exe=/home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/bin/python3.11, platform=linux, version='3.11.7 (main, Jan  8 2024, 05:55:31) [Clang 17.0.6 ]', encoding_fs_io=utf-8-utf-8)
[virtualenv] accepted PythonInfo(spec=CPython3.11.7.final.0-64, exe=/home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/bin/python3.11, platform=linux, version='3.11.7 (main, Jan  8 2024, 05:55:31) [Clang 17.0.6 ]', encoding_fs_io=utf-8-utf-8)
[virtualenv] create virtual environment via CPython3Posix(dest=/tmp/tmpy7vr6roe/.venv, clear=False, no_vcs_ignore=False, global=False)
[virtualenv] create folder /tmp/tmpy7vr6roe/.venv/bin
[virtualenv] create folder /tmp/tmpy7vr6roe/.venv/lib/python3.11/site-packages
[virtualenv] write /tmp/tmpy7vr6roe/.venv/pyvenv.cfg
[virtualenv] 	home = /home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/bin
[virtualenv] 	implementation = CPython
[virtualenv] 	version_info = 3.11.7.final.0
[virtualenv] 	virtualenv = 20.26.4
[virtualenv] 	include-system-site-packages = false
[virtualenv] 	base-prefix = /home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu
[virtualenv] 	base-exec-prefix = /home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu
[virtualenv] 	base-executable = /home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/bin/python3.11
[virtualenv] symlink /home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/bin/python3.11 to /tmp/tmpy7vr6roe/.venv/bin/python
[virtualenv] create virtualenv import hook file /tmp/tmpy7vr6roe/.venv/lib/python3.11/site-packages/_virtualenv.pth
[virtualenv] create /tmp/tmpy7vr6roe/.venv/lib/python3.11/site-packages/_virtualenv.py
[virtualenv] ============================== target debug ==============================
[virtualenv] debug via /tmp/tmpy7vr6roe/.venv/bin/python /home/milad_cohere_com/.local/share/uv/tools/poetry/lib/python3.11/site-packages/virtualenv/create/debug.py
[virtualenv] {
[virtualenv]   "sys": {
[virtualenv]     "executable": "/tmp/tmpy7vr6roe/.venv/bin/python",
[virtualenv]     "_base_executable": "/home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/bin/python3.11",
[virtualenv]     "prefix": "/tmp/tmpy7vr6roe/.venv",
[virtualenv]     "base_prefix": "/home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu",
[virtualenv]     "real_prefix": null,
[virtualenv]     "exec_prefix": "/tmp/tmpy7vr6roe/.venv",
[virtualenv]     "base_exec_prefix": "/home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu",
[virtualenv]     "path": [
[virtualenv]       "/home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/lib/python311.zip",
[virtualenv]       "/home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/lib/python3.11",
[virtualenv]       "/home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/lib/python3.11/lib-dynload",
[virtualenv]       "/tmp/tmpy7vr6roe/.venv/lib/python3.11/site-packages"
[virtualenv]     ],
[virtualenv]     "meta_path": [
[virtualenv]       "<class '_virtualenv._Finder'>",
[virtualenv]       "<class '_frozen_importlib.BuiltinImporter'>",
[virtualenv]       "<class '_frozen_importlib.FrozenImporter'>",
[virtualenv]       "<class '_frozen_importlib_external.PathFinder'>"
[virtualenv]     ],
[virtualenv]     "fs_encoding": "utf-8",
[virtualenv]     "io_encoding": "utf-8"
[virtualenv]   },
[virtualenv]   "version": "3.11.7 (main, Jan  8 2024, 05:55:31) [Clang 17.0.6 ]",
[virtualenv]   "makefile_filename": "/home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/lib/python3.11/config-3.11-x86_64-linux-gnu/Makefile",
[virtualenv]   "os": "<module 'os' (frozen)>",
[virtualenv]   "site": "<module 'site' (frozen)>",
[virtualenv]   "datetime": "<module 'datetime' from '/home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/lib/python3.11/datetime.py'>",
[virtualenv]   "math": "<module 'math' (built-in)>",
[virtualenv]   "json": "<module 'json' from '/home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/lib/python3.11/json/__init__.py'>"
[virtualenv] }
[virtualenv] add activators for Bash, CShell, Fish, Nushell, PowerShell, Python
[virtualenv] write /tmp/tmpy7vr6roe/.venv/pyvenv.cfg
[virtualenv] 	home = /home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/bin
[virtualenv] 	implementation = CPython
[virtualenv] 	version_info = 3.11.7.final.0
[virtualenv] 	virtualenv = 20.26.4
[virtualenv] 	include-system-site-packages = false
[virtualenv] 	base-prefix = /home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu
[virtualenv] 	base-exec-prefix = /home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu
[virtualenv] 	base-executable = /home/milad_cohere_com/.local/share/uv/python/cpython-3.11.7-linux-x86_64-gnu/bin/python3.11
[urllib3:urllib3.connectionpool] https://pypi.org:443 "GET /simple/setuptools/ HTTP/11" 304 0
[filelock:filelock] Attempting to acquire lock 139739990316304 on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/0/4/1/8/c/0418c83b80f7f7bfaec2738bfbbee53d2c1562196c0781702f6eddc8.lock
[filelock:filelock] Lock 139739990316304 acquired on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/0/4/1/8/c/0418c83b80f7f7bfaec2738bfbbee53d2c1562196c0781702f6eddc8.lock
[filelock:filelock] Attempting to release lock 139739990316304 on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/0/4/1/8/c/0418c83b80f7f7bfaec2738bfbbee53d2c1562196c0781702f6eddc8.lock
[filelock:filelock] Lock 139739990316304 released on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/0/4/1/8/c/0418c83b80f7f7bfaec2738bfbbee53d2c1562196c0781702f6eddc8.lock
Source (PyPI): 296 packages found for setuptools >=35.0.2
[urllib3:urllib3.connectionpool] https://pypi.org:443 "GET /simple/cython/ HTTP/11" 304 0
[filelock:filelock] Attempting to acquire lock 139739987016208 on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/a/2/6/7/f/a267f54c19667a20e6416d2dc2a1df4a525eed802f0f2b538b654da1.lock
[filelock:filelock] Lock 139739987016208 acquired on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/a/2/6/7/f/a267f54c19667a20e6416d2dc2a1df4a525eed802f0f2b538b654da1.lock
[filelock:filelock] Attempting to release lock 139739987016208 on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/a/2/6/7/f/a267f54c19667a20e6416d2dc2a1df4a525eed802f0f2b538b654da1.lock
[filelock:filelock] Lock 139739987016208 released on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/a/2/6/7/f/a267f54c19667a20e6416d2dc2a1df4a525eed802f0f2b538b654da1.lock
Source (PyPI): 7 packages found for cython >=0.29.30,<0.30.0
Source (PyPI): 1 packages found for setuptools >=35.0.2
Source (PyPI): 1 packages found for cython >=0.29.30,<0.30.0
[urllib3:urllib3.connectionpool] https://pypi.org:443 "GET /pypi/setuptools/75.1.0/json HTTP/11" 304 0
[filelock:filelock] Attempting to acquire lock 139739984884112 on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/e/2/c/7/8/e2c788554d4e34de515d9f4462de5195d169dd5dfbe5af86b575ab6c.lock
[filelock:filelock] Lock 139739984884112 acquired on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/e/2/c/7/8/e2c788554d4e34de515d9f4462de5195d169dd5dfbe5af86b575ab6c.lock
[filelock:filelock] Attempting to release lock 139739984884112 on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/e/2/c/7/8/e2c788554d4e34de515d9f4462de5195d169dd5dfbe5af86b575ab6c.lock
[filelock:filelock] Lock 139739984884112 released on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/e/2/c/7/8/e2c788554d4e34de515d9f4462de5195d169dd5dfbe5af86b575ab6c.lock
[urllib3:urllib3.connectionpool] https://pypi.org:443 "GET /pypi/cython/0.29.37/json HTTP/11" 304 0
[filelock:filelock] Attempting to acquire lock 139739984955152 on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/f/1/5/2/3/f1523df9719831089707d8802b3cedaec716689bc7e2495e90994870.lock
[filelock:filelock] Lock 139739984955152 acquired on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/f/1/5/2/3/f1523df9719831089707d8802b3cedaec716689bc7e2495e90994870.lock
[filelock:filelock] Attempting to release lock 139739984955152 on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/f/1/5/2/3/f1523df9719831089707d8802b3cedaec716689bc7e2495e90994870.lock
[filelock:filelock] Lock 139739984955152 released on /home/milad_cohere_com/.cache/pypoetry/cache/repositories/PyPI/_http/f/1/5/2/3/f1523df9719831089707d8802b3cedaec716689bc7e2495e90994870.lock
Skipping wheel Cython-0.29.37-cp27-cp27m-manylinux_2_5_i686.manylinux1_i686.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp27-cp27m-manylinux_2_5_x86_64.manylinux1_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp27-cp27mu-manylinux_2_5_i686.manylinux1_i686.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp27-cp27mu-manylinux_2_5_x86_64.manylinux1_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.manylinux_2_24_aarch64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_24_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp310-cp310-manylinux_2_5_i686.manylinux1_i686.manylinux_2_24_i686.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp310-cp310-musllinux_1_1_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.manylinux_2_24_aarch64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp311-cp311-manylinux_2_5_i686.manylinux1_i686.manylinux_2_24_i686.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp311-cp311-musllinux_1_1_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_28_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp312-cp312-musllinux_1_1_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp35-cp35m-manylinux_2_5_i686.manylinux1_i686.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp35-cp35m-manylinux_2_5_x86_64.manylinux1_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp36-cp36m-manylinux_2_17_aarch64.manylinux2014_aarch64.manylinux_2_24_aarch64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp36-cp36m-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_24_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp36-cp36m-manylinux_2_5_i686.manylinux1_i686.manylinux_2_24_i686.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp36-cp36m-manylinux_2_5_i686.manylinux1_i686.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp36-cp36m-manylinux_2_5_x86_64.manylinux1_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp36-cp36m-musllinux_1_1_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp37-cp37m-manylinux_2_17_aarch64.manylinux2014_aarch64.manylinux_2_24_aarch64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp37-cp37m-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_24_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp37-cp37m-manylinux_2_5_i686.manylinux1_i686.manylinux_2_24_i686.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp37-cp37m-manylinux_2_5_i686.manylinux1_i686.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp37-cp37m-manylinux_2_5_x86_64.manylinux1_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp37-cp37m-musllinux_1_1_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp38-cp38-manylinux_2_17_aarch64.manylinux2014_aarch64.manylinux_2_24_aarch64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp38-cp38-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_24_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp38-cp38-manylinux_2_5_i686.manylinux1_i686.manylinux_2_24_i686.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp38-cp38-manylinux_2_5_i686.manylinux1_i686.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp38-cp38-manylinux_2_5_x86_64.manylinux1_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp38-cp38-musllinux_1_1_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp39-cp39-manylinux_2_17_aarch64.manylinux2014_aarch64.manylinux_2_24_aarch64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.manylinux_2_24_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp39-cp39-manylinux_2_5_i686.manylinux1_i686.manylinux_2_24_i686.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp39-cp39-manylinux_2_5_i686.manylinux1_i686.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp39-cp39-manylinux_2_5_x86_64.manylinux1_x86_64.whl as this is not supported by the current environment
Skipping wheel Cython-0.29.37-cp39-cp39-musllinux_1_1_x86_64.whl as this is not supported by the current environment
[build:build] Getting build dependencies for wheel...
Source (PyPI): 296 packages found for setuptools >=35.0.2
Source (PyPI): 7 packages found for cython >=0.29.30,<0.30.0
Source (PyPI): 1 packages found for setuptools >=35.0.2
Source (PyPI): 1 packages found for cython >=0.29.30,<0.30.0
[build:build] Building wheel...

  Stack trace:

  9  ~/.local/share/uv/tools/poetry/lib/python3.11/site-packages/poetry/installation/executor.py:285 in _execute_operation
      283│ 
      284│             try:
    → 285│                 result = self._do_execute_operation(operation)
      286│             except EnvCommandError as e:
      287│                 if e.e.returncode == -2:

  8  ~/.local/share/uv/tools/poetry/lib/python3.11/site-packages/poetry/installation/executor.py:395 in _do_execute_operation
      393│             return 0
      394│ 
    → 395│         result: int = getattr(self, f"_execute_{method}")(operation)
      396│ 
      397│         if result != 0:

  7  ~/.local/share/uv/tools/poetry/lib/python3.11/site-packages/poetry/installation/executor.py:527 in _execute_update
      525│ 
      526│     def _execute_update(self, operation: Install | Update) -> int:
    → 527│         status_code = self._update(operation)
      528│ 
      529│         self._save_url_reference(operation)

  6  ~/.local/share/uv/tools/poetry/lib/python3.11/site-packages/poetry/installation/executor.py:586 in _update
      584│ 
      585│     def _update(self, operation: Install | Update) -> int:
    → 586│         return self._install(operation)
      587│ 
      588│     def _remove(self, package: Package) -> int:

  5  ~/.local/share/uv/tools/poetry/lib/python3.11/site-packages/poetry/installation/executor.py:558 in _install
      556│             archive = self._download_link(operation, Link(package.source_url))
      557│         else:
    → 558│             archive = self._download(operation)
      559│ 
      560│         operation_message = self.get_operation_message(operation)

  4  ~/.local/share/uv/tools/poetry/lib/python3.11/site-packages/poetry/installation/executor.py:750 in _download
      748│             self._yanked_warnings.append(message)
      749│ 
    → 750│         return self._download_link(operation, link)
      751│ 
      752│     def _download_link(self, operation: Install | Update, link: Link) -> Path:

  3  ~/.local/share/uv/tools/poetry/lib/python3.11/site-packages/poetry/installation/executor.py:785 in _download_link
      783│             self._write(operation, message)
      784│ 
    → 785│             archive = self._chef.prepare(archive, output_dir=original_archive.parent)
      786│ 
      787│         # Use the original archive to provide the correct hash.

  2  ~/.local/share/uv/tools/poetry/lib/python3.11/site-packages/poetry/installation/chef.py:123 in prepare
      121│             return self._prepare(archive, destination=destination, editable=editable)
      122│ 
    → 123│         return self._prepare_sdist(archive, destination=output_dir)
      124│ 
      125│     def _prepare(

  1  ~/.local/share/uv/tools/poetry/lib/python3.11/site-packages/poetry/installation/chef.py:194 in _prepare_sdist
      192│             destination.mkdir(parents=True, exist_ok=True)
      193│ 
    → 194│             return self._prepare(
      195│                 sdist_dir,
      196│                 destination,

  ChefBuildError

  Backend subprocess exited when trying to invoke build_wheel
  
  Traceback (most recent call last):
    File "/home/milad_cohere_com/.local/share/uv/tools/poetry/lib/python3.11/site-packages/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
      main()
    File "/home/milad_cohere_com/.local/share/uv/tools/poetry/lib/python3.11/site-packages/pyproject_hooks/_in_process/_in_process.py", line 335, in main
      json_out['return_val'] = hook(**hook_input['kwargs'])
                               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
    File "/home/milad_cohere_com/.local/share/uv/tools/poetry/lib/python3.11/site-packages/pyproject_hooks/_in_process/_in_process.py", line 251, in build_wheel
      return _build_backend().build_wheel(wheel_directory, config_settings,
             ^^^^^^^^^^^^^^^^
    File "/home/milad_cohere_com/.local/share/uv/tools/poetry/lib/python3.11/site-packages/pyproject_hooks/_in_process/_in_process.py", line 74, in _build_backend
      ep = os.environ['PEP517_BUILD_BACKEND']
           ~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^
    File "<frozen os>", line 679, in __getitem__
  KeyError: 'PEP517_BUILD_BACKEND'
  

  at ~/.local/share/uv/tools/poetry/lib/python3.11/site-packages/poetry/installation/chef.py:164 in _prepare
      160│ 
      161│                 error = ChefBuildError("\n\n".join(message_parts))
      162│ 
      163│             if error is not None:
    → 164│                 raise error from None
      165│ 
      166│             return path
      167│ 
      168│     def _prepare_sdist(self, archive: Path, destination: Path | None = None) -> Path:

Note: This error originates from the build backend, and is likely not a problem with poetry but with msgpack (1.0.4) not supporting PEP 517 builds. You can verify this by running 'pip wheel --no-cache-dir --use-pep517 "msgpack (==1.0.4)"'.
```


<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @charliermarsh on 2024-09-17 13:20_

Unfortunately this works fine for me even when forcing source distribution builds for `msgpack==1.0.4`... The trace is honestly a bit strange as that's roughly a bug in the bridge between Poetry and the source distribution build.

Do you have the same Poetry version between uv and pipx? My guess is that there's some version incompatibility between your Poetry install and other dependencies, as in https://github.com/python-poetry/poetry/issues/9351.

---

_Label `needs-mre` added by @charliermarsh on 2024-09-17 13:20_

---

_Comment by @mil-ad on 2024-09-17 14:44_

> Do you have the same Poetry version between uv and pipx?

Yes just confirmed that

> My guess is that there's some version incompatibility between your Poetry install and other dependencies

I don't need `poetry install` to trigger it. All I do is:

```
$ uv tool install poetry
$ poetry -vvv self add "keyrings.google-artifactregistry-auth@latest"
```

---

_Comment by @mil-ad on 2024-09-17 14:47_

perhaps related to the python version that pipx uses vs uv? I played around with `uv tool`'s `--python-preference` but didn't make a difference

---

_Comment by @charliermarsh on 2024-09-17 14:47_

Can you include the `-vvv` output? I really think this is just a Poetry issue somewhere, we have effectively no control over what's happening here, but I'm happy to take a look.

---

_Comment by @mil-ad on 2024-09-17 14:49_

> I really think this is just a Poetry issue somewhere

agreed, I often find myself deleting poetry's caches to fix random issues. Let me try a clean install.

> Can you include the -vvv output?

what I shared was with `-vvv` but just the tail.

---

_Comment by @bmarroquin on 2024-09-18 05:35_

I believe the `self` command is only recommended when using the poetry installer.

Try:
`uv tool install poetry --with keyrings.google-artifactregistry-auth`

---

_Comment by @charliermarsh on 2024-09-18 15:07_

Oh yeah, that makes sense. I'd recommend using `--with` here.

---

_Closed by @charliermarsh on 2024-09-18 15:07_

---

_Comment by @mil-ad on 2024-09-19 09:09_

Yeah - Can confirm that works!

---
