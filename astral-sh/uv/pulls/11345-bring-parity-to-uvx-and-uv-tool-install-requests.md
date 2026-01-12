```yaml
number: 11345
title: "Bring parity to `uvx` and `uv tool install` requests"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/refactor-target
created_at: 2025-02-09T00:18:43Z
updated_at: 2025-02-12T00:16:35Z
url: https://github.com/astral-sh/uv/pull/11345
synced_at: 2026-01-12T16:09:48Z
```

# Bring parity to `uvx` and `uv tool install` requests

---

_@charliermarsh_

## Summary

This PR refactors the whole `Target` abstraction, mostly to remove all the repeated `From*` variants and logic in favor of a higher-level struct that captures those details separately.

In doing so, it also adds support to `uvx` for unnamed requirements, for parity with `uv tool install`. So, e.g., the following will show the `flask` version:

```
uvx git+https://github.com/pallets/flask --version
```

I think this makes sense conceptually since we already support arbitrary named requirements there.


---

_Label `enhancement` added by @charliermarsh on 2025-02-09 00:18_

---

_@zanieb approved on 2025-02-11 22:21_

---

_Merged by @charliermarsh on 2025-02-12 00:16_

---

_Closed by @charliermarsh on 2025-02-12 00:16_

---

_Branch deleted on 2025-02-12 00:16_

---
