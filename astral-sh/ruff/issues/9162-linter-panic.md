```yaml
number: 9162
title: "[Linter panic]"
type: issue
state: closed
author: dycw
labels:
  - bug
assignees: []
created_at: 2023-12-16T12:29:32Z
updated_at: 2023-12-17T12:54:00Z
url: https://github.com/astral-sh/ruff/issues/9162
synced_at: 2026-01-10T11:09:51Z
```

# [Linter panic]

---

_Issue opened by @dycw on 2023-12-16 12:29_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

# Command

```console
ruff --unsafe-fixes --fix .
```

# Stack trace

```console
panicked at /private/tmp/ruff-20231213-5372-n34weh/ruff-0.1.8/crates/ruff_text_size/src/range.rs:48:9:
assertion failed: start.raw <= end.raw
Backtrace:    0: std::backtrace::Backtrace::force_capture
   1: ruff_cli::panic::catch_unwind::{{closure}}
   2: std::panicking::rust_panic_with_hook
   3: std::panicking::begin_panic_handler::{{closure}}
   4: std::sys_common::backtrace::__rust_end_short_backtrace
   5: _rust_begin_unwind
   6: core::panicking::panic_fmt
   7: core::panicking::panic
   8: ruff_linter::linter::lint_fix
   9: ruff_cli::commands::check::check::{{closure}}
  10: rayon::iter::plumbing::bridge_producer_consumer::helper
  11: rayon_core::join::join_context::{{closure}}
  12: rayon::iter::plumbing::bridge_producer_consumer::helper
  13: rayon_core::join::join_context::{{closure}}
  14: rayon::iter::plumbing::bridge_producer_consumer::helper
  15: <rayon_core::job::StackJob<L,F,R> as rayon_core::job::Job>::execute
  16: rayon_core::registry::WorkerThread::wait_until_cold
  17: rayon_core::join::join_context::{{closure}}
  18: rayon::iter::plumbing::bridge_producer_consumer::helper
  19: rayon_core::join::join_context::{{closure}}
  20: rayon::iter::plumbing::bridge_producer_consumer::helper
  21: rayon_core::join::join_context::{{closure}}
  22: rayon::iter::plumbing::bridge_producer_consumer::helper
  23: <rayon_core::job::StackJob<L,F,R> as rayon_core::job::Job>::execute
  24: rayon_core::registry::WorkerThread::wait_until_cold
  25: std::sys_common::backtrace::__rust_begin_short_backtrace
  26: core::ops::function::FnOnce::call_once{{vtable.shim}}
  27: std::sys::unix::thread::Thread::new::thread_start
  28: __pthread_joiner_wake
```

# `pyproject.toml`

```toml
#
# build-system

[build-system]
build-backend = "hatchling.build"
requires = ["hatchling"]

# project
[project]
authors = [{name = "Derek Wan", email = "d.wan@icloud.com"}]
dependencies = [
  "atomicwrites >= 1.4.1",
  "beartype >= 0.16.4",
  "bidict >= 0.22.1",
  "click >= 8.1.7",
  "frozendict >= 2.3.10",
  "loguru >= 0.7.2",
  "more-itertools >= 10.1.0",
  "pathvalidate >= 3.2.0",
  "pyhumps >= 3.8.0",
  "semver >= 3.0.2",
  "tqdm >= 4.66.1",
  "typed-settings >= 23.1.1",
  "typing-extensions >= 4.9.0",
]
dynamic = ["version"]
name = "dycw-utilities"
readme = "README.md"
requires-python = ">= 3.10"

[project.optional-dependencies]
airium = ["airium >= 0.2.6"]
ast-comments = ["ast-comments >= 1.2.0"]
attrs = ["attrs >= 23.1.0"]
beautifulsoup4 = ["beautifulsoup4 >= 4.12.2"]
bottleneck = ["bottleneck >= 1.3.7"]
cryptography = ["cryptography >= 41.0.7"]
cvxpy = ["cvxpy >= 1.4.1"]
dev = [
  # core
  "airium >= 0.2.6",
  "ast-comments >= 1.2.0",
  "attrs >= 23.1.0",
  "beautifulsoup4 >= 4.12.2",
  "bottleneck >= 1.3.7",
  "cryptography >= 41.0.7",
  "cvxpy >= 1.4.1",
  "fastapi >= 0.104.1",
  "fpdf2 >= 2.7.6",
  "hatch >= 1.7.0",
  "holoviews >= 1.18.1",
  "hypothesis >= 6.92.0",
  "luigi >= 3.4.0",
  "mdutils >= 1.6.0",
  "memory-profiler >= 0.61.0",
  "numbagg >= 0.6.7",
  "numpy >= 1.26.2",
  "pandas >= 2.1.3",
  "polars >= 0.19.19",
  "pqdm >= 0.2.0",
  "psutil >= 5.9.6",
  "pydantic >= 2.5.2",
  "pyinstrument >= 4.6.1",
  "pypiserver[passlib] >= 2.0.1",
  "pytest >= 7.4.3",
  "pytest-check >= 2.2.2",
  "scipy >= 1.11.4",
  "selenium >= 4.16.0",
  "sqlalchemy >= 2.0.23",
  "streamlit >= 1.29.0",
  "timeout-decorator >= 0.5.0",
  "xarray >= 2023.11.0",
  "xlrd >= 2.0.1",
  "zarr >= 2.16.1",
  # dev
  "nox",
  "pip-tools",
  # sqlalchemy-dbs
  "cx-oracle",
  "mysqlclient",  # ci-mac-disable
  "psycopg2-binary",
  "pyodbc",
  # test
  "coverage-conditional-plugin >= 0.9.0",
  "exceptiongroup >= 1.1.3",
  "freezegun >= 1.3.1",
  "pytest-cov >= 4.1.0",
  "pytest-instafail >= 0.5.0",
  "pytest-randomly >= 3.15.0",
  "pytest-timeout >= 2.2.0",
  "pytest-xdist >= 3.3.1",
]
fastapi = ["fastapi >= 0.104.1"]
fpdf2 = ["fpdf2 >= 2.7.6", "holoviews >= 1.18.1", "selenium >= 4.16.0"]
hatch = ["hatch >= 1.7.0"]
holoviews = ["holoviews >= 1.18.1", "selenium >= 4.16.0"]
hypothesis = ["hypothesis >= 6.92.0"]
luigi = ["luigi >= 3.4.0"]
mdutils = ["mdutils >= 1.6.0"]
memory-profiler = ["memory-profiler >= 0.61.0"]
numbagg = ["numbagg >= 0.6.7"]
numpy = ["numpy >= 1.26.2"]
pandas = ["pandas >= 2.1.3"]
polars = ["polars >= 0.19.19"]
pqdm = ["pqdm >= 0.2.0"]
psutil = ["psutil >= 5.9.6"]
pydantic = ["pydantic >= 2.5.2"]
pyinstrument = ["pyinstrument >= 4.6.1"]
pypiserver = ["pypiserver[passlib] >= 2.0.1"]
pytest = ["pytest >= 7.4.3"]
pytest-check = ["pytest-check >= 2.2.2"]
scipy = ["scipy >= 1.11.4"]
scripts-csv-to-markdown = ["mdutils >= 1.6.0"]
scripts-generate-snippets = ["ast-comments >= 1.2.0"]
scripts-luigi = ["luigi >= 3.4.0"]
scripts-pypi = ["pypiserver[passlib] >= 2.0.1"]
selenium = ["selenium >= 4.16.0"]
sqlalchemy = ["sqlalchemy >= 2.0.23"]
sqlalchemy-dbs = ["cx-oracle", "mysqlclient", "psycopg2-binary", "pyodbc"]
streamlit = ["streamlit >= 1.29.0"]
test = [
  "exceptiongroup >= 1.1.3",
  "pytest >= 7.4.0",
  "pytest-instafail >= 0.5.0",
  "pytest-randomly >= 3.15.0",
  "pytest-xdist >= 3.3.1",
]
xarray = ["xarray >= 2023.11.0"]
xlrd = ["xlrd >= 2.0.1"]
zarr = ["zarr >= 2.16.1"]

[project.scripts]
clean-dir = "utilities.scripts.clean_dir:main"
csv-to-markdown = "utilities.scripts.csv_to_markdown:main"
generate-snippets = "utilities.scripts.generate_snippets:main"
monitor-memory = "utilities.scripts.monitor_memory:main"
start-luigi-server = "utilities.scripts.luigi.server:main"
start-pypi-server = "utilities.scripts.pypi_server:main"

# coverage
[tool.coverage]

[tool.coverage.coverage_conditional_plugin.rules]
os-eq-linux = 'sys_platform == "linux"'
os-eq-macos = 'sys_platform == "darwin"'
os-eq-windows = 'sys_platform == "windows"'
os-ne-linux = 'sys_platform != "linux"'
os-ne-macos = 'sys_platform != "darwin"'
os-ne-windows = 'sys_platform != "windows"'
version-ge-311 = "sys_version_info >= (3, 11)"

[tool.coverage.html]
directory = ".coverage/html"

[tool.coverage.report]
exclude_lines = [
  "@overload",
  "# pragma: no cover",
  "assert_never",
  "case _ as never:",
]
fail_under = 100.0
skip_covered = true
skip_empty = true

[tool.coverage.run]
branch = true
data_file = ".coverage/data"
omit = ["src/utilities/clean_dir/__main__.py", "src/utilities/streamlit.py"]
parallel = true
plugins = ["coverage_conditional_plugin"]

# hatch
[tool.hatch]

[tool.hatch.build]
sources = ["src"]

[tool.hatch.build.targets.wheel]
packages = ["src/utilities"]

[tool.hatch.version]
path = "src/utilities/__init__.py"

# nitpick
[tool.nitpick]
style = [
  "https://raw.githubusercontent.com/dycw/nitpick/master/styles/3.10.toml",
  "https://raw.githubusercontent.com/dycw/nitpick/master/styles/common.toml",
  "https://raw.githubusercontent.com/dycw/nitpick/master/styles/pip-compile-no-hashes.toml",
]

# pyright
[tool.pyright]
exclude = ["**/__pycache__", ".direnv", ".git", ".nox"]
executionEnvironments = [{root = "src"}]
include = ["src"]
pythonVersion = "3.10"
reportImplicitOverride = "error"
reportImportCycles = "error"
reportMissingSuperCall = "error"
reportMissingTypeArgument = false
reportMissingTypeStubs = false
reportPrivateImportUsage = false
reportPrivateUsage = false
reportPropertyTypeMismatch = "error"
reportShadowedImports = "error"
reportUninitializedInstanceVariable = "error"
reportUnknownArgumentType = false
reportUnknownMemberType = false
reportUnknownParameterType = false
reportUnknownVariableType = false
reportUnnecessaryTypeIgnoreComment = "error"
reportUntypedBaseClass = false
reportUnusedCallResult = "error"
typeCheckingMode = "strict"

# pytest
[tool.pytest]

[tool.pytest.ini_options]
addopts = [
  "-rsxX",
  "--color=auto",
  "--cov=utilities",
  "--cov-config=pyproject.toml",
  "--cov-report=html",
  "--strict-markers",
  "--tb=native",
]
filterwarnings = [
  "error",
  "ignore::DeprecationWarning",
  "ignore::FutureWarning",
  "ignore:Implicitly cleaning up <TemporaryDirectory '.*'>:ResourceWarning",
  "ignore:unclosed file <.*>:ResourceWarning",
]
minversion = "7.0"
testpaths = ["src/tests"]
timeout = 600
xfail_strict = true

# ruff
[tool.ruff]
ignore = [
  "ANN101",  # flake8-annotations, missing-type-self
  "ANN102",  # flake8-annotations, missing-type-cls
  "ANN401",  # flake8-annotations, dynamically-typed-expression
  "B008",  # flake8-bugbear, function-call-argument-default
  "COM812",  # flake8-commas, trailing-comma-missing
  "PGH003",  # pygrep-hooks, blanket-type-ignore
  "PLR0913",  # refactor, too-many-arguments
  "PT012",  # flake8-pytest-style, raises-with-multiple-statements
  "PT013",  # flake8-pytest-style, incorrect-pytest-import
  # formatter
  "E501",  # pycodestyle, line-too-long
  "ISC001",  # flake8-implicit-str-concat, single-line-implicit-string-concatenation
  "W191",  # pycodestyle, tab-indentation
]
select = [
  "A",  # flake8-builtins
  "ANN",  # flake8-annotations
  "ARG",  # flake8-unused-arguments
  "ASYNC",  # flake8-async
  "B",  # flake8-bugbear
  "BLE",  # flake8-blind-excpt
  "C4",  # flake8-comprehensions
  "DTZ",  # flake8-datetimez
  "E",  # pycodestyle
  "EM",  # flake8-errmsg
  "ERA",  # eradicate
  "EXE",  # flake8-executable
  "F",  # pyflakes
  "FA",  # flake8-future-annotations
  "FBT",  # flake8-boolean-trap
  "FIX",  # flake8-fixme
  "FLY",  # flynt
  "FURB",  # refurb
  "G",  # flake8-logging-format
  "I",  # isort
  "ICN",  # flake8-import-conventions
  "INP",  # flake8-no-pep420
  "INT",  # flake8-gettext
  "ISC",  # flake8-implicit-str-concat
  "LOG",  # flake8-logging
  "N",  # pep8-naming
  "NPY",  # numpy-specific-rules
  "PERF",  # perflint
  "PGH",  # pygrep-hooks
  "PIE",  # flake8-pie
  "PL",  # pylint
  "PT",  # flake8-pytest-style
  "PTH",  # flake8-use-pathlib
  "PYI",  # flake8-pyi
  "RET",  # flake8-return
  "RSE",  # flake8-raise
  "RUF",  # ruff
  "S",  # flake8-bandit
  "SIM",  # flake8-simplify
  "SLF",  # flake8-self
  "SLOT",  # flake8-slots
  "T10",  # flake8-debugger
  "T20",  # flake8-print
  "TD",  # flake8-todos
  "TID",  # flake8-tidy-imports
  "TCH",  # flake8-type-checking
  "TRY",  # tryceratops
  "UP",  # pyupgrade
  "W",  # pycodestyle
  "YTT",  # flake8-2020
]
src = ["src"]
target-version = "py310"

[tool.ruff.lint.extend-per-file-ignores]
"src/tests/**/*.py" = [
  "FBT001",  # flake8-boolean-trap, boolean-positional-arg-in-function-definition
  "FBT003",  # flake8-boolean-trap, boolean-positional-value-in-function-call
  "PLR2004",  # refactor, magic-value-comparison
  "S101",  # flake8-bandit, assert-used
]
"src/tests/luigi/test_*.py" = ["I002"]  # isort, missing-required-import
"src/tests/typed_settings/test_*.py" = ["I002"]  # isort, missing-required-import
"src/utilities/_luigi/typed_settings.py" = [
  "I002",
]  # isort, missing-required-import

[tool.ruff.lint.flake8-bugbear]
extend-immutable-calls = ["utilities.typed_settings.click_field"]

[tool.ruff.lint.flake8-tidy-imports]
ban-relative-imports = "all"

[tool.ruff.lint.flake8-type-checking]
quote-annotations = true
```

# Version

```console
> ruff version
ruff 0.1.8
```

## 

P.S. Sorry it's currently not feasible for me to out a minimal example..

---

_Comment by @charliermarsh on 2023-12-16 14:20_

Thanks for filing. Unfortunately there isn't much we can do without an example file or code snippet that triggers the issue (minimal or not -- anything would be fine). As-is, it's a very generic trace, so it's kind of like saying "There's a bug _somewhere_ in Ruff". Any chance you can provide more info? Otherwise, I'll have to close :/

---

_Label `question` added by @charliermarsh on 2023-12-16 14:20_

---

_Comment by @dycw on 2023-12-16 16:44_

Hi @charliermarsh , I've gone ahead and done this (even though it's late!).

Please see https://github.com/dycw/ruff-0.1.8-bug/tree/0.1.0 , or, for posterity:

## `pyproject.toml`

```toml
[tool.ruff]
select = ["TCH"]

[tool.ruff.lint.flake8-type-checking]
quote-annotations = true
```

## `test.py` 

```python
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import Callable

    from something import asdf


def func() -> Callable[[Callable[_P, _R]], Callable[_P, _R]]:
    """Factory for decorators which caches locally using pickles."""
```

## Result

```console
┌─────────────────────────────────────────────────────────── 2023-12-17 00:41:51
│ DW-Mac derek .../ruff-0.1.8-bug  master ? ! +4-1
│ 🐍 3.10.13
└ ❯ which ruff
/opt/homebrew/bin/ruff

┌─────────────────────────────────────────────────────────── 2023-12-17 00:41:57
│ DW-Mac derek .../ruff-0.1.8-bug  master ? ! +4-1
│ 🐍 3.10.13
└ ❯ ruff --version
ruff 0.1.8

┌─────────────────────────────────────────────────────────── 2023-12-17 00:42:01
│ DW-Mac derek .../ruff-0.1.8-bug  master ? ! +4-1
│ 🐍 3.10.13
└ ❯ ruff --unsafe-fixes --fix .
error: Panicked while linting test.py: This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BLinter%20panic%5D

...with the relevant file contents, the `pyproject.toml` settings, and the following stack trace, we'd be very appreciative!

panicked at /private/tmp/ruff-20231213-5372-n34weh/ruff-0.1.8/crates/ruff_text_size/src/range.rs:48:9:
assertion failed: start.raw <= end.raw
Backtrace:    0: std::backtrace::Backtrace::force_capture
   1: ruff_cli::panic::catch_unwind::{{closure}}
   2: std::panicking::rust_panic_with_hook
   3: std::panicking::begin_panic_handler::{{closure}}
   4: std::sys_common::backtrace::__rust_end_short_backtrace
   5: _rust_begin_unwind
   6: core::panicking::panic_fmt
   7: core::panicking::panic
   8: ruff_linter::linter::lint_fix
   9: ruff_cli::commands::check::check::{{closure}}
  10: rayon::iter::plumbing::bridge_producer_consumer::helper
  11: rayon_core::join::join_context::{{closure}}
  12: <rayon_core::job::StackJob<L,F,R> as rayon_core::job::Job>::execute
  13: rayon_core::registry::WorkerThread::wait_until_cold
  14: std::sys_common::backtrace::__rust_begin_short_backtrace
  15: core::ops::function::FnOnce::call_once{{vtable.shim}}
  16: std::sys::unix::thread::Thread::new::thread_start
  17: __pthread_joiner_wake
```

---

_Comment by @charliermarsh on 2023-12-16 17:24_

Awesome, thanks! With a repro we can definitely fix. Much appreciated.

---

_Label `question` removed by @charliermarsh on 2023-12-16 17:24_

---

_Label `bug` added by @charliermarsh on 2023-12-16 17:24_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-16 21:34_

---

_Closed by @charliermarsh on 2023-12-17 12:54_

---
