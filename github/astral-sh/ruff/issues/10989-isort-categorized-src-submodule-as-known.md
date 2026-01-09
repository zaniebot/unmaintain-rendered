---
number: 10989
title: "isort Categorized `src` submodule as Known(ThirdParty) (NoMatch) inside `src`"
type: issue
state: open
author: serjflint
labels:
  - isort
assignees: []
created_at: 2024-04-17T07:12:38Z
updated_at: 2024-04-17T19:18:36Z
url: https://github.com/astral-sh/ruff/issues/10989
synced_at: 2026-01-07T13:12:15-06:00
---

# isort Categorized `src` submodule as Known(ThirdParty) (NoMatch) inside `src`

---

_Issue opened by @serjflint on 2024-04-17 07:12_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hi!

ruff 0.3.4

I am trying to use `ruff --select I` on our monorepo

```
.
├── pyproject.toml
└── src
    └── dir
        └── module
            ├── __init__.py
            └── a.py
        └── test_module
            ├── __init__.py
            └── test_a.py
``` 

I have a config

```toml
[tool.ruff]
...
src = ["src/*"]

[tool.ruff.lint]
preview = true
select = ["I"]
...
```

When I run ruff from beside `pyproject.toml` everything works as intended

```test_a.py
import pytest

from module import a
```

But when I run ruff from inside src/dir the test_a.py is sorted in wrong order

```test_a.py
from module import a
import pytest
```

```bash
cd src/dir
ruff check --fix-only --verbose --config /Users/serjflint/monorepo/pyproject.toml test_module/test_a.py
warning: One or more modules are part of multiple import sections, including: `setup`
[2024-04-17][09:58:26][ruff::resolve][DEBUG] Using user-specified configuration file at: /Users/serjflint/monorepo/pyproject.toml
[2024-04-17][09:58:26][ruff::commands::check][DEBUG] Identified files to lint in: 4.061541ms
[2024-04-17][09:58:26][ruff::diagnostics][DEBUG] Checking: /Users/serjflint/monorepo/src/dir/test_module/test_a.py
[2024-04-17][09:58:26][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'pytest' as Known(ThirdParty) (KnownThirdParty)
[2024-04-17][09:58:26][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'module' as Known(ThirdParty) (NoMatch)
[2024-04-17][09:58:26][ruff::commands::check][DEBUG] Checked 1 files in: 33.535708ms
```

Seems similar to issue https://github.com/astral-sh/ruff/issues/10266 but the other way around

---

_Comment by @serjflint on 2024-04-17 08:23_

Probably related to https://github.com/astral-sh/ruff/issues/10927 in some way

---

_Comment by @serjflint on 2024-04-17 08:25_

As a workaround I added everything from "src/*" to `known-first-party`. https://github.com/astral-sh/ruff/issues/10926 would have helped here.

---

_Label `isort` added by @AlexWaygood on 2024-04-17 11:04_

---

_Referenced in [astral-sh/ruff#11000](../../astral-sh/ruff/issues/11000.md) on 2024-04-17 13:40_

---

_Comment by @charliermarsh on 2024-04-17 14:45_

I'll try to reproduce.

---

_Comment by @charliermarsh on 2024-04-17 14:47_

I'm unable to reproduce:

```
project/src/dir on  main [$?]
❯ cargo run check . --fix -n --verbose
    Finished dev [unoptimized + debuginfo] target(s) in 0.10s
     Running `/Users/crmarsh/workspace/ruff/target/debug/ruff check . --fix -n --verbose`
[2024-04-17][10:47:26][ruff::resolve][DEBUG] Using configuration file (via parent) at: /Users/crmarsh/workspace/ruff/project/pyproject.toml
[2024-04-17][10:47:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/crmarsh/workspace/ruff/project/src/dir/module/__init__.py"
[2024-04-17][10:47:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/crmarsh/workspace/ruff/project/src/dir/module/a.py"
[2024-04-17][10:47:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/crmarsh/workspace/ruff/project/src/dir/test_module/test_a.py"
[2024-04-17][10:47:26][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/crmarsh/workspace/ruff/project/src/dir/test_module/__init__.py"
[2024-04-17][10:47:26][ruff::commands::check][DEBUG] Identified files to lint in: 13.962334ms
[2024-04-17][10:47:26][ruff::diagnostics][DEBUG] Checking: /Users/crmarsh/workspace/ruff/project/src/dir/module/a.py
[2024-04-17][10:47:26][ruff::diagnostics][DEBUG] Checking: /Users/crmarsh/workspace/ruff/project/src/dir/test_module/__init__.py
[2024-04-17][10:47:26][ruff::diagnostics][DEBUG] Checking: /Users/crmarsh/workspace/ruff/project/src/dir/test_module/test_a.py
[2024-04-17][10:47:26][ruff::diagnostics][DEBUG] Checking: /Users/crmarsh/workspace/ruff/project/src/dir/module/__init__.py
[2024-04-17][10:47:26][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'pytest' as Known(ThirdParty) (NoMatch)
[2024-04-17][10:47:26][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'module' as Known(FirstParty) (SourceMatch("/Users/crmarsh/workspace/ruff/project/src/dir"))
[2024-04-17][10:47:26][ruff::commands::check][DEBUG] Checked 4 files in: 4.112042ms
All checks passed!
```

---

_Comment by @charliermarsh on 2024-04-17 14:48_

The problem is your use of `--config`. Using `--config` means we have to use the current working directory as the root for resolving relative paths.

---

_Comment by @charliermarsh on 2024-04-17 14:48_

Because we can't organically discover a workspace root.

---

_Comment by @serjflint on 2024-04-17 14:51_

> The problem is your use of `--config`. Using `--config` means we have to use the current working directory as the root for resolving relative paths.

Thanks for the investigation. How to pass a working directory as a parameter? Or force to use the directory of the config for path resolution?

---

_Comment by @charliermarsh on 2024-04-17 14:53_

The original example you provided works because we discover the workspace root by discovering the `pyproject.toml` on disk in the parent directory. That's the intended setup -- can you not do that?

---

_Comment by @serjflint on 2024-04-17 14:56_

> The original example you provided works because we discover the workspace root by discovering the `pyproject.toml` on disk in the parent directory. That's the intended setup -- can you not do that?

Unfortunately I can not. There is a lot of CI with different wrappers and users can call ruff from any place in the repo.

---

_Comment by @charliermarsh on 2024-04-17 18:15_

If you use configuration on-disk, rather than passing `--config`, the calls will _always_ yield the same results regardless of the current working directory. It's the most robust way to invoke Ruff.

---

_Comment by @serjflint on 2024-04-17 19:18_

> If you use configuration on-disk, rather than passing `--config`, the calls will _always_ yield the same results regardless of the current working directory. It's the most robust way to invoke Ruff.

Not unless someone puts their one config in a submodule. 

My use case is to force everyone in the project use the same config as in the CI regardless. And I wish for that config to mean the same everywhere in the project.

I can fallback to an on-disk configuration or write a wrapper to replace cwd, but that would still be a workaround. The same as my current solution with manually passing all submodules.

---

_Referenced in [astral-sh/ruff-pre-commit#103](../../astral-sh/ruff-pre-commit/issues/103.md) on 2025-03-03 06:28_

---
