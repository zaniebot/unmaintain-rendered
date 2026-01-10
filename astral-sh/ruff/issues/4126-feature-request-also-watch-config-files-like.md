```yaml
number: 4126
title: "[feature request] Also watch config files, like `pyproject.toml` in `--watch` mode. "
type: issue
state: closed
author: bgianfo
labels:
  - cli
assignees: []
created_at: 2023-04-26T23:09:20Z
updated_at: 2023-06-05T19:23:25Z
url: https://github.com/astral-sh/ruff/issues/4126
synced_at: 2026-01-10T11:09:47Z
```

# [feature request] Also watch config files, like `pyproject.toml` in `--watch` mode. 

---

_Issue opened by @bgianfo on 2023-04-26 23:09_

It would be very nice if the watch mode would also watch for changes to configuration files.

Such a feature would make iterating on enabling new rulesets a lot easier. 
<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Label `cli` added by @charliermarsh on 2023-04-27 03:56_

---

_Comment by @mikeleppane on 2023-04-30 07:12_

Hi! Would it be okay if I take a look on this (good first issue). 
Proposal: detect changes in pyproject.toml and ruff.toml config files. 

---

_Comment by @charliermarsh on 2023-05-01 00:02_

@mikeleppane - Definitely! I haven't thought too hard about what's required, but feel free to ask questions here as they come up. My guess is that, in the `match rx.recv()` loop in `crates/ruff_cli/src/lib.rs`, we can check if the file ends in `.toml`, then re-run the code to generate the `pyproject_strategy` and other variables that make it into the `commands::run::run` call.

---

_Comment by @charliermarsh on 2023-06-05 19:23_

Oh, we added this! From @mikeleppane in https://github.com/charliermarsh/ruff/pull/4169.

---

_Closed by @charliermarsh on 2023-06-05 19:23_

---
