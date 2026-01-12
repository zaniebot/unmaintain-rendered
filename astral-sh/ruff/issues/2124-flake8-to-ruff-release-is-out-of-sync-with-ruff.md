```yaml
number: 2124
title: "`flake8-to-ruff` release is out of sync with `ruff`"
type: issue
state: closed
author: ngnpope
labels: []
assignees: []
created_at: 2023-01-24T12:34:26Z
updated_at: 2023-01-24T13:39:59Z
url: https://github.com/astral-sh/ruff/issues/2124
synced_at: 2026-01-12T15:54:42Z
```

# `flake8-to-ruff` release is out of sync with `ruff`

---

_@ngnpope_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

- A minimal code snippet that reproduces the bug.
- The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
- The current Ruff settings (any relevant sections from your `pyproject.toml`).
- The current Ruff version (`ruff --version`).
-->

Right now, the latest release of `flake8-to-ruff` is `0.0.216.dev0` (~2 weeks ago) and the latest release of `ruff` is `0.0.231` (yesterday).

This makes things a little bit confusing as when using the `--plugin` option `flake8-to-ruff` is complaining that plugins are not supported when they are listed in the README as supported, e.g. `flake8-no-pep420` _(supported since ~1 week ago)_ and `flake8-use-pathlib` _(supported since yesterday)_.

Would it be possible to automatically trigger a release of `flake8-to-ruff` after a successful release of `ruff` to avoid this confusion?

---

_Comment by @charliermarsh on 2023-01-24 13:05_

Ah yeah. At the very least I'll start cutting a new release every time. flake8-to-ruff is also a little behind and needs to be updated to include those plugins (a small code change), so I'll do that today too.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-24 13:07_

---

_Comment by @ngnpope on 2023-01-24 13:16_

> ...and needs to be updated to include those plugins...

I did suspect that there might be something more to it. Thanks! 🚀 

---

_Closed by @charliermarsh on 2023-01-24 13:39_

---
