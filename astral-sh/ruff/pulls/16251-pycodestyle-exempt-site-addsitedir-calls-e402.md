```yaml
number: 16251
title: "[pycodestyle] Exempt `site.addsitedir(...)` calls (E402)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - rule
assignees: []
merged: true
base: main
head: add-exception-E402
created_at: 2025-02-19T12:31:07Z
updated_at: 2025-02-19T13:34:30Z
url: https://github.com/astral-sh/ruff/pull/16251
synced_at: 2026-01-12T15:55:54Z
```

# [pycodestyle] Exempt `site.addsitedir(...)` calls (E402)

---

_@VascoSch92_

The PR addresses issue #16247 

---

_Comment by @github-actions[bot] on 2025-02-19 12:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @MichaReiser on 2025-02-19 13:17_

---

_@MichaReiser reviewed on 2025-02-19 13:19_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:560 on 2025-02-19 13:19_

Nit: Maybe `is_site_sys_path_modification`?

---

_@MichaReiser approved on 2025-02-19 13:19_

Thanks

---

_@VascoSch92 reviewed on 2025-02-19 13:27_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/checkers/ast/mod.rs`:560 on 2025-02-19 13:27_

It makes sense. I changed.

---

_Merged by @MichaReiser on 2025-02-19 13:31_

---

_Closed by @MichaReiser on 2025-02-19 13:31_

---

_Branch deleted on 2025-02-19 13:34_

---
