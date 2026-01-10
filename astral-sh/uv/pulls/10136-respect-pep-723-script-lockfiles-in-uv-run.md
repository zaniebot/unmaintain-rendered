```yaml
number: 10136
title: Respect PEP 723 script lockfiles in uv run
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/script-run
created_at: 2024-12-24T03:01:25Z
updated_at: 2025-01-08T20:34:47Z
url: https://github.com/astral-sh/uv/pull/10136
synced_at: 2026-01-10T11:44:36Z
```

# Respect PEP 723 script lockfiles in uv run

---

_Pull request opened by @charliermarsh on 2024-12-24 03:01_

## Summary

If a script has a lockfile, then `uv run` will now reuse it.

Closes https://github.com/astral-sh/uv/issues/7483.

Closes https://github.com/astral-sh/uv/issues/9688.


---

_Label `enhancement` added by @charliermarsh on 2024-12-24 03:01_

---

_Label `no-build` added by @charliermarsh on 2024-12-24 03:01_

---

_Label `no-test` added by @charliermarsh on 2024-12-24 03:01_

---

_Converted to draft by @charliermarsh on 2024-12-24 03:13_

---

_Label `no-build` removed by @charliermarsh on 2024-12-25 20:25_

---

_Label `no-test` removed by @charliermarsh on 2024-12-25 20:25_

---

_Marked ready for review by @charliermarsh on 2024-12-25 20:25_

---

_Review requested from @zanieb by @charliermarsh on 2024-12-26 20:02_

---

_@zanieb reviewed on 2025-01-08 18:04_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:204 on 2025-01-08 18:04_

Feeling like "run script" should live in it's own function at some point

---

_@zanieb approved on 2025-01-08 18:04_

---

_Merged by @charliermarsh on 2025-01-08 20:34_

---

_Closed by @charliermarsh on 2025-01-08 20:34_

---

_Branch deleted on 2025-01-08 20:34_

---
