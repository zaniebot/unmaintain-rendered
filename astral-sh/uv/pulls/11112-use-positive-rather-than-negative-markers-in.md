```yaml
number: 11112
title: Use positive (rather than negative) markers in PyTorch examples
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/pt
created_at: 2025-01-30T18:53:43Z
updated_at: 2025-01-30T19:00:37Z
url: https://github.com/astral-sh/uv/pull/11112
synced_at: 2026-01-12T16:09:41Z
```

# Use positive (rather than negative) markers in PyTorch examples

---

_@charliermarsh_

## Summary

Maybe slightly controversial because it's more verbose, but we really want to limit these indexes to Linux and Windows, rather than ignoring them on Darwin. E.g., we'd also want to ignore them on other platforms.

Further down, I use markers that look like this in the more complete examples, so this feels more consistent.


---

_Label `documentation` added by @charliermarsh on 2025-01-30 18:53_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-30 18:53_

---

_@zanieb approved on 2025-01-30 18:55_

Clearer to me

---

_Merged by @charliermarsh on 2025-01-30 19:00_

---

_Closed by @charliermarsh on 2025-01-30 19:00_

---

_Branch deleted on 2025-01-30 19:00_

---
