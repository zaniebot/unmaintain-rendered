```yaml
number: 15037
title: "[`ruff`] Avoid false positives for RUF027 for typing context bindings."
type: pull_request
state: merged
author: Daverball
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: bugfix/ruf027-typing-context
created_at: 2024-12-17T13:39:04Z
updated_at: 2024-12-18T08:30:54Z
url: https://github.com/astral-sh/ruff/pull/15037
synced_at: 2026-01-10T20:42:27Z
```

# [`ruff`] Avoid false positives for RUF027 for typing context bindings.

---

_Pull request opened by @Daverball on 2024-12-17 13:39_

Closes #14000 

## Summary

For typing context bindings we know that they won't be available at runtime. We shouldn't recommend a fix, that will result in name errors at runtime.

## Test Plan

`cargo nextest run`


---

_Comment by @github-actions[bot] on 2024-12-17 13:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:220 on 2024-12-17 15:59_

I think I have a slight preference to adding an extra method here that takes a string and range over creating a fake AST
```suggestion
                    .simulate_runtime_load_str(&id, literal.range())
```

---

_@MichaReiser approved on 2024-12-17 15:59_

Nice

---

_Label `rule` added by @MichaReiser on 2024-12-17 16:00_

---

_Label `preview` added by @MichaReiser on 2024-12-17 16:00_

---

_@Daverball reviewed on 2024-12-17 16:23_

---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/ruff/rules/missing_fstring_syntax.rs`:220 on 2024-12-17 16:23_

I considered that as well. My most favorite solution would be to extend `Parser` with an optional static offset, so the parsed nodes get created with the correct offset.

---

_@MichaReiser approved on 2024-12-18 07:49_

Thanks

---

_Merged by @MichaReiser on 2024-12-18 07:50_

---

_Closed by @MichaReiser on 2024-12-18 07:50_

---

_Branch deleted on 2024-12-18 08:30_

---
