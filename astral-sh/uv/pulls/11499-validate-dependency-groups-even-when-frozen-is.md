```yaml
number: 11499
title: "Validate dependency groups even when `--frozen` is present"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/frozen
created_at: 2025-02-14T00:14:57Z
updated_at: 2025-02-14T15:54:31Z
url: https://github.com/astral-sh/uv/pull/11499
synced_at: 2026-01-10T11:10:38Z
```

# Validate dependency groups even when `--frozen` is present

---

_Pull request opened by @charliermarsh on 2025-02-14 00:14_

## Summary

We now use the same strategy as for extras, validating against the lockfile instead of the `pyproject.toml`.

Closes https://github.com/astral-sh/uv/issues/10882.


---

_Label `enhancement` added by @charliermarsh on 2025-02-14 00:15_

---

_Comment by @charliermarsh on 2025-02-14 00:16_

(@zanieb -- We could include this in v0.6, but it's not totally necessary.)

---

_Marked ready for review by @charliermarsh on 2025-02-14 00:16_

---

_Comment by @zanieb on 2025-02-14 02:37_

Is there a test case requesting a missing group with `--frozen`?

---

_Comment by @charliermarsh on 2025-02-14 02:38_

Oh, I guess not, thanks. I was mostly focused on getting the existing suite passing.

---

_@zanieb approved on 2025-02-14 02:46_

---

_Comment by @charliermarsh on 2025-02-14 02:53_

@zanieb -- I'll let you merge based on whether you want to include in v0.6.

---

_Merged by @zanieb on 2025-02-14 15:54_

---

_Closed by @zanieb on 2025-02-14 15:54_

---

_Branch deleted on 2025-02-14 15:54_

---
