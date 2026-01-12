```yaml
number: 17014
title: "`uv sync` should update environment `PROMPT` when `[project].name` changes"
type: issue
state: open
author: Saltsmart
labels:
  - enhancement
assignees: []
created_at: 2025-12-06T05:58:50Z
updated_at: 2025-12-08T12:21:38Z
url: https://github.com/astral-sh/uv/issues/17014
synced_at: 2026-01-12T16:02:42Z
```

# `uv sync` should update environment `PROMPT` when `[project].name` changes

---

_@Saltsmart_

### Summary

If `[project].name` is wrong in pyproject.toml, we have to delete the `.venv` folder, correct the name, and then run `uv sync` again to create another environment.

Could `uv sync` rename environment if `[project].name` has been changed?

### Example

_No response_

---

_Label `enhancement` added by @Saltsmart on 2025-12-06 05:58_

---

_Comment by @zanieb on 2025-12-06 13:54_

Are you referring to the name shown in the terminal PROMPT?

---

_Comment by @Saltsmart on 2025-12-07 07:54_

> Are you referring to the name shown in the terminal PROMPT?

Yes, it is defined in multiple shell files so not easy to be corrected.

---

_Renamed from "`uv sync` renames environment if `[project].name` has been changed" to "`uv sync` should update environment `PROMPT` when `[project].name` changes" by @zanieb on 2025-12-08 12:21_

---
