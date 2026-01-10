```yaml
number: 17152
title: Make pyx credential error message reflect the realm
type: pull_request
state: open
author: zsol
labels: []
assignees: []
draft: true
base: main
head: zsol/jj-zmwropzqtzxm
created_at: 2025-12-16T19:24:42Z
updated_at: 2025-12-16T19:25:20Z
url: https://github.com/astral-sh/uv/pull/17152
synced_at: 2026-01-10T05:49:14Z
```

# Make pyx credential error message reflect the realm

---

_Pull request opened by @zsol on 2025-12-16 19:24_

This is unlikely to affect people who aren't pyx devs.

## Test Plan

`PYX_API_URL=http://localhost:8000 uv pip install --default-index $PYX_API_URL/simple foo` yields

```
Run `uv auth login http://localhost:8000` to authenticate uv with pyx
```

But the error message against production pyx.dev remains the same.


---

_Comment by @charliermarsh on 2025-12-16 19:25_

Should we include `PYX_API_URL` in the error message?

---
