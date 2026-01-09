---
number: 2304
title: Using global config is impacting the behaviour of the tests
type: issue
state: closed
author: JonathanPlasse
labels:
  - bug
  - internal
assignees: []
created_at: 2023-01-28T17:19:05Z
updated_at: 2023-01-28T18:32:51Z
url: https://github.com/astral-sh/ruff/issues/2304
synced_at: 2026-01-07T13:12:14-06:00
---

# Using global config is impacting the behaviour of the tests

---

_Issue opened by @JonathanPlasse on 2023-01-28 17:19_

When running the tests with this as global config.
```toml
# /home/jonathan/.config/ruff/ruff.toml
# https://github.com/charliermarsh/ruff#reference
target-version = "py37"
select = ["ALL"]
ignore = [
    "ANN101", # missing-type-self
    "ANN102", # missing-type-cls
    "ANN401", # dynamically-typed-expression
    "D10",    # missing-docstring
    "DTZ003", # call-datetime-utcnow
    "E501",   # line-too-long
]
unfixable = [
    "ERA001", # commented-out-code
    "F401",   # unused-import
    "F841",   # unused-variable
    "T20",    # print-found
]

[flake8-bugbear] # https://github.com/charliermarsh/ruff#flake8-bugbear
extend-immutable-calls = ["fastapi.Body", "fastapi.Depends"]

[flake8-builtins] # https://github.com/charliermarsh/ruff#flake8-builtins
builtins-ignorelist = []

[flake8-pytest-style] # https://github.com/charliermarsh/ruff#flake8-pytest-style-pt
fixture-parentheses = false
mark-parentheses = false
parametrize-names-type = "csv"

[isort]
known-third-party = []

[pep8-naming] # https://github.com/charliermarsh/ruff#pep8-naming
classmethod-decorators = [
    "classmethod",
    "pydantic.validator",
    "sqlalchemy.orm.declared_attr",
]

[pydocstyle] # https://github.com/charliermarsh/ruff#pydocstyle
convention = "pep257"
```
Running `cargo test --all` fails with this error messages
```console
---- test_stdin_filename stdout ----
thread 'test_stdin_filename' panicked at 'assertion failed: `(left == right)`
  left: `"F401.py:1:1: INP001 File `F401.py` is part of an implicit namespace package. Add an `__init__.py`.\nF401.py:1:8: F401 `os` imported but unused\nFound 2 errors.\n1 potentially fixable with the --fix option.\n"`,
 right: `"F401.py:1:8: F401 `os` imported but unused\nFound 1 error.\n1 potentially fixable with the --fix option.\n"`', ruff_cli/tests/integration_test.rs:48:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- test_stdin_json stdout ----
thread 'test_stdin_json' panicked at 'assertion failed: `(left == right)`
  left: `"[\n  {\n    \"code\": \"INP001\",\n    \"message\": \"File `F401.py` is part of an implicit namespace package. Add an `__init__.py`.\",\n    \"fix\": null,\n    \"location\": {\n      \"row\": 1,\n      \"column\": 1\n    },\n    \"end_location\": {\n      \"row\": 1,\n      \"column\": 1\n    },\n    \"filename\": \"/home/jonathan/Documents/github/ruff/ruff_cli/F401.py\"\n  },\n  {\n    \"code\": \"F401\",\n    \"message\": \"`os` imported but unused\",\n    \"fix\": null,\n    \"location\": {\n      \"row\": 1,\n      \"column\": 8\n    },\n    \"end_location\": {\n      \"row\": 1,\n      \"column\": 10\n    },\n    \"filename\": \"/home/jonathan/Documents/github/ruff/ruff_cli/F401.py\"\n  }\n]\n"`,
 right: `"[\n  {\n    \"code\": \"F401\",\n    \"message\": \"`os` imported but unused\",\n    \"fix\": {\n      \"content\": \"\",\n      \"message\": \"Remove unused import: `os`\",\n      \"location\": {\n        \"row\": 1,\n        \"column\": 0\n      },\n      \"end_location\": {\n        \"row\": 2,\n        \"column\": 0\n      }\n    },\n    \"location\": {\n      \"row\": 1,\n      \"column\": 8\n    },\n    \"end_location\": {\n      \"row\": 1,\n      \"column\": 10\n    },\n    \"filename\": \"/home/jonathan/Documents/github/ruff/ruff_cli/F401.py\"\n  }\n]\n"`', ruff_cli/tests/integration_test.rs:70:5


failures:
    test_stdin_filename
    test_stdin_json

test result: FAILED. 7 passed; 2 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.08s

error: test failed, to rerun pass `-p ruff_cli --test integration_test`
```
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->


---

_Comment by @charliermarsh on 2023-01-28 17:29_

We should run the tests under `--isolated` mode, I think.

---

_Label `bug` added by @charliermarsh on 2023-01-28 17:29_

---

_Label `internal` added by @charliermarsh on 2023-01-28 17:29_

---

_Referenced in [astral-sh/ruff#2306](../../astral-sh/ruff/pulls/2306.md) on 2023-01-28 18:02_

---

_Closed by @charliermarsh on 2023-01-28 18:32_

---

_Referenced in [astral-sh/ruff#9870](../../astral-sh/ruff/issues/9870.md) on 2025-12-05 19:23_

---
