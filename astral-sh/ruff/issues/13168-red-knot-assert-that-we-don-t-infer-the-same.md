---
number: 13168
title: "[red-knot] assert that we don't infer the same expression/definition in multiple regions"
type: issue
state: closed
author: carljm
labels:
  - ty
assignees: []
created_at: 2024-08-30T19:23:36Z
updated_at: 2024-11-07T15:20:18Z
url: https://github.com/astral-sh/ruff/issues/13168
synced_at: 2026-01-10T01:22:53Z
---

# [red-knot] assert that we don't infer the same expression/definition in multiple regions

---

_Issue opened by @carljm on 2024-08-30 19:23_

We should not double-infer the same expression (or the same definition) in e.g. both "scope" region and "definition" region.

To check this invariant, we could modify `TypeInferenceBuilder::extend` to check that the sets of keys in the two expression maps and definition maps being combined are disjoint.

This will probably be a bit costly, so we probably want to do it only in a debug build.

---

_Label `red-knot` added by @carljm on 2024-08-30 19:23_

---

_Referenced in [astral-sh/ruff#13267](../../astral-sh/ruff/pulls/13267.md) on 2024-09-08 05:34_

---

_Comment by @carljm on 2024-11-07 15:12_

@MichaReiser I think you recently added an assertion for this and cleaned up the existing issues? Closing as completed, feel free to reopen if you feel there's more we should do here.

---

_Closed by @carljm on 2024-11-07 15:12_

---

_Comment by @MichaReiser on 2024-11-07 15:20_

I added an assertion for expressions. I don't think we currently handle `Definition`s but it's also unclear to me how to detect this. However, we do have unit tests that pull the types of every (or most) definitions... so maybe that's fine for now

---
