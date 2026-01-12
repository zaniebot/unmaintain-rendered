```yaml
number: 7436
title: "uv sync: show long path errors on Windows"
type: issue
state: open
author: bes-alielzein
labels:
  - windows
assignees: []
created_at: 2024-09-16T18:26:53Z
updated_at: 2024-09-16T20:04:35Z
url: https://github.com/astral-sh/uv/issues/7436
synced_at: 2026-01-12T15:59:13Z
```

# uv sync: show long path errors on Windows

---

_@bes-alielzein_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

When long paths are not enabled on Windows, `uv sync` fails with an unclear error. However, when using pip, the error message is revealing the actual issue (path is too long).

Reference to the config: https://github.com/BesLogic/releaf-canopeum/blob/bcb64747ba6a28f5c5c3c6072ee2bd7ce1445ca9/canopeum_backend/pyproject.toml

UV version: 0.4.10

`uv sync --locked --extra dev`
```
Using Python 3.12.3 interpreter at: C:\Users\AliElzein\.pyenv\pyenv-win\versions\3.12.3\python3.12.exe
Creating virtualenv at: .venv
Resolved 56 packages in 1ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: djangorestframework-camel-case==1.4.2
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit code: 1
--- stdout:
running bdist_wheel
running build
running build_py
copying djangorestframework_camel_case\middleware.py -> build\lib\djangorestframework_camel_case
copying djangorestframework_camel_case\parser.py -> build\lib\djangorestframework_camel_case
copying djangorestframework_camel_case\render.py -> build\lib\djangorestframework_camel_case
copying djangorestframework_camel_case\settings.py -> build\lib\djangorestframework_camel_case
copying djangorestframework_camel_case\util.py -> build\lib\djangorestframework_camel_case
copying djangorestframework_camel_case\__init__.py -> build\lib\djangorestframework_camel_case
running egg_info
writing djangorestframework_camel_case.egg-info\PKG-INFO
writing dependency_links to djangorestframework_camel_case.egg-info\dependency_links.txt
writing top-level names to djangorestframework_camel_case.egg-info\top_level.txt
reading manifest file 'djangorestframework_camel_case.egg-info\SOURCES.txt'
reading manifest template 'MANIFEST.in'
adding license file 'LICENSE'
adding license file 'AUTHORS.rst'
writing manifest file 'djangorestframework_camel_case.egg-info\SOURCES.txt'
installing to build\bdist.win-amd64\wheel
running install
running install_lib
running install_egg_info
Copying djangorestframework_camel_case.egg-info to build\bdist.win-amd64\wheel\.\djangorestframework_camel_case-1.4.2-py3.12.egg-info
--- stderr:
C:\Users\AliElzein\AppData\Local\uv\cache\builds-v0\.tmpu9Mmm2\Lib\site-packages\setuptools\_distutils\dist.py:261: UserWarning: Unknown distribution option: 'test_suite'
  warnings.warn(msg)
error: [WinError 3] Le chemin d’accès spécifié est introuvable: 'build\\bdist.win-amd64\\wheel\\.\\djangorestframework_camel_case-1.4.2-py3.12.egg-info'
---
```
The error is in french and translates to: "The system cannot find the path specified".

However, when running the following command:

`uv pip install -e .[dev]`

```
Resolved 56 packages in 1.60s
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: djangorestframework-camel-case==1.4.2
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit code: 1
--- stdout:
running bdist_wheel
running build
running build_py
creating build\lib\djangorestframework_camel_case
copying djangorestframework_camel_case\middleware.py -> build\lib\djangorestframework_camel_case
copying djangorestframework_camel_case\parser.py -> build\lib\djangorestframework_camel_case
copying djangorestframework_camel_case\render.py -> build\lib\djangorestframework_camel_case
copying djangorestframework_camel_case\settings.py -> build\lib\djangorestframework_camel_case
copying djangorestframework_camel_case\util.py -> build\lib\djangorestframework_camel_case
copying djangorestframework_camel_case\__init__.py -> build\lib\djangorestframework_camel_case
running egg_info
writing djangorestframework_camel_case.egg-info\PKG-INFO
writing dependency_links to djangorestframework_camel_case.egg-info\dependency_links.txt
writing top-level names to djangorestframework_camel_case.egg-info\top_level.txt
reading manifest file 'djangorestframework_camel_case.egg-info\SOURCES.txt'
reading manifest template 'MANIFEST.in'
adding license file 'LICENSE'
adding license file 'AUTHORS.rst'
writing manifest file 'djangorestframework_camel_case.egg-info\SOURCES.txt'
installing to build\bdist.win-amd64\wheel
running install
running install_lib
creating build\bdist.win-amd64\wheel
creating build\bdist.win-amd64\wheel\djangorestframework_camel_case
copying build\lib\djangorestframework_camel_case\middleware.py -> build\bdist.win-amd64\wheel\.\djangorestframework_camel_case
copying build\lib\djangorestframework_camel_case\parser.py -> build\bdist.win-amd64\wheel\.\djangorestframework_camel_case
copying build\lib\djangorestframework_camel_case\render.py -> build\bdist.win-amd64\wheel\.\djangorestframework_camel_case
copying build\lib\djangorestframework_camel_case\settings.py -> build\bdist.win-amd64\wheel\.\djangorestframework_camel_case
copying build\lib\djangorestframework_camel_case\util.py -> build\bdist.win-amd64\wheel\.\djangorestframework_camel_case
copying build\lib\djangorestframework_camel_case\__init__.py -> build\bdist.win-amd64\wheel\.\djangorestframework_camel_case
running install_egg_info
Copying djangorestframework_camel_case.egg-info to build\bdist.win-amd64\wheel\.\djangorestframework_camel_case-1.4.2-py3.12.egg-info
--- stderr:
C:\Users\AliElzein\AppData\Local\uv\cache\builds-v0\.tmp8tpGbo\Lib\site-packages\setuptools\_distutils\dist.py:261: UserWarning: Unknown distribution option: 'test_suite'
  warnings.warn(msg)
error: [WinError 206] Nom de fichier ou extension trop long: 'build\\bdist.win-amd64\\wheel\\.\\djangorestframework_camel_case-1.4.2-py3.12.egg-info'
---
```

The error message is different. The error message translates to: "The filename or extension is too long".

---

_Comment by @charliermarsh on 2024-09-16 18:52_

That's surprising to me -- I don't think we have any control over the error message there, it comes directly from the build backend.

---

_Label `windows` added by @charliermarsh on 2024-09-16 18:53_

---

_Comment by @charliermarsh on 2024-09-16 18:54_

Oh wait, these are both uv. I thought the second trace was `pip install`. That's even more surprising to me then hah.

---

_Comment by @charliermarsh on 2024-09-16 18:54_

Is it the same Python version in both cases?

---

_Comment by @SamuelT-Beslogic on 2024-09-16 19:39_

> Is it the same Python version in both cases?

I would think so? When we got the logs for this, we run `uv pip install` *immediatly* after `uv sync`, so it should be using the same venv. The global Python install is also 3.12.3

Also of note, running standard `pip` directly in the venv created by the failed `uv sync` command worked, (probably that the path is just short enough w/o uv's cache being used)

`djangorestframework-camel-case` is probably only relevant here in that it's one of our longest-named dependency, just enough to trigger the issue.

---

Wild guess (I don't know too much about uv's internals), looks like the sync command may accidentally suppress the "path too long" error, then failing because the egg-info failed to be created.

---

Also for any future reader, the solution is: https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation#registry-setting-to-enable-long-paths
This issue is simply about showing a more helpful error message when using `uv sync`

---
