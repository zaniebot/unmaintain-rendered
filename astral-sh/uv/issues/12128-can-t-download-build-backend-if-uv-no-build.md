```yaml
number: 12128
title: "Can't download build-backend if UV_NO_BUILD_ISOLATION=1"
type: issue
state: closed
author: JakkuSakura
labels:
  - bug
assignees: []
created_at: 2025-03-12T03:42:06Z
updated_at: 2025-03-12T20:59:37Z
url: https://github.com/astral-sh/uv/issues/12128
synced_at: 2026-01-12T16:00:55Z
```

# Can't download build-backend if UV_NO_BUILD_ISOLATION=1

---

_@JakkuSakura_

### Summary

Setuptools are not being installed automatically. So when default-on build-isolation and on newer python versions >= 3.12, I can't install my package
```log
uv venv
Using CPython 3.13.2 interpreter at: /usr/bin/python3.13
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
uv pip install -e .
Resolved 18 packages in 2.02s
  × Failed to build `pybuild @ file:///home/jakku/Dev/pybuild`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta.build_editable` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 8, in <module>
          import setuptools.build_meta as backend
      ModuleNotFoundError: No module named 'setuptools'

      hint: This usually indicates a problem with the package or the build environment.
```

I already have my `pyproject.toml`
```toml
[build-system]
requires = [
    "setuptools>=61.0",
]
build-backend = "setuptools.build_meta"

[project]
name = "pybuild"
description = "A custom build tool"
authors = [
    { name = "Dev", email = "dev@example.com" },
]
dependencies = [
    "tomli~=2.2",
    "tomli-w~=1.2",
    "typer~=0.15",
    "pip-requirements-parser~=32.0",
    "pydantic!=2.10",
    "pyyaml~=6.0",
]
requires-python = ">=3.10"
version = "0.3.0"

[tool.setuptools]

[tool.ruff]
line-length = 99
target-version = "py310"
fix = true
unsafe-fixes = true

[tool.ruff.lint]
select = [
    "E",
    "F",
    "W",
    "B",
    "I",
    "RUF100",
    "PGH004",
    "UP",
    "C4",
    "SIM201",
    "SIM202",
    "RET501",
    "RET502",
]
ignore = [
    "B007",
    "B011",
    "B023",
    "E402",
    "E501",
    "E721",
    "E731",
    "E741",
    "UP031",
    "UP032",
    "C416",
]
unfixable = [
    "F841",
    "F601",
    "F602",
    "B018",
    "UP036",
]

[tool.ruff.lint.isort]
combine-as-imports = true
extra-standard-library = [
    "typing_extensions",
]

[tool.pyright]
include = [
    "src",
    "tests",
]
exclude = [
    "**/node_modules",
    "**/__pycache__",
    "src/experimental",
    "src/typestubs",
]
ignore = []
reportMissingImports = "error"
reportMissingTypeStubs = false

[tool.pyright.defineConstant]
DEBUG = true

[tool.pytest.ini_options]
minversion = "7.0.0"
testpaths = [
    "tests",
]
python_files = "test_*.py"

[tool.coverage.run]
branch = true
source = [
    "pybuild",
]

[tool.coverage.report]
format = "markdown"
show_missing = true
skip_covered = false
omit = [
    "tests/*",
]
exclude_lines = [
    "# pragma: no cover",
    "raise AssertionError\\b",
    "raise NotImplementedError\\b",
    "return NotImplemented\\b",
    "raise$",
    "assert False\\b",
    "if __name__ == ['\"]__main__['\"]:$",
]

[dependency-groups]
dev = [
    "pytest>=7.0",
    "coverage>=7.5",
    "pyright>=1.1",
    "ruff>=0.9",
    "pytest-cov>=6.0",
]
```

But if I use other build backends or build on MacOS, it has no issues

### Platform

Ubuntu 25, Linux 6.11.0-14-generic x86_64 GNU/Linux

### Version

uv 0.5.30 (1cd9c3 2025.2.12)

### Python version

Python 3.13.2

---

_Label `bug` added by @JakkuSakura on 2025-03-12 03:42_

---

_Comment by @konstin on 2025-03-12 10:15_

Can you try `uv pip install -v --no-cache -e .`, and if that still fails, share the output?

---

_Comment by @JakkuSakura on 2025-03-12 11:07_

sometimes other build backends fail. I'm trying to reproduce and see if I can get the cause

---

_Renamed from "Can't download setuptools build-backend on python>=3.12 on Ubuntu" to "Can't download build-backend on Ubuntu" by @JakkuSakura on 2025-03-12 11:13_

---

_Comment by @JakkuSakura on 2025-03-12 11:14_

```log
rm -rf .venv && uv venv && uv pip install -e . -v --no-cache 
Using CPython 3.10.0
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
DEBUG uv 0.5.30 (0f9788fe3 2025-02-20) # I tried different versions
DEBUG Searching for default Python interpreter in virtual environments
DEBUG Found `cpython-3.10.0-linux-x86_64-gnu` at `/home/jakku/Dev/pybuild/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.10.0 environment at: .venv
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: file:///home/jakku/Dev/pybuild
DEBUG Using request timeout of 30s
DEBUG Found PEP 621 metadata for /home/jakku/Dev/pybuild in `pyproject.toml` (pybuild)
DEBUG Found static `pyproject.toml` for: pybuild @ file:///home/jakku/Dev/pybuild
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.10
DEBUG Solving with target Python version: >=3.10
DEBUG Adding direct dependency: pybuild*
DEBUG Searching for a compatible version of pybuild @ file:///home/jakku/Dev/pybuild (*)
DEBUG Adding transitive dependency for pybuild==0.3.0: tomli>=2.2, <3.dev0
DEBUG Adding transitive dependency for pybuild==0.3.0: tomli-w>=1.2, <2.dev0
DEBUG Adding transitive dependency for pybuild==0.3.0: typer>=0.15, <1.dev0
DEBUG Adding transitive dependency for pybuild==0.3.0: pip-requirements-parser>=32.0, <33.dev0
DEBUG Adding transitive dependency for pybuild==0.3.0: pydantic<2.10 | >=2.10+
DEBUG Adding transitive dependency for pybuild==0.3.0: pyyaml>=6.0, <7.dev0
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/tomli/
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/tomli-w/
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/typer/
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/pip-requirements-parser/
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/pyyaml/
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/pydantic/
DEBUG No netrc file found
DEBUG No cache entry for: https://pypi.org/simple/tomli/
DEBUG No cache entry for: https://pypi.org/simple/tomli-w/
DEBUG No cache entry for: https://pypi.org/simple/pip-requirements-parser/
WARN Fixing invalid version specifier by removing star after comparison operator other than equal and not equal (before: `>=3.6.*`; after: `>=3.6`)
WARN Fixing invalid version specifier by removing star after comparison operator other than equal and not equal (before: `>=3.6.*`; after: `>=3.6`)
WARN Fixing invalid version specifier by removing star after comparison operator other than equal and not equal (before: `>=3.6.*`; after: `>=3.6`)
WARN Fixing invalid version specifier by removing star after comparison operator other than equal and not equal (before: `>=3.6.*`; after: `>=3.6`)
WARN Fixing invalid version specifier by removing star after comparison operator other than equal and not equal (before: `>=3.6.*`; after: `>=3.6`)
WARN Fixing invalid version specifier by removing star after comparison operator other than equal and not equal (before: `>=3.6.*`; after: `>=3.6`)
WARN Fixing invalid version specifier by removing star after comparison operator other than equal and not equal (before: `>=3.6.*`; after: `>=3.6`)
WARN Fixing invalid version specifier by removing star after comparison operator other than equal and not equal (before: `>=3.6.*`; after: `>=3.6`)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/54/d0/d04f1d1e064ac901439699ee097f58688caadea42498ec9c4b4ad2ef84ab/pip_requirements_parser-32.0.1-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/c7/18/c86eb8e0202e32dd3df50d43d7ff9854f8e0603945ff398974c1d91ac1ef/tomli_w-1.2.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://pypi.org/simple/pydantic/
DEBUG Searching for a compatible version of tomli (>=2.2, <3.dev0)
DEBUG Selecting: tomli==2.2.1 [compatible] (tomli-2.2.1-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/6e/c2/61d3e0f47e2b74ef40a68b9e6ad5984f6241a942f7cd3bbfbdbd03861ea9/tomli-2.2.1-py3-none-any.whl.metadata
DEBUG No cache entry for: https://pypi.org/simple/typer/
DEBUG No cache entry for: https://pypi.org/simple/pyyaml/
DEBUG Searching for a compatible version of tomli-w (>=1.2, <2.dev0)
DEBUG Selecting: tomli-w==1.2.0 [compatible] (tomli_w-1.2.0-py3-none-any.whl)
DEBUG Searching for a compatible version of typer (>=0.15, <1.dev0)
DEBUG Selecting: typer==0.15.2 [compatible] (typer-0.15.2-py3-none-any.whl)
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py2.5.exe
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py2.6.exe
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py3.0.exe
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py3.1.exe
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py3.2.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py2.6.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py3.1.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py3.2.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py3.3.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py3.4.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py2.6.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py3.1.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py3.2.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py3.3.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py3.4.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win-amd64-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win-amd64-py3.4.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win-amd64-py3.5.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win32-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win32-py3.4.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win32-py3.5.exe
WARN Skipping file for pyyaml: PyYAML-4.2b4.win-amd64-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-4.2b4.win-amd64-py3.4.exe
WARN Skipping file for pyyaml: PyYAML-4.2b4.win32-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-4.2b4.win32-py3.4.exe
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/f4/3c/8cc1cc84deffa6e25d2d0c688ebb80635dfdbf1dbea3e30c541c8cf4d860/pydantic-2.10.6-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/6b/4e/1523cb902fd98355e2e9ea5e5eb237cbc5f3ad5f3075fa65087aa0ecb669/PyYAML-6.0.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Adding transitive dependency for typer==0.15.2: click>=8.0.0
DEBUG Adding transitive dependency for typer==0.15.2: typing-extensions>=3.7.4.3
DEBUG Adding transitive dependency for typer==0.15.2: shellingham>=1.3.0
DEBUG Adding transitive dependency for typer==0.15.2: rich>=10.11.0
DEBUG Searching for a compatible version of pip-requirements-parser (>=32.0, <33.dev0)
DEBUG Selecting: pip-requirements-parser==32.0.1 [compatible] (pip_requirements_parser-32.0.1-py3-none-any.whl)
DEBUG Adding transitive dependency for pip-requirements-parser==32.0.1: packaging*
DEBUG Adding transitive dependency for pip-requirements-parser==32.0.1: pyparsing*
DEBUG Searching for a compatible version of pydantic (<2.10 | >=2.10+)
DEBUG Selecting: pydantic==2.10.6 [compatible] (pydantic-2.10.6-py3-none-any.whl)
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/click/
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/typing-extensions/
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/shellingham/
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/rich/
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/packaging/
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/pyparsing/
DEBUG Adding transitive dependency for pydantic==2.10.6: annotated-types>=0.6.0
DEBUG Adding transitive dependency for pydantic==2.10.6: pydantic-core>=2.27.2, <2.27.2+
DEBUG Adding transitive dependency for pydantic==2.10.6: typing-extensions>=4.12.2
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/annotated-types/
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/pydantic-core/
DEBUG No cache entry for: https://pypi.org/simple/typing-extensions/
DEBUG No cache entry for: https://pypi.org/simple/shellingham/
DEBUG No cache entry for: https://pypi.org/simple/packaging/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl.metadata
WARN Fixing invalid version specifier by removing stray quotes (before: `>= '2.7'`; after: `>= 2.7`)
WARN Fixing invalid version specifier by removing stray quotes (before: `>= '2.7'`; after: `>= 2.7`)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl.metadata
DEBUG No cache entry for: https://pypi.org/simple/pydantic-core/
DEBUG No cache entry for: https://pypi.org/simple/annotated-types/
DEBUG No cache entry for: https://pypi.org/simple/pyparsing/
WARN Skipping file for pyparsing: pyparsing-1.5.1.win32.exe
WARN Skipping file for pyparsing: pyparsing-1.5.2.win32.exe
WARN Skipping file for pyparsing: pyparsing-1.5.3.win32-py2.4.exe
WARN Skipping file for pyparsing: pyparsing-1.5.3.win32-py2.5.exe
WARN Skipping file for pyparsing: pyparsing-1.5.3.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-1.5.3.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-1.5.3.win32-py3.0.exe
WARN Skipping file for pyparsing: pyparsing-1.5.3.win32-py3.1.exe
WARN Skipping file for pyparsing: pyparsing-1.5.3.win32-py3.2.exe
WARN Skipping file for pyparsing: pyparsing-1.5.4.win32-py2.4.exe
WARN Skipping file for pyparsing: pyparsing-1.5.4.win32-py2.5.exe
WARN Skipping file for pyparsing: pyparsing-1.5.4.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-1.5.4.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-1.5.4.win32-py3.0.exe
WARN Skipping file for pyparsing: pyparsing-1.5.4.win32-py3.1.exe
WARN Skipping file for pyparsing: pyparsing-1.5.4.win32-py3.2.exe
WARN Skipping file for pyparsing: pyparsing-1.5.5.win32-py2.4.exe
WARN Skipping file for pyparsing: pyparsing-1.5.5.win32-py2.5.exe
WARN Skipping file for pyparsing: pyparsing-1.5.5.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-1.5.5.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-1.5.5.win32-py3.0.exe
WARN Skipping file for pyparsing: pyparsing-1.5.5.win32-py3.1.exe
WARN Skipping file for pyparsing: pyparsing-1.5.5.win32-py3.2.exe
WARN Skipping file for pyparsing: pyparsing-1.5.6.win32-py2.4.exe
WARN Skipping file for pyparsing: pyparsing-1.5.6.win32-py2.5.exe
WARN Skipping file for pyparsing: pyparsing-1.5.6.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-1.5.6.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-1.5.6.win32-py3.0.exe
WARN Skipping file for pyparsing: pyparsing-1.5.6.win32-py3.1.exe
WARN Skipping file for pyparsing: pyparsing-1.5.6.win32-py3.2.exe
WARN Skipping file for pyparsing: pyparsing-1.5.7.win32-py2.5.exe
WARN Skipping file for pyparsing: pyparsing-1.5.7.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-1.5.7.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.0.0.win32-py3.0.exe
WARN Skipping file for pyparsing: pyparsing-2.0.0.win32-py3.1.exe
WARN Skipping file for pyparsing: pyparsing-2.0.0.win32-py3.2.exe
WARN Skipping file for pyparsing: pyparsing-2.0.0.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.0.1.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.0.1.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.0.1.win32-py3.0.exe
WARN Skipping file for pyparsing: pyparsing-2.0.1.win32-py3.1.exe
WARN Skipping file for pyparsing: pyparsing-2.0.1.win32-py3.2.exe
WARN Skipping file for pyparsing: pyparsing-2.0.1.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.0.2.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.0.2.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.0.2.win32-py3.0.exe
WARN Skipping file for pyparsing: pyparsing-2.0.2.win32-py3.1.exe
WARN Skipping file for pyparsing: pyparsing-2.0.2.win32-py3.2.exe
WARN Skipping file for pyparsing: pyparsing-2.0.2.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.0.2.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.0.3.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.0.3.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.0.3.win32-py3.0.exe
WARN Skipping file for pyparsing: pyparsing-2.0.3.win32-py3.1.exe
WARN Skipping file for pyparsing: pyparsing-2.0.3.win32-py3.2.exe
WARN Skipping file for pyparsing: pyparsing-2.0.3.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.0.3.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.0.4.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.0.4.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.0.4.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.0.4.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.0.4.win32-py3.5.exe
WARN Skipping file for pyparsing: pyparsing-2.0.5.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.0.5.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.0.5.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.0.5.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.0.5.win32-py3.5.exe
WARN Skipping file for pyparsing: pyparsing-2.0.6.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.0.6.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.0.6.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.0.6.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.0.6.win32-py3.5.exe
WARN Skipping file for pyparsing: pyparsing-2.0.7.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.0.7.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.0.7.win32-py3.2.exe
WARN Skipping file for pyparsing: pyparsing-2.0.7.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.0.7.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.0.7.win32-py3.5.exe
WARN Skipping file for pyparsing: pyparsing-2.1.0.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.1.0.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.1.0.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.1.0.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.1.0.win32-py3.5.exe
WARN Skipping file for pyparsing: pyparsing-2.1.1.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.1.1.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.1.1.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.1.1.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.1.1.win32-py3.5.exe
WARN Skipping file for pyparsing: pyparsing-2.1.10.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.1.10.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.1.10.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.1.10.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.1.10.win32-py3.5.exe
WARN Skipping file for pyparsing: pyparsing-2.1.2.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.1.2.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.1.2.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.1.2.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.1.2.win32-py3.5.exe
WARN Skipping file for pyparsing: pyparsing-2.1.3.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.1.3.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.1.3.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.1.3.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.1.3.win32-py3.5.exe
WARN Skipping file for pyparsing: pyparsing-2.1.4.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.1.4.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.1.4.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.1.4.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.1.4.win32-py3.5.exe
WARN Skipping file for pyparsing: pyparsing-2.1.5.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.1.5.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.1.5.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.1.5.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.1.5.win32-py3.5.exe
WARN Skipping file for pyparsing: pyparsing-2.1.6.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.1.6.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.1.6.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.1.6.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.1.6.win32-py3.5.exe
WARN Skipping file for pyparsing: pyparsing-2.1.7.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.1.7.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.1.7.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.1.7.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.1.7.win32-py3.5.exe
WARN Skipping file for pyparsing: pyparsing-2.1.8.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.1.8.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.1.8.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.1.8.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.1.8.win32-py3.5.exe
WARN Skipping file for pyparsing: pyparsing-2.1.9.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.1.9.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.1.9.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.1.9.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.1.9.win32-py3.5.exe
WARN Skipping file for pyparsing: pyparsing-2.2.0.win32-py2.6.exe
WARN Skipping file for pyparsing: pyparsing-2.2.0.win32-py2.7.exe
WARN Skipping file for pyparsing: pyparsing-2.2.0.win32-py3.3.exe
WARN Skipping file for pyparsing: pyparsing-2.2.0.win32-py3.4.exe
WARN Skipping file for pyparsing: pyparsing-2.2.0.win32-py3.5.exe
DEBUG No cache entry for: https://pypi.org/simple/rich/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/78/b6/6307fbef88d9b5ee7421e68d78a9f162e0da4900bc5f5793f6d3d0e34fb8/annotated_types-0.7.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/1c/a7/c8a2d361bf89c0d9577c934ebb7421b25dc84bf3a8e3ac0a40aed9acc547/pyparsing-3.2.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pydantic-core (>=2.27.2, <2.27.2+)
DEBUG Selecting: pydantic-core==2.27.2 [compatible] (pydantic_core-2.27.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/32/90/3b15e31b88ca39e9e626630b4c4a1f5a0dfd09076366f4219429e6786076/pydantic_core-2.27.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for pydantic-core==2.27.2: typing-extensions>=4.6.0, <4.7.0 | >=4.7.0+
DEBUG Searching for a compatible version of pyyaml (>=6.0, <7.dev0)
DEBUG Selecting: pyyaml==6.0.2 [compatible] (PyYAML-6.0.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG No cache entry for: https://pypi.org/simple/click/
DEBUG Searching for a compatible version of click (>=8.0.0)
DEBUG Selecting: click==8.1.8 [compatible] (click-8.1.8-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of typing-extensions (>=4.12.2)
DEBUG Selecting: typing-extensions==4.12.2 [compatible] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of shellingham (>=1.3.0)
DEBUG Selecting: shellingham==1.5.4 [compatible] (shellingham-1.5.4-py2.py3-none-any.whl)
DEBUG Searching for a compatible version of rich (>=10.11.0)
DEBUG Selecting: rich==13.9.4 [compatible] (rich-13.9.4-py3-none-any.whl)
DEBUG Adding transitive dependency for rich==13.9.4: markdown-it-py>=2.2.0
DEBUG Adding transitive dependency for rich==13.9.4: pygments>=2.13.0, <3.0.0
DEBUG Adding transitive dependency for rich==13.9.4: typing-extensions{python_full_version < '3.11'}>=4.0.0, <5.0
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.11'} (>=4.0.0, <5.0)
DEBUG Selecting: typing-extensions==4.12.2 [compatible] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/pygments/
DEBUG Adding transitive dependency for typing-extensions==4.12.2: typing-extensions==4.12.2
DEBUG Adding transitive dependency for typing-extensions==4.12.2: typing-extensions{python_full_version < '3.11'}==4.12.2
DEBUG Searching for a compatible version of typing-extensions{python_full_version < '3.11'} (==4.12.2)
DEBUG Selecting: typing-extensions==4.12.2 [compatible] (typing_extensions-4.12.2-py3-none-any.whl)
DEBUG Searching for a compatible version of packaging (*)
DEBUG Selecting: packaging==24.2 [compatible] (packaging-24.2-py3-none-any.whl)
DEBUG Searching for a compatible version of pyparsing (*)
DEBUG Selecting: pyparsing==3.2.1 [compatible] (pyparsing-3.2.1-py3-none-any.whl)
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/markdown-it-py/
DEBUG Searching for a compatible version of annotated-types (>=0.6.0)
DEBUG Selecting: annotated-types==0.7.0 [compatible] (annotated_types-0.7.0-py3-none-any.whl)
DEBUG No cache entry for: https://pypi.org/simple/markdown-it-py/
DEBUG Searching for a compatible version of markdown-it-py (>=2.2.0)
DEBUG Selecting: markdown-it-py==3.0.0 [compatible] (markdown_it_py-3.0.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for markdown-it-py==3.0.0: mdurl>=0.1, <1.dev0
DEBUG No cache entry for: https://gitlab.com/api/v4/projects/auros%2Fpybuild/packages/pypi/mdurl/
DEBUG No cache entry for: https://pypi.org/simple/pygments/
WARN Skipping file for pygments: Pygments-0.10-py2.3.egg
WARN Skipping file for pygments: Pygments-0.10-py2.4.egg
WARN Skipping file for pygments: Pygments-0.10-py2.5.egg
WARN Skipping file for pygments: Pygments-0.11-py2.3.egg
WARN Skipping file for pygments: Pygments-0.11-py2.4.egg
WARN Skipping file for pygments: Pygments-0.11-py2.5.egg
WARN Skipping file for pygments: Pygments-0.11.1-py2.3.egg
WARN Skipping file for pygments: Pygments-0.11.1-py2.4.egg
WARN Skipping file for pygments: Pygments-0.11.1-py2.5.egg
WARN Skipping file for pygments: Pygments-0.5-py2.3.egg
WARN Skipping file for pygments: Pygments-0.5-py2.4.egg
WARN Skipping file for pygments: Pygments-0.5-py2.5.egg
WARN Skipping file for pygments: Pygments-0.5.1-py2.3.egg
WARN Skipping file for pygments: Pygments-0.5.1-py2.4.egg
WARN Skipping file for pygments: Pygments-0.5.1-py2.5.egg
WARN Skipping file for pygments: Pygments-0.6-py2.3.egg
WARN Skipping file for pygments: Pygments-0.6-py2.4.egg
WARN Skipping file for pygments: Pygments-0.6-py2.5.egg
WARN Skipping file for pygments: Pygments-0.7-py2.3.egg
WARN Skipping file for pygments: Pygments-0.7-py2.4.egg
WARN Skipping file for pygments: Pygments-0.7-py2.5.egg
WARN Skipping file for pygments: Pygments-0.7.1-py2.3.egg
WARN Skipping file for pygments: Pygments-0.7.1-py2.4.egg
WARN Skipping file for pygments: Pygments-0.7.1-py2.5.egg
WARN Skipping file for pygments: Pygments-0.8-py2.3.egg
WARN Skipping file for pygments: Pygments-0.8-py2.4.egg
WARN Skipping file for pygments: Pygments-0.8-py2.5.egg
WARN Skipping file for pygments: Pygments-0.8.1-py2.3.egg
WARN Skipping file for pygments: Pygments-0.8.1-py2.4.egg
WARN Skipping file for pygments: Pygments-0.8.1-py2.5.egg
WARN Skipping file for pygments: Pygments-0.9-py2.3.egg
WARN Skipping file for pygments: Pygments-0.9-py2.4.egg
WARN Skipping file for pygments: Pygments-0.9-py2.5.egg
WARN Skipping file for pygments: Pygments-1.0-py2.3.egg
WARN Skipping file for pygments: Pygments-1.0-py2.4.egg
WARN Skipping file for pygments: Pygments-1.0-py2.5.egg
WARN Skipping file for pygments: Pygments-1.0-py2.6.egg
WARN Skipping file for pygments: Pygments-1.1-py2.4.egg
WARN Skipping file for pygments: Pygments-1.1-py2.5.egg
WARN Skipping file for pygments: Pygments-1.1-py2.6.egg
WARN Skipping file for pygments: Pygments-1.1.1-py2.4.egg
WARN Skipping file for pygments: Pygments-1.1.1-py2.5.egg
WARN Skipping file for pygments: Pygments-1.1.1-py2.6.egg
WARN Skipping file for pygments: Pygments-1.2-py2.4.egg
WARN Skipping file for pygments: Pygments-1.2-py2.5.egg
WARN Skipping file for pygments: Pygments-1.2-py2.6.egg
WARN Skipping file for pygments: Pygments-1.2.1-py2.4.egg
WARN Skipping file for pygments: Pygments-1.2.1-py2.5.egg
WARN Skipping file for pygments: Pygments-1.2.1-py2.6.egg
WARN Skipping file for pygments: Pygments-1.2.2-py2.4.egg
WARN Skipping file for pygments: Pygments-1.2.2-py2.5.egg
WARN Skipping file for pygments: Pygments-1.2.2-py2.6.egg
WARN Skipping file for pygments: Pygments-1.3-py2.4.egg
WARN Skipping file for pygments: Pygments-1.3-py2.5.egg
WARN Skipping file for pygments: Pygments-1.3-py2.6.egg
WARN Skipping file for pygments: Pygments-1.3.1-py2.4.egg
WARN Skipping file for pygments: Pygments-1.3.1-py2.5.egg
WARN Skipping file for pygments: Pygments-1.3.1-py2.6.egg
WARN Skipping file for pygments: Pygments-1.4-py2.4.egg
WARN Skipping file for pygments: Pygments-1.4-py2.5.egg
WARN Skipping file for pygments: Pygments-1.4-py2.6.egg
WARN Skipping file for pygments: Pygments-1.4-py2.7.egg
WARN Skipping file for pygments: Pygments-1.5-py2.6.egg
WARN Skipping file for pygments: Pygments-1.5-py2.7.egg
WARN Skipping file for pygments: Pygments-1.6-py2.6.egg
WARN Skipping file for pygments: Pygments-1.6-py2.7.egg
WARN Skipping file for pygments: Pygments-1.6rc1-py2.6.egg
WARN Skipping file for pygments: Pygments-1.6rc1-py2.7.egg
DEBUG Searching for a compatible version of pygments (>=2.13.0, <3.0.0)
DEBUG Selecting: pygments==2.19.1 [compatible] (pygments-2.19.1-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl.metadata
DEBUG No cache entry for: https://pypi.org/simple/mdurl/
DEBUG Searching for a compatible version of mdurl (>=0.1, <1.dev0)
DEBUG Selecting: mdurl==0.1.2 [compatible] (mdurl-0.1.2-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl.metadata
DEBUG Tried 18 versions: annotated-types 1, click 1, markdown-it-py 1, mdurl 1, packaging 1, pip-requirements-parser 1, pybuild 1, pydantic 1, pydantic-core 1, pygments 1, pyparsing 1, pyyaml 1, rich 1, shellingham 1, tomli 1, tomli-w 1, typer 1, typing-extensions 1
DEBUG marker environment resolution took 1.736s
Resolved 18 packages in 1.73s
DEBUG Identified uncached distribution: pydantic-core==2.27.2
DEBUG Identified uncached distribution: annotated-types==0.7.0
DEBUG Identified uncached distribution: mdurl==0.1.2
DEBUG Identified uncached distribution: rich==13.9.4
DEBUG Identified uncached distribution: pydantic==2.10.6
DEBUG Identified uncached distribution: pygments==2.19.1
DEBUG Identified uncached distribution: shellingham==1.5.4
DEBUG Identified uncached distribution: pyparsing==3.2.1
DEBUG Identified uncached distribution: click==8.1.8
DEBUG Identified uncached distribution: packaging==24.2
DEBUG Identified uncached distribution: typer==0.15.2
DEBUG Identified uncached distribution: pyyaml==6.0.2
DEBUG Identified uncached distribution: tomli==2.2.1
DEBUG Identified uncached distribution: pip-requirements-parser==32.0.1
DEBUG Identified uncached distribution: typing-extensions==4.12.2
DEBUG Identified uncached distribution: pybuild @ file:///home/jakku/Dev/pybuild
DEBUG Identified uncached distribution: markdown-it-py==3.0.0
DEBUG Identified uncached distribution: tomli-w==1.2.0
DEBUG Acquired lock for `/tmp/.tmpio0OfR/sdists-v7/editable/a73cdf1d7de5c09e`
DEBUG No cache entry for: https://files.pythonhosted.org/packages/8a/0b/9fcc47d19c48b59121088dd6da2488a49d5f72dacf8262e2790a1d2c7d15/pygments-2.19.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/f4/3c/8cc1cc84deffa6e25d2d0c688ebb80635dfdbf1dbea3e30c541c8cf4d860/pydantic-2.10.6-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/32/90/3b15e31b88ca39e9e626630b4c4a1f5a0dfd09076366f4219429e6786076/pydantic_core-2.27.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/19/71/39c7c0d87f8d4e6c020a393182060eaefeeae6c01dab6a84ec346f2567df/rich-13.9.4-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/6b/4e/1523cb902fd98355e2e9ea5e5eb237cbc5f3ad5f3075fa65087aa0ecb669/PyYAML-6.0.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7e/d4/7ebdbd03970677812aac39c869717059dbb71a4cfc033ca6e5221787892c/click-8.1.8-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/1c/a7/c8a2d361bf89c0d9577c934ebb7421b25dc84bf3a8e3ac0a40aed9acc547/pyparsing-3.2.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/ef/eb23f262cca3c0c4eb7ab1933c3b1f03d021f2c48f54763065b6f0e321be/packaging-24.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7f/fc/5b29fea8cee020515ca82cc68e3b8e1e34bb19a3535ad854cac9257b414c/typer-0.15.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/26/9f/ad63fc0248c5379346306f8668cda6e2e2e9c95e01216d2b8ffd9ff037d0/typing_extensions-4.12.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/6e/c2/61d3e0f47e2b74ef40a68b9e6ad5984f6241a942f7cd3bbfbdbd03861ea9/tomli-2.2.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/54/d0/d04f1d1e064ac901439699ee097f58688caadea42498ec9c4b4ad2ef84ab/pip_requirements_parser-32.0.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/78/b6/6307fbef88d9b5ee7421e68d78a9f162e0da4900bc5f5793f6d3d0e34fb8/annotated_types-0.7.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/c7/18/c86eb8e0202e32dd3df50d43d7ff9854f8e0603945ff398974c1d91ac1ef/tomli_w-1.2.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e0/f9/0595336914c5619e5f28a1fb793285925a8cd4b432c9da0a987836c7f822/shellingham-1.5.4-py2.py3-none-any.whl
   Building pybuild @ file:///home/jakku/Dev/pybuild
DEBUG Building: pybuild @ file:///home/jakku/Dev/pybuild
DEBUG No workspace root found, using project root
DEBUG Proceeding without build isolation
DEBUG Calling `uv_build.build_editable("/tmp/.tmpio0OfR/builds-v0/.tmpPhmgR9", {}, None)`
Downloading pygments (1.2MiB)
Downloading pydantic-core (1.9MiB)
DEBUG Traceback (most recent call last):
DEBUG   File "<string>", line 8, in <module>
DEBUG ModuleNotFoundError: No module named 'uv_build'
DEBUG Released lock at `/tmp/.tmpio0OfR/sdists-v7/editable/a73cdf1d7de5c09e/.lock`
  × Failed to build `pybuild @ file:///home/jakku/Dev/pybuild`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `uv_build.build_editable` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 8, in <module>
      ModuleNotFoundError: No module named 'uv_build'

      hint: This usually indicates a problem with the package or the build environment.
DEBUG Released lock at `/home/jakku/Dev/pybuild/.venv/.lock`
```

---

_Comment by @JakkuSakura on 2025-03-12 11:19_

https://github.com/tox-dev/tox-uv/issues/15
Might be similar to this issue

---

_Comment by @JakkuSakura on 2025-03-12 11:43_

I reproduced it on an empty repo

rm -rf .venv pyproject.toml && uv init && uv venv && uv pip install -e . -v
works when UV_NO_BUILD_ISOLATION=0
it doesn't work when UV_NO_BUILD_ISOLATION=1

There should be some document saying, if we enable NO_BUILD_ISOLATION, build-backend will not be automatically installed

---

_Renamed from "Can't download build-backend on Ubuntu" to "Can't download build-backend if UV_NO_BUILD_ISOLATION=1" by @JakkuSakura on 2025-03-12 11:50_

---

_Closed by @JakkuSakura on 2025-03-12 11:50_

---

_Comment by @JakkuSakura on 2025-03-12 11:53_

Oh, maybe I should keep it open. uv should be downloading build-backend into venv even with no-build-isolation

To me, no-build-isolation sounds like "we don't use separate venvs for build, but try to build things in current venv"

---

_Reopened by @JakkuSakura on 2025-03-12 11:53_

---

_Comment by @charliermarsh on 2025-03-12 20:59_

I think this is working as intended (and should match pip): if you use `--no-build-isolation`, we don't install any build requirements -- you have to take care of that part.

---

_Closed by @charliermarsh on 2025-03-12 20:59_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-12 20:59_

---
