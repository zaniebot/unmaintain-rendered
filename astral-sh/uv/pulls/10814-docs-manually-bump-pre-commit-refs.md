```yaml
number: 10814
title: "docs: manually bump pre-commit refs"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/manually-bump-pre-commit-refs
created_at: 2025-01-21T13:18:53Z
updated_at: 2025-01-21T14:33:38Z
url: https://github.com/astral-sh/uv/pull/10814
synced_at: 2026-01-12T16:09:30Z
```

# docs: manually bump pre-commit refs

---

_@mkniewallner_

## Summary

rooster should pick those up, but those 2 references were added to 0.5.8 while a 0.5.9 was already released (https://github.com/astral-sh/uv/commit/f5add0ca5e2711396e033741375049d0454730f4), so they never got bumped automatically.

I've searched for other cases like this in the documentation on other versions, just in case, and it seems that this is the only case.

---

_@charliermarsh approved on 2025-01-21 14:28_

Thanks

---

_Label `documentation` added by @charliermarsh on 2025-01-21 14:28_

---

_Merged by @charliermarsh on 2025-01-21 14:28_

---

_Closed by @charliermarsh on 2025-01-21 14:28_

---

_Branch deleted on 2025-01-21 14:33_

---
