```yaml
number: 14935
title: Add tracing support to mdtest
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/mdtest-tracing
created_at: 2024-12-12T13:19:03Z
updated_at: 2024-12-13T09:12:59Z
url: https://github.com/astral-sh/ruff/pull/14935
synced_at: 2026-01-12T15:55:49Z
```

# Add tracing support to mdtest

---

_@MichaReiser_

## Summary

This PR extends the mdtest configuration with a `log` setting that can be any of:

* `true`: Enables tracing
* `false`: Disables tracing (default)
* String: An ENV_FILTER similar to `RED_KNOT_LOG`

```toml
log = true
```

Closes https://github.com/astral-sh/ruff/issues/13865

## Test Plan

I changed a test and tried `log=true`, `log=false`, and `log=INFO`


---

_Review requested from @carljm by @MichaReiser on 2024-12-12 13:19_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-12 13:19_

---

_Review requested from @sharkdp by @MichaReiser on 2024-12-12 13:19_

---

_Label `red-knot` added by @MichaReiser on 2024-12-12 13:19_

---

_@AlexWaygood reviewed on 2024-12-12 13:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/config.rs`:9 on 2024-12-12 13:23_

should we also add this to the example at https://github.com/astral-sh/ruff/tree/main/crates/red_knot_test#configuration ?

---

_@MichaReiser reviewed on 2024-12-12 13:31_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/config.rs`:9 on 2024-12-12 13:31_

I added a link to the `MarkdownTestConfig`. I'd prefer not having to list all options to avoid repeating the same documentation in multiple places.

---

_Comment by @github-actions[bot] on 2024-12-12 13:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-12-12 17:51_

Nice, thank you!!

---

_Merged by @MichaReiser on 2024-12-13 09:10_

---

_Closed by @MichaReiser on 2024-12-13 09:10_

---

_Branch deleted on 2024-12-13 09:10_

---
