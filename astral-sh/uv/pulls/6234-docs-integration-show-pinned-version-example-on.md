```yaml
number: 6234
title: "docs(integration): show pinned version example on GH Actions"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: docs/pinned-uv-example-gh-actions
created_at: 2024-08-19T21:56:21Z
updated_at: 2024-08-19T22:12:49Z
url: https://github.com/astral-sh/uv/pull/6234
synced_at: 2026-01-10T13:09:51Z
```

# docs(integration): show pinned version example on GH Actions

---

_Pull request opened by @mkniewallner on 2024-08-19 21:56_

## Summary

Suggestion from https://github.com/astral-sh/uv/pull/6216#discussion_r1722204667.

I did not think of a clean way to avoid repetition, so tried to use tabs for the platforms to only show the pin recommendation in one additional block.

![Screenshot from 2024-08-19 23-54-36](https://github.com/user-attachments/assets/8a870c68-da60-460a-8bda-4afb72d16b86)

## Test Plan

Local run of the documentation.

---

_Marked ready for review by @mkniewallner on 2024-08-19 21:57_

---

_@mkniewallner reviewed on 2024-08-19 22:02_

---

_Review comment by @mkniewallner on `pyproject.toml`:71 on 2024-08-19 22:02_

Not sure of the heuristic used by `rooster` to find references to uv versions, so not sure if this will actually work.

---

_@zanieb reviewed on 2024-08-19 22:07_

---

_Review comment by @zanieb on `pyproject.toml`:71 on 2024-08-19 22:07_

Yeah this will work, it replaces the last version number with the new version number — that's all. Thank you!

---

_@zanieb approved on 2024-08-19 22:11_

Thanks!

---

_Label `documentation` added by @zanieb on 2024-08-19 22:12_

---

_Label `preview` added by @zanieb on 2024-08-19 22:12_

---

_Merged by @zanieb on 2024-08-19 22:12_

---

_Closed by @zanieb on 2024-08-19 22:12_

---

_Branch deleted on 2024-08-19 22:12_

---
