---
number: 13433
title: Failed to build inflate64
type: issue
state: closed
author: NewAtair
labels:
  - question
assignees: []
created_at: 2025-05-13T16:38:01Z
updated_at: 2025-10-19T12:22:04Z
url: https://github.com/astral-sh/uv/issues/13433
synced_at: 2026-01-07T13:12:18-06:00
---

# Failed to build inflate64

---

_Issue opened by @NewAtair on 2025-05-13 16:38_

### Summary

Hello all,

uv fails to build 

`uv add -v py7zr`

Detailed log: [https://gist.github.com/NewAtair/c307de9fbe2f571233ced47f1ce02e08.js](url)

Last lines of the log:

> DEBUG Reverting changes to `uv.lock`
  x Failed to build `inflate64==1.0.1`
  |-> The build backend returned an error
  `-> Call to `setuptools.build_meta.build_wheel` failed (exit code: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_py
      copying src\inflate64\__init__.py -> build\lib.win-amd64-cpython-313\inflate64
      running egg_info
      writing src\inflate64.egg-info\PKG-INFO
      writing dependency_links to src\inflate64.egg-info\dependency_links.txt
      writing requirements to src\inflate64.egg-info\requires.txt
      writing top-level names to src\inflate64.egg-info\top_level.txt
      writing manifest file 'src\inflate64.egg-info\SOURCES.txt'
      reading manifest file 'src\inflate64.egg-info\SOURCES.txt'
      reading manifest template 'MANIFEST.in'
      adding license file 'COPYING'
      writing manifest file 'src\inflate64.egg-info\SOURCES.txt'
      copying src\inflate64\py.typed -> build\lib.win-amd64-cpython-313\inflate64
      running build_ext
      building 'inflate64._inflate64' extension

      [stderr]
      C:\Users\username\AppData\Local\uv\cache\builds-v0\.tmp9ZApHf\Lib\site-packages\setuptools_scm\git.py:310: UserWarning: git archive did not support describe output
        warnings.warn("git archive did not support describe output")
      C:\Users\username\AppData\Local\uv\cache\builds-v0\.tmp9ZApHf\Lib\site-packages\setuptools_scm\git.py:328: UserWarning: unprocessed git archival found (no export subst applied)
        warnings.warn("unprocessed git archival found (no export subst applied)")
      C:\Users\username\AppData\Local\uv\cache\builds-v0\.tmp9ZApHf\Lib\site-packages\setuptools\config\_apply_pyprojecttoml.py:82: SetuptoolsDeprecationWarning: `project.license` as
      a TOML table is deprecated
      !!

              ********************************************************************************
              Please use a simple string containing a SPDX expression for `project.license`. You can also use `project.license-files`. (Both options available on setuptools>=77.0.0).

              By 2026-Feb-18, you need to update your project and remove deprecated calls
              or your builds will no longer be supported.

              See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
              ********************************************************************************

      !!
        corresp(dist, value, root_dir)
      C:\Users\username\AppData\Local\uv\cache\builds-v0\.tmp9ZApHf\Lib\site-packages\setuptools\config\_apply_pyprojecttoml.py:61: SetuptoolsDeprecationWarning: License classifiers
      are deprecated.
      !!

              ********************************************************************************
              Please consider removing the following classifiers in favor of a SPDX license expression:

              License :: OSI Approved :: GNU Lesser General Public License v2 or later (LGPLv2+)

              See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
              ********************************************************************************

      !!
        dist._finalize_license_expression()
      C:\Users\username\AppData\Local\uv\cache\builds-v0\.tmp9ZApHf\Lib\site-packages\setuptools\dist.py:761: SetuptoolsDeprecationWarning: License classifiers are deprecated.
      !!

              ********************************************************************************
              Please consider removing the following classifiers in favor of a SPDX license expression:

              License :: OSI Approved :: GNU Lesser General Public License v2 or later (LGPLv2+)

              See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
              ********************************************************************************

      !!
        self._finalize_license_expression()
      ERROR setuptools_scm._file_finders.git listing git files failed - pretending there aren't any
      warning: no previously-included files found matching '.gitignore'
      warning: no previously-included files found matching '.woodpecker.yml'
      no previously-included directories found matching 'ci'
      no previously-included directories found matching 'ISSUE_TEMPLATE'
      error: Microsoft Visual C++ 14.0 or greater is required. Get it with "Microsoft C++ Build Tools": https://visualstudio.microsoft.com/visual-cpp-build-tools/

hint: This usually indicates a problem with the package or the build environment.
help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
PS C:\Tools\Scripte\Python allgemein>

If  run with

`pip install --use-pep517 --no-cache --force-reinstall py7zr`

than it is successful with:

`Successfully installed brotli-1.1.0 inflate64-1.0.1 multivolumefile-0.2.3 psutil-7.0.0 py7zr-0.22.0 pybcj-1.0.6 pycryptodomex-3.22.0 pyppmd-1.1.1 pyzstd-0.17.0 texttable-1.7.0 typing-extensions-4.13.2`

If I  install py7zr with conda, there are no issues too.

So I'm a bit puzzled.

### Platform

Windows 11 Version 23H2

### Version

uv  0.7.3 (3c413f74b 2025-05-07)

### Python version

Python 3.13.3

---

_Label `bug` added by @NewAtair on 2025-05-13 16:38_

---

_Comment by @konstin on 2025-05-13 17:02_

Does pip use the same Python version as uv, and does pip also show building inflate64?

---

_Comment by @NewAtair on 2025-05-13 19:56_

Hi,

according to:

`PS C:\> pip --version
pip 25.1.1 from C:\Users\username\AppData\Local\Programs\Python\Python312\Lib\site-packages\pip (python 3.12)`

and here ist the log from

`PS C:\> pip install --use-pep517 --no-cache --force-reinstall --verbose py7zr`

[https://gist.github.com/NewAtair/032810460f135775a4f0a8e8c5532208.js](url)

So pip ist downloading and not building inflate64.

With Python 3.12 it is working:

`PS C:\TEMP> mkdir test
PS C:\TEMP> cd test
PS C:\TEMP\test> uv venv --python 3.12 --seed
PS C:\TEMP\test> .venv\Scripts\activate
PS C:\TEMP\test> uv pip install --verbose py7zr
`

See Log: [https://gist.github.com/NewAtair/6b9fd9519f59d497d04f711eedab8e75](url)

This is very interesting, because with conda and Python 3.13.3 I don't have those issues.

Kind regard


---

_Comment by @konstin on 2025-05-13 21:03_

Conda is a different ecosystem that uses its own prebuilt packages from a different build service. They may have Python 3.13 conda packages, but on PyPI, there are only wheels up to 3.12.

To use PyPI with uv/pip, you either need to downgrade to an older Python version (below 3.13) or follow the guidance in the error message and configure your machine for building the package:

> error: Microsoft Visual C++ 14.0 or greater is required. Get it with "Microsoft C++ Build Tools": https://visualstudio.microsoft.com/visual-cpp-build-tools/

---

_Label `bug` removed by @konstin on 2025-05-13 21:03_

---

_Label `question` added by @konstin on 2025-05-13 21:03_

---

_Comment by @Ravencentric on 2025-05-13 22:01_

Upstream issue: https://github.com/miurahr/py7zr/issues/640

---

_Comment by @NewAtair on 2025-05-14 13:31_

General question (for a newbie): Wehen the update will be available? 
To install Microsoft Visual C++ 14.0 is not a feasible solution. 

Anyway, thank you for the clarification.

---

_Comment by @konstin on 2025-05-14 13:46_

This is not a bug in uv, as the error message says, this is about for which platforms and Python versions inflate64 has wheel and how inflate64's build system works.

---

_Comment by @NewAtair on 2025-05-14 14:02_

Maybe I'm was not clear enough before. I know that ist not a bug within uv so I have to wait for the updates further upstream for the updates of py7zr, inflate64, and ect.

---

_Closed by @konstin on 2025-10-19 12:22_

---
