```yaml
number: 4100
title: "Request: adding `flake8-requirements`"
type: issue
state: open
author: jamesbraza
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-04-25T20:14:38Z
updated_at: 2024-06-24T22:29:26Z
url: https://github.com/astral-sh/ruff/issues/4100
synced_at: 2026-01-10T11:09:47Z
```

# Request: adding `flake8-requirements`

---

_Issue opened by @jamesbraza on 2023-04-25 20:14_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

https://github.com/Arkq/flake8-requirements is a plugin for `flake8` that validates the `install_requires` field of a `setup.py`.

It's the last `flake8` plugin I use that's not a part of `ruff`.

It would be great to add this into `ruff`!

---

_Label `plugin` added by @charliermarsh on 2023-04-25 23:55_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:26_

---

_Closed by @MichaReiser on 2023-07-27 10:13_

---

_Reopened by @dhruvmanila on 2023-07-27 13:02_

---

_Comment by @jamesbraza on 2024-06-24 22:29_

Maybe this can be a feature of https://github.com/astral-sh/uv? Cc @charliermarsh @zanieb 

Just thinking out loud

---
