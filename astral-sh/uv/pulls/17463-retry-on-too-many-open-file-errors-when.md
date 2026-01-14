```yaml
number: 17463
title: "Retry on \"too many open file\" errors when uninstalling Python"
type: pull_request
state: open
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: main
head: claude/retry-open-files-error-o5T7E
created_at: 2026-01-14T14:15:43Z
updated_at: 2026-01-14T14:32:00Z
url: https://github.com/astral-sh/uv/pull/17463
synced_at: 2026-01-14T14:41:38Z
```

# Retry on "too many open file" errors when uninstalling Python

---

_@zanieb_

_No description provided._

---

_Label `bug` added by @zanieb on 2026-01-14 14:15_

---

_Comment by @konstin on 2026-01-14 14:20_

Can we try instead increasing the ulimit (https://github.com/astral-sh/uv/issues/16999#issuecomment-3641417484)? There's multiple locations where we could run into these kinds of errors, a higher limits avoids them occurring in the first place and is also more performant.

---

_Comment by @zanieb on 2026-01-14 14:23_

What about both? I think adjusting the ulimit is scarier and I'd want to gate that with preview to start. This feels safe, though it can be less performant in the bad case.

---
