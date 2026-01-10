```yaml
number: 17322
title: "Respect script lockfiles in `--with-requirements`"
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
base: main
head: charlie/target-lock
created_at: 2026-01-05T00:26:16Z
updated_at: 2026-01-05T14:36:46Z
url: https://github.com/astral-sh/uv/pull/17322
synced_at: 2026-01-10T05:49:14Z
```

# Respect script lockfiles in `--with-requirements`

---

_Pull request opened by @charliermarsh on 2026-01-05 00:26_

## Summary

Closes https://github.com/astral-sh/uv/issues/17243.


---

_Label `bug` added by @charliermarsh on 2026-01-05 00:26_

---

_Marked ready for review by @charliermarsh on 2026-01-05 00:26_

---

_@konstin approved on 2026-01-05 14:31_

Can we document this, either in `--with-requirements` or in some locking docs? Otherwise users will be confused that the lockfile gets updated but not used.

---

_Closed by @charliermarsh on 2026-01-05 14:36_

---
