```yaml
number: 13380
title: Update the revisions of the formatter stability check projects
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/update-formatter-stability-checks
created_at: 2024-09-17T11:09:10Z
updated_at: 2024-09-18T06:26:42Z
url: https://github.com/astral-sh/ruff/pull/13380
synced_at: 2026-01-10T21:30:32Z
```

# Update the revisions of the formatter stability check projects

---

_Pull request opened by @MichaReiser on 2024-09-17 11:09_

## Summary

Bump the revisions of the projects used in the formatter stability check to:

* Compare against a more recent version of black
* Increased usage of more recent syntax features. 

Some projects have migrated to Ruff. These projects won't give us much signal anymore on how compatible Ruff is with black but are useful when promoting the stable style: many changes mean a lot of churn for projects adopting the new style guide.

## Test Plan

Main

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75869 |              1812 |              1669 |
| django         |           0.99933 |              2771 |               181 |
| home-assistant |           0.99897 |             10596 |              8492 |
| poetry         |           0.99897 |               321 |                14 |
| transformers   |           0.99726 |              2660 |              1119 |
| twine          |           1.00000 |                32 |                 7 |
| typeshed       |           0.99977 |              3669 |                75 |
| warehouse      |           0.99963 |               653 |                25 |
| zulip          |           0.99890 |              1459 |                69 |




PR

 | project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.76424 |              1959 |              1781 |
| django         |           0.99942 |              2791 |                76 |
| home-assistant |           1.00000 |             12993 |                 0 |
| poetry         |           1.00000 |               444 |                 0 |
| transformers   |           0.99909 |              3151 |                15 |
| twine          |           1.00000 |                32 |                 0 |
| typeshed       |           0.99139 |              4766 |               133 |
| warehouse      |           0.99962 |               767 |                33 |
| zulip          |           1.00000 |              1699 |                 0 |


* warehouse: More changed files
* typeshed: More changed files
* twine: fewer changed fiels
* transfomrers: fewer changed files
* django: fewer changed files
* cpython more changed files

I haven't done any comparison what the changes are.


---

_Label `ci` added by @MichaReiser on 2024-09-17 11:09_

---

_Comment by @github-actions[bot] on 2024-09-17 11:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "Update the revisions of projects still using black in the formatter stability check" to "Update the revisions of the formatter stability check projects" by @MichaReiser on 2024-09-17 12:35_

---

_Review requested from @konstin by @MichaReiser on 2024-09-17 12:51_

---

_Merged by @MichaReiser on 2024-09-18 06:26_

---

_Closed by @MichaReiser on 2024-09-18 06:26_

---

_Branch deleted on 2024-09-18 06:26_

---
