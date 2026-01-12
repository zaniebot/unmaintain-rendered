```yaml
number: 7542
title: "Make test suite robust against `UV_CACHE_DIR`"
type: issue
state: closed
author: konstin
labels:
  - good first issue
  - help wanted
  - internal
assignees: []
created_at: 2024-09-19T11:26:57Z
updated_at: 2024-10-04T14:41:26Z
url: https://github.com/astral-sh/uv/issues/7542
synced_at: 2026-01-12T15:59:14Z
```

# Make test suite robust against `UV_CACHE_DIR`

---

_@konstin_

Currently, the help tests fail when `UV_CACHE_DIR` is set (https://github.com/astral-sh/uv/actions/runs/10939768235/job/30370759126?pr=7540). We should unset `UV_CACHE_DIR` for the commands those tests run (other tests override the value). Then we can migrate the two remaining usage of `curl -LsSf https://astral.sh/uv/install.sh | sh` in `ci.yml` to https://github.com/astral-sh/setup-uv.

---

_Label `good first issue` added by @konstin on 2024-09-19 11:26_

---

_Label `help wanted` added by @konstin on 2024-09-19 11:26_

---

_Label `internal` added by @konstin on 2024-09-19 11:26_

---

_Closed by @zanieb on 2024-10-04 14:41_

---
