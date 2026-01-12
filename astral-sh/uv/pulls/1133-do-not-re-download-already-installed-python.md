```yaml
number: 1133
title: Do not re-download already installed Python versions
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/skip-redownload
created_at: 2024-01-26T19:41:26Z
updated_at: 2024-01-27T18:46:22Z
url: https://github.com/astral-sh/uv/pull/1133
synced_at: 2026-01-12T16:04:27Z
```

# Do not re-download already installed Python versions

---

_@zanieb_

A minor usability improvement cherry-picked from https://github.com/astral-sh/puffin/pull/1131 e.g.

```
❯ scripts/bootstrap/install.sh
Installing cpython-3.8.12-darwin-arm64
Already available, skipping download
Updated executables for python3.8.12
Installing cpython-3.8.18-darwin-arm64
Already available, skipping download
Updated executables for python3.8.18
Installing cpython-3.9.18-darwin-arm64
Already available, skipping download
Updated executables for python3.9.18
Installing cpython-3.10.13-darwin-arm64
Already available, skipping download
Updated executables for python3.10.13
Installing cpython-3.11.7-darwin-arm64
Already available, skipping download
Updated executables for python3.11.7
Installing cpython-3.12.1-darwin-arm64
Already available, skipping download
Updated executables for python3.12.1
Done!
```

---

_Label `internal` added by @zanieb on 2024-01-26 19:42_

---

_@charliermarsh approved on 2024-01-27 01:51_

---

_Merged by @zanieb on 2024-01-27 18:46_

---

_Closed by @zanieb on 2024-01-27 18:46_

---

_Branch deleted on 2024-01-27 18:46_

---
