```yaml
number: 19416
title: "[`flake8-pyi`] Skip fix if all `Union` members are `None` (`PYI016`) "
type: pull_request
state: merged
author: soundsonacid
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: frank/19403
created_at: 2025-07-17T23:21:41Z
updated_at: 2025-07-22T17:09:50Z
url: https://github.com/astral-sh/ruff/pull/19416
synced_at: 2026-01-10T17:58:13Z
```

# [`flake8-pyi`] Skip fix if all `Union` members are `None` (`PYI016`) 

---

_Pull request opened by @soundsonacid on 2025-07-17 23:21_

patches #19403 

---

_Review requested from @AlexWaygood by @soundsonacid on 2025-07-17 23:21_

---

_Comment by @soundsonacid on 2025-07-17 23:27_

resolving test failures... stand by!

---

_Converted to draft by @soundsonacid on 2025-07-17 23:35_

---

_Comment by @github-actions[bot] on 2025-07-18 15:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @soundsonacid on 2025-07-18 16:52_

---

_Renamed from "[pyi108]: skip if all Union members are None" to "[`flake8-pyi`] Skip fix if all `Union` members are `None` (`PYI016`) " by @ntBre on 2025-07-22 13:01_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:114 on 2025-07-22 13:05_

nit: this comment seems a bit long to me, for what it's explaining. I'd be happy with just the first line, or maybe the first ~3 lines at most.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:117 on 2025-07-22 13:06_

I was going to leave a nit about the `n` variable name, but I think we could replace the whole closure with `is_none_literal_expr`:


```suggestion
        .all(Expr::is_none_literal_expr)
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:123 on 2025-07-22 13:09_

I think we can just return here without defusing. It's okay to emit the diagnostic because there _is_ a duplicate union member, we just don't want to add an autofix that will cause an error. The diagnostic is still helpful to draw the user's attention to the code. If we go with this, we should also update the test comment since it won't be OK  anymore.

---

_@ntBre reviewed on 2025-07-22 13:11_

Thank you!  I just had a few small suggestions.

---

_Label `bug` added by @ntBre on 2025-07-22 13:11_

---

_Label `fixes` added by @ntBre on 2025-07-22 13:11_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-22 13:23_

---

_Review comment by @soundsonacid on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:117 on 2025-07-22 14:05_

unfortunately this iterator yields &&Expr so we can't do the nice little inlining function call stuff like that

going to change to `|expr| Expr::is_none_literal_expr(expr)`

---

_@soundsonacid reviewed on 2025-07-22 14:05_

---

_@soundsonacid reviewed on 2025-07-22 14:06_

---

_Review comment by @soundsonacid on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:123 on 2025-07-22 14:06_

ok sounds good i think this makes sense

---

_@ntBre reviewed on 2025-07-22 14:17_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:117 on 2025-07-22 14:17_

Ah that makes sense. If you'll allow me one more even tinier nit, we could use `expr.is_none_literal_expr()` instead of the qualified form.

---

_@ntBre approved on 2025-07-22 14:17_

---

_Merged by @ntBre on 2025-07-22 17:03_

---

_Closed by @ntBre on 2025-07-22 17:03_

---
