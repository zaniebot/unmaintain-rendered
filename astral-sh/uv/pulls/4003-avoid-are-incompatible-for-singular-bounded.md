```yaml
number: 4003
title: "Avoid 'are incompatible' for singular bounded versions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/is
created_at: 2024-06-03T23:32:51Z
updated_at: 2024-06-04T19:17:37Z
url: https://github.com/astral-sh/uv/pull/4003
synced_at: 2026-01-10T13:54:02Z
```

# Avoid 'are incompatible' for singular bounded versions

---

_Pull request opened by @charliermarsh on 2024-06-03 23:32_

## Summary

Not sure if this is worth the complexity, but it does read better.

---

_Label `error messages` added by @charliermarsh on 2024-06-03 23:32_

---

_Marked ready for review by @charliermarsh on 2024-06-04 00:03_

---

_@zanieb approved on 2024-06-04 13:47_

Seems okay. We have similar complexity for singular and plural cases of `PackageRange`.

The logic in `PackageRange`  looks a bit different, can you make sure these implementations are consistent?

https://github.com/astral-sh/uv/blob/35d07142d9f694e81491430b080d3f4e9f680cd8/crates/uv-resolver/src/pubgrub/report.rs#L786-L794

---

_Comment by @charliermarsh on 2024-06-04 13:49_

Will do, thanks.

---

_Comment by @charliermarsh on 2024-06-04 19:12_

Thanks, I decided to reuse `is_plural`.

---

_@zanieb approved on 2024-06-04 19:14_

nice

---

_Merged by @charliermarsh on 2024-06-04 19:17_

---

_Closed by @charliermarsh on 2024-06-04 19:17_

---

_Branch deleted on 2024-06-04 19:17_

---
