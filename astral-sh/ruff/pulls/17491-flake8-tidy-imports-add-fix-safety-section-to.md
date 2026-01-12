```yaml
number: 17491
title: "[`flake8-tidy-imports`] Add fix safety section to docs (`TID252`) "
type: pull_request
state: open
author: Kalmaegi
labels:
  - documentation
assignees: []
base: main
head: doc_fix_safety_for_relative_imports
created_at: 2025-04-20T08:37:15Z
updated_at: 2025-04-22T19:34:28Z
url: https://github.com/astral-sh/ruff/pull/17491
synced_at: 2026-01-12T15:56:02Z
```

# [`flake8-tidy-imports`] Add fix safety section to docs (`TID252`) 

---

_@Kalmaegi_

## Summary

add `fix safety` section to `TID252: relative_imports.rs`, for #15584 

When there are multiple lines of quotes, the comments in between will be deleted after the fix, but everything else works fine.
Before:
![cbeddda57dcb4ebcf789623d92a3082](https://github.com/user-attachments/assets/db2122a1-6e0b-4349-b348-6c8d699549b9)


After:
![8168a4da225aea24256b8bc59569b1d](https://github.com/user-attachments/assets/cea0bc6b-5719-4c4e-8f59-bf306732ab2e)



---

_Comment by @VascoSch92 on 2025-04-20 12:06_

I think this fix could change the program behaviour when module caching or manipulation is involved. For example, in testing scenarios or when dealing with complex module manipulations.


---

_Renamed from "[flake8-tidy-imports] Add fix safety section to docs (TID252) " to "[`flake8-tidy-imports`] Add fix safety section to docs (`TID252`) " by @Kalmaegi on 2025-04-20 14:58_

---

_Comment by @github-actions[bot] on 2025-04-21 15:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @AlexWaygood on 2025-04-22 19:18_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-04-22 19:34_

---
