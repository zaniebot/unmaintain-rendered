```yaml
number: 13960
title: "Consider `unsafe-fixes` config value for code action / command"
type: issue
state: closed
author: BarrensZeppelin
labels:
  - server
assignees: []
created_at: 2024-10-28T08:52:04Z
updated_at: 2025-01-22T08:14:14Z
url: https://github.com/astral-sh/ruff/issues/13960
synced_at: 2026-01-10T11:09:55Z
```

# Consider `unsafe-fixes` config value for code action / command

---

_Issue opened by @BarrensZeppelin on 2024-10-28 08:52_

It would be nice to be able to configure the ruff server to also apply unsafe fixes with the `ruff.applyAutoFix` command and `source.fixAll.ruff` code action.

---

_Comment by @MichaReiser on 2024-10-28 08:59_

Hi @BarrensZeppelin 

Have you tried https://docs.astral.sh/ruff/settings/#unsafe-fixes and/or https://docs.astral.sh/ruff/settings/#lint_extend-safe-fixes?

---

_Label `server` added by @MichaReiser on 2024-10-28 09:00_

---

_Comment by @BarrensZeppelin on 2024-10-28 09:01_

Unsafe fixes are explicitly disabled here:
https://github.com/astral-sh/ruff/blob/9f3a38d408f473df7a6b3574c5d1389bef303dd5/crates/ruff_server/src/fix.rs#L74

But I guess `extend-safe-fixes` could be a work-around. :thinking: 

---

_Comment by @MichaReiser on 2024-10-28 09:10_

Hmm, good call. I think we should respect the configuration here rather than just outright disabling it.

---

_Renamed from "Add ability to apply unsafe fixes with `ruff.applyAutoFix` server command" to "Consider `unsafe-fixes` config value for code action / command" by @dhruvmanila on 2024-10-28 09:39_

---

_Closed by @dhruvmanila on 2025-01-22 08:14_

---
