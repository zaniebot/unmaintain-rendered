```yaml
number: 10207
title: "Allow `--constraints` and `--overrides` in `uvx`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/uvx-constraints
created_at: 2024-12-27T19:42:04Z
updated_at: 2025-03-07T22:55:11Z
url: https://github.com/astral-sh/uv/pull/10207
synced_at: 2026-01-10T11:10:34Z
```

# Allow `--constraints` and `--overrides` in `uvx`

---

_Pull request opened by @charliermarsh on 2024-12-27 19:42_

## Summary

Closes https://github.com/astral-sh/uv/issues/9813.


---

_Label `enhancement` added by @charliermarsh on 2024-12-27 19:42_

---

_@charliermarsh reviewed on 2024-12-27 19:44_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:750 on 2024-12-27 19:44_

I don't know if this is a good change. Previously, we just checked if the _environment_ was compatible with the request. Now, we check if the _receipt_ contained the exact same requested dependencies. This is consistent with `uv tool install`, but it's also "stricter".

---

_Marked ready for review by @charliermarsh on 2024-12-27 19:46_

---

_Review requested from @zanieb by @charliermarsh on 2025-03-03 03:53_

---

_@zanieb approved on 2025-03-03 19:06_

Ah I wanted this just the other day

---

_@charliermarsh reviewed on 2025-03-03 21:13_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:750 on 2025-03-03 21:13_

@zanieb -- Did you have an opinion on this? Or too hard to say without more context?


---

_@zanieb reviewed on 2025-03-03 21:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:750 on 2025-03-03 21:19_

I didn't think about it too hard, but I think it's basically okay. What motivates the change?

---

_@charliermarsh reviewed on 2025-03-04 01:48_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:750 on 2025-03-04 01:48_

We can't use `satisfies` with `overrides`, so we'd need to two branches.

---

_@zanieb reviewed on 2025-03-04 01:57_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:750 on 2025-03-04 01:57_

Hm, why can' we use `satisfies` with `overrides`? Aren't they just equivalent to constraints w.r.t. environments?

---

_@charliermarsh reviewed on 2025-03-04 02:01_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:750 on 2025-03-04 02:01_

No, but we had an extensive conversation about this in the past... I'd have to dig it up.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/run.rs`:750 on 2025-03-04 02:10_

I'll look at it separately. The current API definitely doesn't support it though and I think I determined that it was non-trivial when we tried in the past. (Note that everywhere that we use `satisfies`, we skip if `overrides` is non-empty.)

---

_@charliermarsh reviewed on 2025-03-04 02:10_

---

_Merged by @charliermarsh on 2025-03-04 02:18_

---

_Closed by @charliermarsh on 2025-03-04 02:18_

---

_Branch deleted on 2025-03-04 02:18_

---

_@Parsa5242 approved on 2025-03-07 22:55_

---
