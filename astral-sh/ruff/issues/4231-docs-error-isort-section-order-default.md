```yaml
number: 4231
title: "Docs error: isort `section-order` default"
type: issue
state: closed
author: Secrus
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2023-05-04T20:10:38Z
updated_at: 2023-05-04T21:35:29Z
url: https://github.com/astral-sh/ruff/issues/4231
synced_at: 2026-01-10T11:09:47Z
```

# Docs error: isort `section-order` default

---

_Issue opened by @Secrus on 2023-05-04 20:10_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
The [docs](https://beta.ruff.rs/docs/settings/#section-order) for `isort` `section-order` setting, list default value as `[]`. From my testing and [code spelunking](https://github.com/charliermarsh/ruff/blob/64b7280eb824d0e5f9da887e82dcda53838dd38d/crates/ruff/src/rules/isort/settings.rs#LL343C83-L343C83), it seems, that the default value is actually not empty, but the same as the order of sections in the enum that represent this. 


---

_Label `documentation` added by @charliermarsh on 2023-05-04 20:11_

---

_Comment by @charliermarsh on 2023-05-04 20:11_

Yeah, I think you're right.

---

_Label `good first issue` added by @charliermarsh on 2023-05-04 20:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-04 21:28_

---

_Closed by @charliermarsh on 2023-05-04 21:35_

---
