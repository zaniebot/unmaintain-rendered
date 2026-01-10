```yaml
number: 19223
title: "[`pandas-vet`] Remove `pandas-df-variable-name` (`PD901`)"
type: pull_request
state: merged
author: CodeMan62
labels:
  - rule
  - breaking
assignees: []
merged: true
base: brent/0.13.0
head: codeman/7710
created_at: 2025-07-09T01:32:48Z
updated_at: 2025-09-08T17:34:32Z
url: https://github.com/astral-sh/ruff/pull/19223
synced_at: 2026-01-10T17:46:21Z
```

# [`pandas-vet`] Remove `pandas-df-variable-name` (`PD901`)

---

_Pull request opened by @CodeMan62 on 2025-07-09 01:32_

## Summary
closes #7710 

## Test Plan

It is is removal so i don't think we have to add tests otherwise i have followed test plan mentioned in contributing.md


---

_Comment by @github-actions[bot] on 2025-07-09 02:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `breaking` added by @ntBre on 2025-07-09 13:51_

---

_Label `do-not-merge` added by @ntBre on 2025-07-09 13:51_

---

_Added to milestone `v0.13` by @ntBre on 2025-07-09 13:51_

---

_Converted to draft by @dhruvmanila on 2025-07-10 06:22_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pandas_vet/rules/assignment_to_df.rs`:9 on 2025-09-05 13:52_

```suggestion
/// This rule has been removed as it's highly opinionated and overly strict in most cases.
```

I think the rest of the sentence is still valuable enough.

---

_@ntBre approved on 2025-09-05 13:52_

---

_Marked ready for review by @ntBre on 2025-09-05 13:52_

---

_Renamed from "[pandas-vet] Remove pandas-df-variable-name (PD901)" to "[`pandas-vet`] Remove `pandas-df-variable-name` (`PD901`)" by @ntBre on 2025-09-05 13:57_

---

_Merged by @ntBre on 2025-09-05 14:07_

---

_Closed by @ntBre on 2025-09-05 14:07_

---

_Label `do-not-merge` removed by @ntBre on 2025-09-08 17:34_

---

_Label `rule` added by @ntBre on 2025-09-08 17:34_

---
