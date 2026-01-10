```yaml
number: 9777
title: Multiline f-strings are not indexed as multiline strings
type: issue
state: closed
author: sanxiyn
labels:
  - bug
assignees: []
created_at: 2024-02-02T08:05:07Z
updated_at: 2024-02-06T02:25:35Z
url: https://github.com/astral-sh/ruff/issues/9777
synced_at: 2026-01-10T11:09:51Z
```

# Multiline f-strings are not indexed as multiline strings

---

_Issue opened by @sanxiyn on 2024-02-02 08:05_

Tracking leftover work from #9744. Removing trailing whitespace inside multiline strings is unsafe, so multiline strings are indexed. But multiline f-strings are currently not indexed as multiline strings.

---

_Comment by @dhruvmanila on 2024-02-02 11:13_

Thanks for the issue! Are you interested in taking on the work required to complete this as you commented on the linked PR?

---

_Label `bug` added by @dhruvmanila on 2024-02-02 11:13_

---

_Comment by @sanxiyn on 2024-02-02 11:39_

Yes, I want to try.

---

_Assigned to @sanxiyn by @dhruvmanila on 2024-02-02 11:52_

---

_Comment by @dhruvmanila on 2024-02-02 11:52_

Awesome! Feel free to ping me if you run into any issues :)

---

_Closed by @charliermarsh on 2024-02-06 02:25_

---
