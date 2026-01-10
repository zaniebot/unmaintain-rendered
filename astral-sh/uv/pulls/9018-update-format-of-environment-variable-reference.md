```yaml
number: 9018
title: Update format of environment variable reference
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/env-vars
created_at: 2024-11-11T15:41:19Z
updated_at: 2024-11-11T18:41:35Z
url: https://github.com/astral-sh/uv/pull/9018
synced_at: 2026-01-10T12:00:00Z
```

# Update format of environment variable reference

---

_Pull request opened by @zanieb on 2024-11-11 15:41_

- Sorts the variables
- Separates `UV_` variables from others
- Uses headings so the toc is available

---

_Label `documentation` added by @zanieb on 2024-11-11 15:41_

---

_Review comment by @j178 on `crates/uv-dev/src/generate_env_vars_reference.rs`:104 on 2024-11-11 16:33_

Should we allow empty lines since we're using paragraphs now?

---

_@j178 reviewed on 2024-11-11 16:33_

---

_@zanieb reviewed on 2024-11-11 17:02_

---

_Review comment by @zanieb on `crates/uv-dev/src/generate_env_vars_reference.rs`:104 on 2024-11-11 17:02_

Ah true, I wasn't sure what the intent of stripping the empty lines was but this makes sense now.

---

_@charliermarsh approved on 2024-11-11 17:54_

---

_Merged by @zanieb on 2024-11-11 18:41_

---

_Closed by @zanieb on 2024-11-11 18:41_

---

_Branch deleted on 2024-11-11 18:41_

---
