```yaml
number: 9069
title: "Rename `src` directory to `sdist`"
type: pull_request
state: closed
author: charliermarsh
labels:
  - compatibility
assignees: []
base: main
head: charlie/sdist
created_at: 2024-11-12T19:18:11Z
updated_at: 2024-12-02T02:16:42Z
url: https://github.com/astral-sh/uv/pull/9069
synced_at: 2026-01-12T16:08:38Z
```

# Rename `src` directory to `sdist`

---

_@charliermarsh_

## Summary

It seems like XGBoost looks at `../src` and so it's causing problems for us to use that name as the build directory. We can't really dodge these problems in general -- it seems like it might be an XGBoost issue? -- but it's easy for us to change it to something that's less likely to collide.

Closes https://github.com/astral-sh/uv/issues/9068.


---

_Label `compatibility` added by @charliermarsh on 2024-11-12 19:18_

---

_@zanieb approved on 2024-11-12 19:20_

---

_Closed by @charliermarsh on 2024-12-02 02:16_

---
