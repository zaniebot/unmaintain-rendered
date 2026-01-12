```yaml
number: 19198
title: Import sort grouping does not work for namespaced editable workspace packages
type: issue
state: closed
author: Spenhouet
labels:
  - question
  - isort
assignees: []
created_at: 2025-07-08T09:28:30Z
updated_at: 2025-07-24T11:59:44Z
url: https://github.com/astral-sh/ruff/issues/19198
synced_at: 2026-01-12T15:54:56Z
```

# Import sort grouping does not work for namespaced editable workspace packages

---

_@Spenhouet_

### Summary

When using namespaced editable workspace packages the grouping of the auto sorted imports does not work as expected (mixes internal and external imports).

## Reproduction

1. Download and open in VS Code:  [ruff-bug-sort.zip](https://github.com/user-attachments/files/21117885/ruff-bug-sort.zip)
2. Execute VS code Task "Install Dependencies"
3. Open `apps/qwe-web/main.py`
4. Press save to sort (no change, as it's already sorted)

## Expected Behavior

The imports are grouped into standard packages, external packages and internal packages.

```python
from typing import Any

from fastapi import APIRouter
from fastapi import FastAPI
from fastapi import HTTPException

from airamed.flags import feature
```

## Actual Behavior

```python
from typing import Any

from airamed.flags import feature
from fastapi import APIRouter
from fastapi import FastAPI
from fastapi import HTTPException
```

## Additional Context

The issue here is related to the namespace and not directly to the editable workspace package. Without namespace the import is grouped correctly.

## Environment

OS and version: Windows 11 with WSL and Ubuntu 20.04
Python version: 3.10.18 via uv


### Version

ruff 0.12.2

---

_Comment by @ntBre on 2025-07-08 13:12_

This sounds familiar to me, but I'm not turning up the right related issue at the moment. cc @dylwil3 

You can at least work around it with the [known-first-party](https://docs.astral.sh/ruff/settings/#lint_isort_known-first-party) option in your `.ruff.toml` file, if nothing else.

Here's the project layout:

```
.
├── README.md
├── apps
│   └── qwe-web
│       ├── README.md
│       ├── main.py
│       ├── pyproject.toml
│       └── tests
│           ├── __init__.py
│           ├── conftest.py
│           └── test_main.py
├── components
│   └── flags
│       ├── README.md
│       ├── pyproject.toml
│       └── src
│           └── airamed
│               └── flags
│                   ├── __init__.py
│                   ├── core.py
│                   ├── py.typed
│                   └── testing.py
├── pyproject.toml
├── ruff-bug-sort.zip
├── scripts
│   ├── init.sh
│   └── install.sh
└── uv.lock
```

<details><summary>Verbose logs</summary>

```
[2025-07-08][13:05:15][ruff::resolve][DEBUG] Using configuration file (via parent) at: /home/clod/.ruff.toml
warning: The following rules have been removed and ignoring them has no effect:
    - ANN101
    - ANN102

[2025-07-08][13:05:15][ignore::gitignore][DEBUG] opened gitignore file: /home/clod/.gitignore
[2025-07-08][13:05:15][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/clod/.vscode"
[2025-07-08][13:05:15][ignore::gitignore][DEBUG] opened gitignore file: /home/clod/.ruff_cache/.gitignore
[2025-07-08][13:05:15][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/clod/components/flags/src/airamed/flags/core.py"
[2025-07-08][13:05:15][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/clod/.ruff_cache"
[2025-07-08][13:05:15][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/clod/components/flags/src/airamed/flags/testing.py"
[2025-07-08][13:05:15][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/clod/pyproject.toml"
[2025-07-08][13:05:15][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/clod/components/flags/pyproject.toml"
[2025-07-08][13:05:15][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/clod/components/flags/src/airamed/flags/__init__.py"
[2025-07-08][13:05:15][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/clod/apps/qwe-web/main.py"
[2025-07-08][13:05:15][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/clod/apps/qwe-web/tests/test_main.py"
[2025-07-08][13:05:15][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/clod/apps/qwe-web/pyproject.toml"
[2025-07-08][13:05:15][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/clod/apps/qwe-web/tests/__init__.py"
[2025-07-08][13:05:15][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/clod/apps/qwe-web/tests/conftest.py"
[2025-07-08][13:05:15][ruff::commands::check][DEBUG] Identified files to lint in: 3.81885ms
[2025-07-08][13:05:15][ruff::diagnostics][DEBUG] Checking: /home/clod/components/flags/pyproject.toml
[2025-07-08][13:05:15][ruff::diagnostics][DEBUG] Checking: /home/clod/apps/qwe-web/tests/__init__.py
[2025-07-08][13:05:15][ruff::diagnostics][DEBUG] Checking: /home/clod/apps/qwe-web/main.py
[2025-07-08][13:05:15][ruff::diagnostics][DEBUG] Checking: /home/clod/components/flags/src/airamed/flags/__init__.py
[2025-07-08][13:05:15][ruff::diagnostics][DEBUG] Checking: /home/clod/apps/qwe-web/tests/test_main.py
[2025-07-08][13:05:15][ruff::diagnostics][DEBUG] Checking: /home/clod/pyproject.toml
[2025-07-08][13:05:15][ruff::diagnostics][DEBUG] Checking: /home/clod/components/flags/src/airamed/flags/core.py
[2025-07-08][13:05:15][ruff::diagnostics][DEBUG] Checking: /home/clod/apps/qwe-web/pyproject.toml
[2025-07-08][13:05:15][ruff::diagnostics][DEBUG] Checking: /home/clod/apps/qwe-web/tests/conftest.py
[2025-07-08][13:05:15][ruff::diagnostics][DEBUG] Checking: /home/clod/components/flags/src/airamed/flags/testing.py
[2025-07-08][13:05:15][ruff_linter::settings::types][DEBUG] Adding per-file ignores for "/home/clod/apps/qwe-web/tests/__init__.py" due to absolute match on "(?-u)^/home/clod(?:/|/.*/)tests(?:/|/.*/).*\\.py$": {Assert, BooleanTypeHintPositionalArgument}
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'airamed.flags.core' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'airamed.flags' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'pydantic_settings' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff_linter::settings::types][DEBUG] Adding per-file ignores for "/home/clod/apps/qwe-web/tests/conftest.py" due to absolute match on "(?-u)^/home/clod(?:/|/.*/)tests(?:/|/.*/).*\\.py$": {Assert, BooleanTypeHintPositionalArgument}
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'pytest' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'fastapi' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'pytest' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff_linter::settings::types][DEBUG] Adding per-file ignores for "/home/clod/apps/qwe-web/tests/test_main.py" due to absolute match on "(?-u)^/home/clod(?:/|/.*/)tests(?:/|/.*/).*\\.py$": {Assert, BooleanTypeHintPositionalArgument}
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'airamed.flags.core' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'functools' as Known(StandardLibrary) (KnownStandardLibrary)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'fastapi.testclient' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'airamed.flags.testing' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'pydantic_settings' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'typing' as Known(StandardLibrary) (KnownStandardLibrary)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'typing' as Known(StandardLibrary) (KnownStandardLibrary)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'collections.abc' as Known(StandardLibrary) (KnownStandardLibrary)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'fastapi.testclient' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'fastapi' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'airamed.flags.testing' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'pytest' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'main' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'typing' as Known(StandardLibrary) (KnownStandardLibrary)
[2025-07-08][13:05:15][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'fastapi' as Known(ThirdParty) (NoMatch)
[2025-07-08][13:05:15][ruff::commands::check][DEBUG] Checked 10 files in: 2.659625ms
All checks passed!
```

</details>

---

_Label `isort` added by @ntBre on 2025-07-08 13:12_

---

_Comment by @Spenhouet on 2025-07-08 13:27_

Nice! Yes, adding our namespace to the `.ruff.toml` does fix that for us. As everything is under this single namespace, this is a sufficient fix for us. From my point the issue can be closed. But should you plan to improve the underlying detection, we can leave the issue open.

```
[lint.isort]
known-first-party = ["airamed"]
```

---

_Comment by @MichaReiser on 2025-07-09 07:07_

What I understand is that `airmed` is a package and linked into the venv (using an editable install). Ruff can't infer this but you can tell it that `components/flags/src` is a search path root path by setting the [`src`](https://docs.astral.sh/ruff/settings/#src) setting.

---

_Label `question` added by @MichaReiser on 2025-07-09 07:07_

---

_Closed by @MichaReiser on 2025-07-24 11:59_

---
