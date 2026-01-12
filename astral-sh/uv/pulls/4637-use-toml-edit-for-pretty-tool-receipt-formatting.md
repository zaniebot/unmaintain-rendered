```yaml
number: 4637
title: "Use `toml_edit` for pretty tool receipt formatting"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - preview
assignees: []
merged: true
base: zb/tool-receipt-entry-points
head: zb/tool-receipt-format
created_at: 2024-06-28T21:27:31Z
updated_at: 2024-06-29T03:34:16Z
url: https://github.com/astral-sh/uv/pull/4637
synced_at: 2026-01-12T16:06:21Z
```

# Use `toml_edit` for pretty tool receipt formatting

---

_@zanieb_

I started doing this because I wanted more control over the document but this ends up being useful for https://github.com/astral-sh/uv/pull/4638

I worry about the complexity of this implementation for a largely internal document (unlike the lock file which is very user facing). I think https://github.com/astral-sh/uv/pull/4638 might be better implemented by wrapping `PathBuf`. Then, in the future document could change to JSON with little effort. However, I think that we want tool definitions to be user-facing _eventually_ so I'm unsure this is wasted effort or complexity.

---

_Label `internal` added by @zanieb on 2024-06-28 21:27_

---

_Marked ready for review by @zanieb on 2024-06-28 21:33_

---

_Label `preview` added by @zanieb on 2024-06-28 21:33_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-28 21:58_

---

_Review requested from @BurntSushi by @zanieb on 2024-06-28 21:59_

---

_@charliermarsh approved on 2024-06-28 23:47_

I think it’s fine. We already have the dependency anyway.

---

_Merged by @zanieb on 2024-06-29 03:34_

---

_Closed by @zanieb on 2024-06-29 03:34_

---

_Branch deleted on 2024-06-29 03:34_

---
