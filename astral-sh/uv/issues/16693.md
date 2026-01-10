```yaml
number: 16693
title: uv build does not warn or fail when license files do not exist
type: issue
state: closed
author: N-Wouda
labels:
  - bug
assignees: []
created_at: 2025-11-11T21:12:56Z
updated_at: 2025-11-14T10:02:05Z
url: https://github.com/astral-sh/uv/issues/16693
synced_at: 2026-01-10T03:23:55Z
```

# uv build does not warn or fail when license files do not exist

---

_Issue opened by @N-Wouda on 2025-11-11 21:12_

### Summary

`uv build` does not inform the user that a license file specified in the `license-files` field could not be found. This has unintended consequences, since after building the package, its `.dist-info` directory contains an empty `licenses/` directory.

I understand the [packaging guide](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license-files) entry about `license-files` as requiring each glob (including literal globs) to match at least one file:
> * Each glob must match at least one file.

If no matches are found that seems to me misspecified, and worth pointing out to the user.

The current implementation allows the build to complete, which presents its own problems. If the built package is then installed in another project, `uv` cannot also remove it fully (it does not remove the actually empty `.dist-info/licenses/` directory that is supposed to contain the license files), leaving the managed `.venv` in a broken state. But this follow-up issue is caused by the previous problem, so perhaps addressing the root problem suffices.

###  Steps to reproduce:

1. Run
```
uv init --package uvtest
```

2. Add
```
license-files = ["abc"]
```
to the `[project]`  table.

3. Run
```
uv build -vvv
```
This does not raise or warn that the license file `abc` could not be found. Output:
<details>

```
DEBUG uv 0.9.8
TRACE Checking shared lock for `/home/niels/.cache/uv` at `/home/niels/.cache/uv/.lock`
DEBUG Acquired shared lock for `/home/niels/.cache/uv`
DEBUG Found workspace root: `/home/niels/projects/uvtest`
TRACE Discovering workspace members for: `/home/niels/projects/uvtest`
DEBUG Adding root workspace member: `/home/niels/projects/uvtest`
DEBUG Reading Python requests from version file at `/home/niels/projects/uvtest/.python-version`
DEBUG Searching for Python 3.10 in virtual environments, managed installations, or search path
TRACE Found cached interpreter info for Python 3.10.12, skipping query of: .venv/bin/python3
DEBUG Found `cpython-3.10.12-linux-x86_64-gnu` at `/home/niels/projects/uvtest/.venv/bin/python3` (virtual environment)
DEBUG Using request timeout of 30s
Building source distribution (uv build backend)...
DEBUG Found PEP 639 license declarations, using METADATA 2.4
DEBUG built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE Not a file in license files match: .
DEBUG Adding content files to source distribution
DEBUG Source root: src
DEBUG Module path: uvtest
TRACE Including readme at: README.md
TRACE Including license files at: abc`
DEBUG Source distribution includes: ["pyproject.toml", "src/uvtest/**", "README.md", "abc"]
DEBUG built glob set; 3 literals, 0 basenames, 0 extensions, 1 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG Source dist excludes: ["__pycache__", "*.pyc", "*.pyo"]
DEBUG built glob set; 0 literals, 1 basenames, 2 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG Adding to sdist: .
DEBUG Adding to sdist: README.md
DEBUG Adding to sdist: pyproject.toml
DEBUG Adding to sdist: src
DEBUG Adding to sdist: src/uvtest
DEBUG Adding to sdist: src/uvtest/__init__.py
DEBUG Visited 6 files for source dist build
Building wheel from source distribution (uv build backend)...
DEBUG Writing wheel at dist/uvtest-0.1.0-py3-none-any.whl
DEBUG Wheel excludes: ["__pycache__", "*.pyc", "*.pyo"]
DEBUG built glob set; 0 literals, 1 basenames, 2 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG Adding content files to wheel
DEBUG Source root: /home/niels/.cache/uv/sdists-v9/.tmpTE2im7/uvtest-0.1.0/src
DEBUG Module path: uvtest
DEBUG Adding to wheel: uvtest
TRACE Adding directory uvtest
DEBUG Adding to wheel: uvtest/__init__.py
TRACE Adding uvtest/__init__.py from /home/niels/.cache/uv/sdists-v9/.tmpTE2im7/uvtest-0.1.0/src/uvtest/__init__.py
DEBUG Visited 2 files for wheel build
DEBUG Adding license files
TRACE Including project.license-files at `/home/niels/.cache/uv/sdists-v9/.tmpTE2im7/uvtest-0.1.0` with `abc`
DEBUG built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE Adding directory uvtest-0.1.0.dist-info/licenses/
DEBUG Adding metadata files to wheel
TRACE Adding directory uvtest-0.1.0.dist-info
TRACE Adding uvtest-0.1.0.dist-info/WHEEL
TRACE Adding uvtest-0.1.0.dist-info/entry_points.txt
DEBUG Found PEP 639 license declarations, using METADATA 2.4
DEBUG built glob set; 1 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
TRACE Not a file in license files match: .
TRACE Adding uvtest-0.1.0.dist-info/METADATA
TRACE Adding uvtest-0.1.0.dist-info/RECORD
TRACE Adding central directory
Successfully built dist/uvtest-0.1.0.tar.gz
Successfully built dist/uvtest-0.1.0-py3-none-any.whl
DEBUG Released lock at `/home/niels/.cache/uv/.lock`
```

</details>

### Platform

Linux 6.6.87.2-microsoft-standard-WSL2 x86_64 GNU/Linux

### Version

uv 0.9.8

### Python version

Python 3.10.12

---

_Label `bug` added by @N-Wouda on 2025-11-11 21:12_

---

_Renamed from "uv build does not warn when license files do not exist" to "uv build does not warn or fail when license files do not exist" by @N-Wouda on 2025-11-12 10:27_

---

_Comment by @woodruffw on 2025-11-12 19:23_

Thanks for the report @N-Wouda. From a quick look, I suspect that the relevant pieces here are `license_files_source_dist` and `license_files_wheel` -- both return an iterator of glob patterns which the uv build backend then collects licenses from.

(In both cases the actual glob evaluation gets deferred a bit, until the wheel/sdist actually gets constructed. So that's probably why they fail open when no match is found.)

---

_Comment by @N-Wouda on 2025-11-13 10:15_

Ah, thanks. I see a PR is already in the works.

I only noticed this because I couldn't remove one of my packages fully (due to the empty `licenses` directory), and upon investigation found out that the package's `license-files` field contained a typo. An easy thing to miss, but I was surprised `uv` hadn't caught it while building the package. That's what led me to this bug report :).


---

_Closed by @konstin on 2025-11-14 10:02_

---
