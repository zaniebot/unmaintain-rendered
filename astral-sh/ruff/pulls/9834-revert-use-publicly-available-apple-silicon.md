```yaml
number: 9834
title: "Revert \"Use publicly available Apple Silicon runners (#9726)\""
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - release
assignees: []
merged: true
base: main
head: charlie/silicon
created_at: 2024-02-05T15:53:32Z
updated_at: 2024-02-05T16:24:52Z
url: https://github.com/astral-sh/ruff/pull/9834
synced_at: 2026-01-12T15:55:30Z
```

# Revert "Use publicly available Apple Silicon runners (#9726)"

---

_@charliermarsh_

## Summary

Sadly, the Apple Silicon runners use macOS 14 and produce binaries that segfault when run on macOS 11 (at least), and possibly on macOS 12 and/or macOS 13.

macOS 11 is EOL, but it doesn't seem like a good tradeoff to speed up our release builds at the expense of user support and compatibility.

This reverts commit f0066e1b895ab4e0541c405b27127129dbd34d43.

Closes https://github.com/astral-sh/ruff/issues/9823.


---

_Review requested from @konstin by @charliermarsh on 2024-02-05 15:53_

---

_Marked ready for review by @charliermarsh on 2024-02-05 15:53_

---

_Label `bug` added by @charliermarsh on 2024-02-05 15:53_

---

_Label `release` added by @charliermarsh on 2024-02-05 15:53_

---

_Review requested from @zanieb by @charliermarsh on 2024-02-05 15:56_

---

_@MichaReiser approved on 2024-02-05 16:02_

😢 

---

_@konstin approved on 2024-02-05 16:04_

---

_Merged by @charliermarsh on 2024-02-05 16:24_

---

_Closed by @charliermarsh on 2024-02-05 16:24_

---

_Branch deleted on 2024-02-05 16:24_

---
