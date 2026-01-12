```yaml
number: 2736
title: Add Homebrew system check test
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/add-brew-test
created_at: 2024-03-30T14:51:51Z
updated_at: 2024-04-01T14:50:46Z
url: https://github.com/astral-sh/uv/pull/2736
synced_at: 2026-01-12T16:05:11Z
```

# Add Homebrew system check test

---

_@zanieb_

Following #2735 adds a system check that uses Homebrew. I think we were never were actually using Homebrew's Python in the past, we were mislead or something changed in the runners recently that broke it.

---

_Label `testing` added by @zanieb on 2024-03-30 14:51_

---

_@zanieb reviewed on 2024-03-30 14:54_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:485 on 2024-03-30 14:54_

Alternatively, we could just `brew install python3` and that will be on the path. Probably cleaner unless we actually care about Python 3.8 coverage? @charliermarsh 

---

_@samypr100 reviewed on 2024-04-01 01:20_

---

_Review comment by @samypr100 on `.github/workflows/ci.yml`:485 on 2024-04-01 01:20_

Would `brew link --force python@3.8` help instead of a manual symlink?

---

_Marked ready for review by @zanieb on 2024-04-01 14:47_

---

_Merged by @zanieb on 2024-04-01 14:50_

---

_Closed by @zanieb on 2024-04-01 14:50_

---

_Branch deleted on 2024-04-01 14:50_

---
