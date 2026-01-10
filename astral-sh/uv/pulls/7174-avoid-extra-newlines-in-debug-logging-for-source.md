```yaml
number: 7174
title: Avoid extra newlines in debug logging for source builds
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - tracing
assignees: []
merged: true
base: main
head: charlie/o
created_at: 2024-09-07T18:09:03Z
updated_at: 2024-09-07T18:34:02Z
url: https://github.com/astral-sh/uv/pull/7174
synced_at: 2026-01-10T12:53:41Z
```

# Avoid extra newlines in debug logging for source builds

---

_Pull request opened by @charliermarsh on 2024-09-07 18:09_

## Summary

@henryiii brought this up and I noticed it too:

![Screenshot 2024-09-06 at 4 09 32 PM](https://github.com/user-attachments/assets/a2849a0e-0515-4856-a9fe-14c713ed9b75)

## Test Plan

`uv build`:

![Screenshot 2024-09-07 at 2 08 39 PM](https://github.com/user-attachments/assets/841db9b7-1fd1-4964-aa57-58b479a255ff)

`uv pip install -e . --verbose`:

![Screenshot 2024-09-07 at 2 08 52 PM](https://github.com/user-attachments/assets/30b2817a-d4a3-4437-b47d-2bc037f5afa4)


---

_Label `bug` added by @charliermarsh on 2024-09-07 18:09_

---

_Label `tracing` added by @charliermarsh on 2024-09-07 18:09_

---

_Merged by @charliermarsh on 2024-09-07 18:34_

---

_Closed by @charliermarsh on 2024-09-07 18:34_

---

_Branch deleted on 2024-09-07 18:34_

---
