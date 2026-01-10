```yaml
number: 3228
title: Upgrade macOS target to 12.0
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/manylinux
created_at: 2024-04-23T23:18:09Z
updated_at: 2024-04-23T23:28:15Z
url: https://github.com/astral-sh/uv/pull/3228
synced_at: 2026-01-10T14:37:54Z
```

# Upgrade macOS target to 12.0

---

_Pull request opened by @charliermarsh on 2024-04-23 23:18_

## Summary

macOS 11 has been EOL for 7 months, and it seems like it's common to publish ARM-only wheels at 12.0+. I think this is a better default for ARM macs.

Closes https://github.com/astral-sh/uv/issues/3227.

## Test Plan

`cargo run pip install scikit-learn==1.3.2 --python-platform macos --no-build`


---

_Marked ready for review by @charliermarsh on 2024-04-23 23:18_

---

_Comment by @charliermarsh on 2024-04-23 23:25_

I think this is better as a default. I'm going to enable the API to accept arbitrary macOS and manylinux versions too.

---

_Label `enhancement` added by @charliermarsh on 2024-04-23 23:25_

---

_Merged by @charliermarsh on 2024-04-23 23:28_

---

_Closed by @charliermarsh on 2024-04-23 23:28_

---

_Branch deleted on 2024-04-23 23:28_

---
