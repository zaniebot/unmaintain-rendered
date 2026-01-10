```yaml
number: 3603
title: "Discussion: `Pre-commit` failures after following Contributing guide"
type: issue
state: closed
author: luke396
labels: []
assignees: []
created_at: 2023-03-19T07:05:11Z
updated_at: 2023-03-19T19:29:48Z
url: https://github.com/astral-sh/ruff/issues/3603
synced_at: 2026-01-10T11:09:46Z
```

# Discussion: `Pre-commit` failures after following Contributing guide

---

_Issue opened by @luke396 on 2023-03-19 07:05_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hello, I am new to Rust and Ruff. I have forked and cloned the repository to my local machine and followed the steps in the [Contributing](https://beta.ruff.rs/docs/contributing/) guide to set up my local environment.

When I ran `pre-commit run --all-files`, there were some failures. I fixed them by following the hook, but it still seemed a little strange. Therefore, I opened an issue and a PR to discuss whether we should accept the changes that the current `pre-commit` requires or change some `pre-commit` configurations to make it work without any failures.

---

_Closed by @charliermarsh on 2023-03-19 19:29_

---
