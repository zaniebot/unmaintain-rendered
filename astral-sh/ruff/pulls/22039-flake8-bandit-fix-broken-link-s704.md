```yaml
number: 22039
title: "[`flake8-bandit`] Fix broken link (`S704`)"
type: pull_request
state: merged
author: ntBre
labels:
  - documentation
assignees: []
merged: true
base: main
head: brent/s704-link
created_at: 2025-12-17T22:14:37Z
updated_at: 2025-12-17T22:35:06Z
url: https://github.com/astral-sh/ruff/pull/22039
synced_at: 2026-01-12T15:57:39Z
```

# [`flake8-bandit`] Fix broken link (`S704`)

---

_@ntBre_

Summary
--

While going through all the rules I noticed a broken link in the S704 docs. (I then got really confused because it appeared to be correct in the _other_ `unsafe_markup_use.rs` file for the removed RUF version of the rule).

Test Plan
--

[Before](https://docs.astral.sh/ruff/rules/unsafe-markup-use/):

<img width="537" height="171" alt="image" src="https://github.com/user-attachments/assets/01007cab-6673-48e5-b3a5-6006bc78a027" />


After:

<img width="451" height="189" alt="image" src="https://github.com/user-attachments/assets/4e5d0e0d-76be-4f66-b747-e209f11ab11a" />

I also did at least a cursory grep for other cases with escaped link brackets (`\[`) and only turned up this rule on main.


---

_Label `documentation` added by @ntBre on 2025-12-17 22:14_

---

_Comment by @astral-sh-bot[bot] on 2025-12-17 22:27_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Merged by @ntBre on 2025-12-17 22:35_

---

_Closed by @ntBre on 2025-12-17 22:35_

---

_Branch deleted on 2025-12-17 22:35_

---
