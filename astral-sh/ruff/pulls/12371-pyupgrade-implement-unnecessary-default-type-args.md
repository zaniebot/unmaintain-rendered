```yaml
number: 12371
title: "[`pyupgrade`] Implement `unnecessary-default-type-args` (`UP043`)"
type: pull_request
state: merged
author: cake-monotone
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: implement-up043-unnecessary-default-type-args
created_at: 2024-07-17T17:16:55Z
updated_at: 2024-07-17T19:45:43Z
url: https://github.com/astral-sh/ruff/pull/12371
synced_at: 2026-01-10T21:47:02Z
```

# [`pyupgrade`] Implement `unnecessary-default-type-args` (`UP043`)

---

_Pull request opened by @cake-monotone on 2024-07-17 17:16_

## Summary

Add new rule and implement for `unnecessary default type arguments` under the `UP` category (`UP043`).

```py
// < py313
Generator[int, None, None] 

// >= py313
Generator[int]
```

I think that as Python 3.13 develops, there might be more default type arguments added besides `Generator` and `AsyncGenerator`. So, I made this more flexible to accommodate future changes.

related issue: #12286

## Test Plan

snapshot included..!


---

_Renamed from "[`pyupgrade`] Implement `unnecessary default type args` (`UP043`)" to "[`pyupgrade`] Implement `unnecessary-default-type-args` (`UP043`)" by @cake-monotone on 2024-07-17 17:17_

---

_Comment by @charliermarsh on 2024-07-17 18:03_

Nice, thank you! Will review.

---

_Label `rule` added by @charliermarsh on 2024-07-17 18:03_

---

_Label `preview` added by @charliermarsh on 2024-07-17 18:03_

---

_@charliermarsh approved on 2024-07-17 19:39_

This is great, thank you. I just made some minor tweaks to formatting and docs.

---

_Merged by @charliermarsh on 2024-07-17 19:45_

---

_Closed by @charliermarsh on 2024-07-17 19:45_

---
