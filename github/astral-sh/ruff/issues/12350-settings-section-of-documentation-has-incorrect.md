---
number: 12350
title: Settings section of documentation has incorrect default value for output-format
type: issue
state: closed
author: sashko1988
labels:
  - documentation
assignees: []
created_at: 2024-07-16T16:48:10Z
updated_at: 2024-07-19T17:51:48Z
url: https://github.com/astral-sh/ruff/issues/12350
synced_at: 2026-01-07T13:12:15-06:00
---

# Settings section of documentation has incorrect default value for output-format

---

_Issue opened by @sashko1988 on 2024-07-16 16:48_

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
"documentation", "default settings", "output-format", 0.5.2 version

`Settings` section for toml files still shows `concise` as default (https://docs.astral.sh/ruff/settings/#output-format):

> The style in which violation messages should be formatted: "full" (shows source), "concise" (default), "grouped" (group messages by file), "json" (machine-readable), "junit" (machine-readable XML), "github" (GitHub Actions annotations), "gitlab" (GitLab CI code quality report), "pylint" (Pylint text format) or "azure" (Azure Pipeline logging commands).
> 
> Default value: "concise"

While cli outputs full as a default:
> --output-format <OUTPUT_FORMAT>    Output serialization format for violations. The default serialization format is "full"

And `ruff` actually uses `full` if a user doesn't specify it. 



---

_Comment by @charliermarsh on 2024-07-16 16:56_

Thank you -- it looks like we forgot to update `crates/ruff_workspace/src/options.rs`. Are you interested in submitting a PR?

---

_Label `documentation` added by @charliermarsh on 2024-07-16 16:56_

---

_Comment by @sashko1988 on 2024-07-16 17:14_

@charliermarsh would love to! 

---

_Referenced in [astral-sh/ruff#12409](../../astral-sh/ruff/pulls/12409.md) on 2024-07-19 17:47_

---

_Closed by @charliermarsh on 2024-07-19 17:51_

---

_Closed by @charliermarsh on 2024-07-19 17:51_

---
