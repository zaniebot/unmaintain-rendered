```yaml
number: 3099
title: "Display download and installation completion using `Reporter`"
type: pull_request
state: closed
author: zanieb
labels:
  - internal
assignees: []
draft: true
base: main
head: zb/install-report
created_at: 2024-04-17T17:14:14Z
updated_at: 2024-05-25T15:26:58Z
url: https://github.com/astral-sh/uv/pull/3099
synced_at: 2026-01-10T14:32:20Z
```

# Display download and installation completion using `Reporter`

---

_Pull request opened by @zanieb on 2024-04-17 17:14_

Explores moving download and install summaries to the `DownloadReporter` and `InstallReporter` instead of writing in the command implementation manually. This is a small step towards making the `uv pip install` implementation easier to re-use e.g. in `uv run` with custom reporting.

---

_Label `internal` added by @zanieb on 2024-04-17 17:14_

---

_Review comment by @zanieb on `crates/uv-installer/src/installer.rs`:81 on 2024-04-17 17:14_

We never called this apparently.

---

_@zanieb reviewed on 2024-04-17 17:14_

---

_@zanieb reviewed on 2024-04-17 17:24_

---

_Review comment by @zanieb on `crates/uv/src/commands/reporters.rs`:98 on 2024-04-17 17:24_

How does the progress bar implementation get around this? Do they just eat errors?

---

_Closed by @zanieb on 2024-05-25 15:26_

---

_Comment by @zanieb on 2024-05-25 15:26_

To revisit later

---
