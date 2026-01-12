```yaml
number: 15692
title: "Add `check` command"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/check-command
created_at: 2025-01-23T14:13:51Z
updated_at: 2025-01-24T16:00:34Z
url: https://github.com/astral-sh/ruff/pull/15692
synced_at: 2026-01-12T15:55:52Z
```

# Add `check` command

---

_@MichaReiser_

## Summary

This PR changes the CLI to introduce a dedicated `check` command similar to `ruff check` (vs just `red_knot <path>`)

## Test Plan

Updated CLI tests


---

_Review requested from @carljm by @MichaReiser on 2025-01-23 14:13_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-01-23 14:13_

---

_Review requested from @sharkdp by @MichaReiser on 2025-01-23 14:13_

---

_Label `red-knot` added by @MichaReiser on 2025-01-23 14:14_

---

_@MichaReiser reviewed on 2025-01-23 14:14_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:31 on 2025-01-23 14:14_

Some more tidy work. I realised I can use a guard to attach the settings automatically. The only change other than removing the `insta_settings` method is to call `knot check` over just `knot`

---

_Review comment by @dcreager on `crates/red_knot/tests/cli.rs`:31 on 2025-01-23 14:51_

Cool!

---

_@dcreager approved on 2025-01-23 14:51_

---

_@carljm approved on 2025-01-23 19:59_

🎉 

---

_Merged by @MichaReiser on 2025-01-24 16:00_

---

_Closed by @MichaReiser on 2025-01-24 16:00_

---

_Branch deleted on 2025-01-24 16:00_

---
