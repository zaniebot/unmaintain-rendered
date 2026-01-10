```yaml
number: 1025
title: Treat missing package name error as an unsupported requirement
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/gs
created_at: 2024-01-20T18:11:57Z
updated_at: 2024-01-22T00:53:11Z
url: https://github.com/astral-sh/uv/pull/1025
synced_at: 2026-01-10T15:39:03Z
```

# Treat missing package name error as an unsupported requirement

---

_Pull request opened by @charliermarsh on 2024-01-20 18:11_

## Summary

Based on user feedback. Calling it a "parse error" is misleading, since this is really something we don't support, but that users can work around.


---

_Label `error messages` added by @charliermarsh on 2024-01-20 18:12_

---

_@zanieb reviewed on 2024-01-21 23:30_

---

_Review comment by @zanieb on `crates/pep508-rs/src/lib.rs`:850 on 2024-01-21 23:30_

Maybe "Add the name of the package before the URL" instead of "Prepend the specifier with a package name"?

---

_@charliermarsh reviewed on 2024-01-22 00:15_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/lib.rs`:850 on 2024-01-22 00:15_

Will do.

---

_Merged by @charliermarsh on 2024-01-22 00:53_

---

_Closed by @charliermarsh on 2024-01-22 00:53_

---

_Branch deleted on 2024-01-22 00:53_

---
