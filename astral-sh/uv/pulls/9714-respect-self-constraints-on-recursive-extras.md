```yaml
number: 9714
title: Respect self-constraints on recursive extras
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/rec-constraint
created_at: 2024-12-08T04:16:29Z
updated_at: 2024-12-08T04:53:43Z
url: https://github.com/astral-sh/uv/pull/9714
synced_at: 2026-01-12T16:08:57Z
```

# Respect self-constraints on recursive extras

---

_@charliermarsh_

## Summary

Sort of ridiculous, but today this passes, when it should fail:

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.0"
dependencies = []

[project.optional-dependencies]
async = [
    "foo[async]==0.2.0",
]
```


---

_Label `bug` added by @charliermarsh on 2024-12-08 04:16_

---

_Review requested from @konstin by @charliermarsh on 2024-12-08 04:27_

---

_Merged by @charliermarsh on 2024-12-08 04:53_

---

_Closed by @charliermarsh on 2024-12-08 04:53_

---

_Branch deleted on 2024-12-08 04:53_

---
