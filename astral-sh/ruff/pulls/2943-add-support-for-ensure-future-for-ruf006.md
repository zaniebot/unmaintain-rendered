```yaml
number: 2943
title: "Add support for `ensure_future` for RUF006"
type: pull_request
state: merged
author: zunda-arrow
labels: []
assignees: []
merged: true
base: main
head: ensure-future-ruf006
created_at: 2023-02-15T21:56:09Z
updated_at: 2023-02-15T23:18:22Z
url: https://github.com/astral-sh/ruff/pull/2943
synced_at: 2026-01-12T15:55:12Z
```

# Add support for `ensure_future` for RUF006

---

_@zunda-arrow_

apologies if I did the snapshots wrong lol. This is my first time using the tool.

closes #2941 

---

_@charliermarsh reviewed on 2023-02-15 22:20_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/asyncio_dangling_task.rs`:59 on 2023-02-15 22:20_

Let's tailor this message based on the function call. If you take a look at the `DeferralKeyword` enum in `crates/ruff/src/rules/pyflakes/rules/yield_outside_function.rs`, we can do the same thing here: add it as a member on the rule struct, then include it in the format message.

---

_Review comment by @zunda-arrow on `crates/ruff/src/rules/ruff/rules/asyncio_dangling_task.rs`:59 on 2023-02-15 22:51_

Change is made! :smile: 

---

_@zunda-arrow reviewed on 2023-02-15 22:51_

---

_Comment by @zunda-arrow on 2023-02-15 22:57_

Is there anything special I have to do to get both keywords to appear properly in the README?

---

_Comment by @charliermarsh on 2023-02-15 23:13_

Na, the README only shows the message _format_, not the realized version with the placeholders filled in. So this is by design, even if somewhat suboptimal for certain rules.

---

_@charliermarsh reviewed on 2023-02-15 23:16_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/asyncio_dangling_task.rs`:68 on 2023-02-15 23:16_

I just renamed this to something more appropriate for the specific check.

---

_Merged by @charliermarsh on 2023-02-15 23:18_

---

_Closed by @charliermarsh on 2023-02-15 23:18_

---

_Branch deleted on 2023-02-15 23:18_

---
