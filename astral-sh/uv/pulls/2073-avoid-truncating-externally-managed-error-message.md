```yaml
number: 2073
title: Avoid truncating EXTERNALLY-MANAGED error message
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/externally-managed
created_at: 2024-02-29T02:49:05Z
updated_at: 2024-02-29T03:00:27Z
url: https://github.com/astral-sh/uv/pull/2073
synced_at: 2026-01-12T16:04:51Z
```

# Avoid truncating EXTERNALLY-MANAGED error message

---

_@charliermarsh_

## Summary

This is still imperfect, since the INI parser seems to strip empty lines, but at least the content is preserved.

Closes https://github.com/astral-sh/uv/issues/2061.

## Test Plan

![Screenshot 2024-02-28 at 9 48 24 PM](https://github.com/astral-sh/uv/assets/1309177/66224e94-0500-4634-83cb-33981443b8a3)


---

_Marked ready for review by @charliermarsh on 2024-02-29 03:00_

---

_Merged by @charliermarsh on 2024-02-29 03:00_

---

_Closed by @charliermarsh on 2024-02-29 03:00_

---

_Branch deleted on 2024-02-29 03:00_

---

_Label `bug` added by @charliermarsh on 2024-02-29 03:00_

---
