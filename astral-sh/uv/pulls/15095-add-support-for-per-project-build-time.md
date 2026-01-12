```yaml
number: 15095
title: Add support for per-project build-time environment variables
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/env-var
created_at: 2025-08-05T23:13:25Z
updated_at: 2025-08-06T23:01:57Z
url: https://github.com/astral-sh/uv/pull/15095
synced_at: 2026-01-12T16:11:34Z
```

# Add support for per-project build-time environment variables

---

_@charliermarsh_

## Summary

E.g., you can now do:

```toml
[tool.uv.extra-build-variables]
flash-attn = { FLASH_ATTENTION_SKIP_CUDA_BUILD = "TRUE" }
```


---

_Label `configuration` added by @charliermarsh on 2025-08-05 23:24_

---

_Review requested from @zanieb by @charliermarsh on 2025-08-05 23:29_

---

_Marked ready for review by @charliermarsh on 2025-08-05 23:29_

---

_@zanieb approved on 2025-08-06 23:01_

Fun test case haha seems a little silly to be asserting the version and not just the presence of the variable but 🤷‍♀️ nbd

---

_Merged by @zanieb on 2025-08-06 23:01_

---

_Closed by @zanieb on 2025-08-06 23:01_

---

_Branch deleted on 2025-08-06 23:01_

---
