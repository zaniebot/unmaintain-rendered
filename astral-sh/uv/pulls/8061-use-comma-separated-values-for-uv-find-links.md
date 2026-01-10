```yaml
number: 8061
title: "Use comma-separated values for `UV_FIND_LINKS`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/f
created_at: 2024-10-09T22:57:51Z
updated_at: 2024-10-09T23:05:52Z
url: https://github.com/astral-sh/uv/pull/8061
synced_at: 2026-01-10T12:54:02Z
```

# Use comma-separated values for `UV_FIND_LINKS`

---

_Pull request opened by @charliermarsh on 2024-10-09 22:57_

## Summary

These values can include spaces when passed on the command-line... Clap doesn't give us a way to provide a value separator for _only_ an environment variable (as is pip's behavior), so I think we're stuck using comma-separated for here right now.

See, e.g., https://github.com/clap-rs/clap/discussions/3796.

Closes #8057.

---

_Label `bug` added by @charliermarsh on 2024-10-09 22:57_

---

_Merged by @charliermarsh on 2024-10-09 23:05_

---

_Closed by @charliermarsh on 2024-10-09 23:05_

---

_Branch deleted on 2024-10-09 23:05_

---
