```yaml
number: 11110
title: Remove unnecessary UTF-8 conversion in hash parsing
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/enc
created_at: 2025-01-30T18:46:35Z
updated_at: 2025-01-30T18:55:48Z
url: https://github.com/astral-sh/uv/pull/11110
synced_at: 2026-01-10T11:10:34Z
```

# Remove unnecessary UTF-8 conversion in hash parsing

---

_Pull request opened by @charliermarsh on 2025-01-30 18:46_

## Summary

I believe this is a no-op?

---

_Review requested from @BurntSushi by @charliermarsh on 2025-01-30 18:46_

---

_Label `performance` added by @charliermarsh on 2025-01-30 18:46_

---

_Comment by @charliermarsh on 2025-01-30 18:49_

It looks like this has been here since the beginning: https://github.com/astral-sh/uv/pull/719/files#diff-2295d6bb5754521711b3fadff36e1a0eb151bcea2f4f084b78d553b7b7e5a0dfR76-R101. I'm not sure why I did it that way. It must've been an oversight.

---

_@zanieb approved on 2025-01-30 18:52_

---

_@BurntSushi approved on 2025-01-30 18:53_

I'm surprised there isn't a Clippy lint for this haha.

---

_Merged by @charliermarsh on 2025-01-30 18:55_

---

_Closed by @charliermarsh on 2025-01-30 18:55_

---

_Branch deleted on 2025-01-30 18:55_

---
