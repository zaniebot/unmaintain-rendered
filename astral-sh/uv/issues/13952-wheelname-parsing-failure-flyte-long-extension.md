```yaml
number: 13952
title: "`wheelname_parsing_failure[flyte-long-extension]` benchmark flakes"
type: issue
state: closed
author: zanieb
labels:
  - good first issue
  - ci-flake
assignees: []
created_at: 2025-06-10T16:54:55Z
updated_at: 2025-06-24T15:54:14Z
url: https://github.com/astral-sh/uv/issues/13952
synced_at: 2026-01-12T16:01:40Z
```

# `wheelname_parsing_failure[flyte-long-extension]` benchmark flakes

---

_@zanieb_

These benchmarks are all over the place and result in frequent false positives and negatives. We should do something about these.

---

_Label `ci-flake` added by @zanieb on 2025-06-10 16:54_

---

_Comment by @konstin on 2025-06-10 16:55_

CC @BurntSushi

---

_Comment by @zanieb on 2025-06-10 16:56_

Both in the instrumented and walltime benchmarks. A latest example is at 

https://github.com/astral-sh/uv/pull/13948#issuecomment-2959984499

---

_Comment by @charliermarsh on 2025-06-10 17:02_

I would be ok removing them from CI.

---

_Comment by @zanieb on 2025-06-11 13:44_

Let's disable these for now then.

---

_Label `good first issue` added by @zanieb on 2025-06-11 13:44_

---

_Comment by @numanijaz on 2025-06-14 14:10_

Hi, can I work on it?

---

_Comment by @charliermarsh on 2025-06-20 20:17_

Go for it.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-06-24 13:44_

---

_Closed by @charliermarsh on 2025-06-24 15:54_

---
