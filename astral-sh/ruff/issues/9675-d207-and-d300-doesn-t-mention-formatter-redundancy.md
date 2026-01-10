```yaml
number: 9675
title: "D207 and D300 doesn't mention formatter redundancy"
type: issue
state: closed
author: trag1c
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2024-01-29T15:06:52Z
updated_at: 2024-12-28T23:45:02Z
url: https://github.com/astral-sh/ruff/issues/9675
synced_at: 2026-01-10T11:09:51Z
```

# D207 and D300 doesn't mention formatter redundancy

---

_Issue opened by @trag1c on 2024-01-29 15:06_

Both [D207](https://docs.astral.sh/ruff/rules/under-indentation/) and [D208](https://docs.astral.sh/ruff/rules/over-indentation/) seem to be affected by the formatter, yet only D208 documents that behavior. Are there any cases in which D208 violations won't get formatted correctly or is this just an oversight?

---

_Comment by @zanieb on 2024-01-29 15:30_

It looks like an oversight to me; thanks!

---

_Label `documentation` added by @zanieb on 2024-01-29 15:30_

---

_Label `good first issue` added by @zanieb on 2024-01-29 15:30_

---

_Comment by @trag1c on 2024-01-29 16:36_

Looks like it also applies to [D300](https://docs.astral.sh/ruff/rules/triple-single-quotes/).

---

_Renamed from "D207 doesn't mention formatter redundancy" to "D207 and D300 doesn't mention formatter redundancy" by @trag1c on 2024-01-29 16:36_

---

_Comment by @takaya0 on 2024-02-01 12:48_

I'd like to try this issue. Could you assign me to fix this issue?

---

_Closed by @charliermarsh on 2024-02-17 12:37_

---

_Comment by @Avasam on 2024-12-28 23:43_

Are you sure D300 applies? The playground even with `preview=true` does not format those docstring back to triple-quoting: https://play.ruff.rs/c41e8485-df53-4dbd-a738-274ffc71c6fe?secondary=Format

---
