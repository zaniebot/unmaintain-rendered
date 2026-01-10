---
number: 6553
title: Provide pydocstyle convention setting through the CLI
type: issue
state: closed
author: Jonas1312
labels: []
assignees: []
created_at: 2023-08-14T08:32:43Z
updated_at: 2023-08-14T17:41:53Z
url: https://github.com/astral-sh/ruff/issues/6553
synced_at: 2026-01-10T01:22:45Z
---

# Provide pydocstyle convention setting through the CLI

---

_Issue opened by @Jonas1312 on 2023-08-14 08:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hi,

I would like to set the [pydocstyle convention](https://beta.ruff.rs/docs/settings/#pydocstyle-convention) to "google", but I struggle to pass this setting to the CLI.

Is it only possible to change this setting in the toml config?

Thanks

---

_Comment by @charliermarsh on 2023-08-14 17:41_

Yeah, it's only possible to set via the TOML file right now. We've avoided exposing every parameter to the CLI to keep the interface lean. In the future, we'll likely implement a generic mechanism for setting any argument (it's discussed a bit here: https://github.com/astral-sh/ruff/issues/5102). Sorry for the inconvenience.

---

_Closed by @charliermarsh on 2023-08-14 17:41_

---
