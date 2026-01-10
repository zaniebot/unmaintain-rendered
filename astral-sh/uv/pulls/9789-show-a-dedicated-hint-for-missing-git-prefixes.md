```yaml
number: 9789
title: "Show a dedicated hint for missing `git+` prefixes"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/hint-github
created_at: 2024-12-10T21:17:51Z
updated_at: 2024-12-10T21:29:38Z
url: https://github.com/astral-sh/uv/pull/9789
synced_at: 2026-01-10T12:00:01Z
```

# Show a dedicated hint for missing `git+` prefixes

---

_Pull request opened by @charliermarsh on 2024-12-10 21:17_

## Summary

This has been bothering me a bit: `uv pip install "foo @ https://github.com/user/foo"` fails, telling you that it doesn't end in a supported extension. But we should be able to tell you that it looks like a Git repo.


---

_Label `error messages` added by @charliermarsh on 2024-12-10 21:17_

---

_Review requested from @zanieb by @charliermarsh on 2024-12-10 21:17_

---

_@zanieb approved on 2024-12-10 21:19_

---

_Merged by @charliermarsh on 2024-12-10 21:29_

---

_Closed by @charliermarsh on 2024-12-10 21:29_

---

_Branch deleted on 2024-12-10 21:29_

---
