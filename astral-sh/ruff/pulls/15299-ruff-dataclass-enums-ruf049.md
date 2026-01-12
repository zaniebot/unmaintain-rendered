```yaml
number: 15299
title: "[`ruff`] Dataclass enums (`RUF049`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: RUF049
created_at: 2025-01-06T11:51:21Z
updated_at: 2025-01-06T13:44:45Z
url: https://github.com/astral-sh/ruff/pull/15299
synced_at: 2026-01-12T15:55:50Z
```

# [`ruff`] Dataclass enums (`RUF049`)

---

_@InSyncWithFoo_

## Summary

Resolves #15275.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-06 11:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/dataclass_enum.rs`:76 on 2025-01-06 12:47_

I'm not sure if we should provide a fix here and instead ask the user to remove the decorator manually (or maybe change the applicability to manual). It's mainly because users should think about whether the class should be a dataclass or an enum, it just can't be both and they've to decide. 

---

_@MichaReiser approved on 2025-01-06 13:03_

Thanks. This looks good to me. The only thing I feel unsure about is if we should provide an automatic fix. This feels like the kind of change that has too many implication for it to be correct automatically (maybe it shouldn't even be an enum?)

---

_Label `rule` added by @MichaReiser on 2025-01-06 13:14_

---

_Label `preview` added by @MichaReiser on 2025-01-06 13:14_

---

_@InSyncWithFoo reviewed on 2025-01-06 13:37_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/dataclass_enum.rs`:76 on 2025-01-06 13:37_

Fix removed. The reported range remain the decorator's, though, as I think `@dataclass` is more likely to be erroneous.

---

_Merged by @MichaReiser on 2025-01-06 13:44_

---

_Closed by @MichaReiser on 2025-01-06 13:44_

---

_Branch deleted on 2025-01-06 13:44_

---
