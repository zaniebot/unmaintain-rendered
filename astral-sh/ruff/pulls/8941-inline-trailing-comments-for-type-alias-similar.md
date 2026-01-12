```yaml
number: 8941
title: Inline trailing comments for type alias similar to assignments
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
assignees: []
merged: true
base: main
head: align-type-alias-with-assignment-formatting
created_at: 2023-12-01T04:29:05Z
updated_at: 2023-12-12T03:09:10Z
url: https://github.com/astral-sh/ruff/pull/8941
synced_at: 2026-01-10T23:40:55Z
```

# Inline trailing comments for type alias similar to assignments

---

_Pull request opened by @MichaReiser on 2023-12-01 04:29_

## Summary

This PR aligns the comment formatting for type aliases with the assignment formatting. 

Black and Ruff inline a trailing assignment comment into the parentheses of the value if the value gets parenthesized. 
Neither Black or Ruff apply this layout for type assignments today. To me, this feels inconsistent because type alias statements are just a specific form of assignment. 

Note that this is a Black divergence. Depending on the ecosystem check results, we should consider hiding this behind preview for now. Although I don't expect this to be common, considering that type aliases are new.

## Test Plan

Added tests. 

The similarity index remain unchanged. 

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99984 |              2772 |                34 |
| home-assistant |           0.99955 |             10596 |               213 |
| poetry         |           0.96208 |               317 |                35 |
| transformers   |           0.99967 |              2657 |               324 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99976 |               654 |                14 |
| zulip          |           0.99957 |              1459 |                36 |


---

_Comment by @MichaReiser on 2023-12-01 04:29_

Current dependencies on/for this PR:
* main
  * **PR #8920** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8920?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8943** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8943?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #9102** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/9102?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #8941** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8941?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈
      * **PR #8940** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8940?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8941?utm_source=stack-comment).

---

_Renamed from "cccccbhcgljulfeerhjebkiinfkinldefjgtnteuhttu" to "Inline trailing comments for type alias similar to assignments" by @MichaReiser on 2023-12-01 04:29_

---

_Label `formatter` added by @MichaReiser on 2023-12-01 04:29_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-12-01 04:33_

---

_@charliermarsh approved on 2023-12-01 04:35_

I think this is correct.

---

_Comment by @github-actions[bot] on 2023-12-01 04:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @MichaReiser on 2023-12-04 05:27_

---

_Closed by @MichaReiser on 2023-12-04 05:27_

---

_Branch deleted on 2023-12-04 05:27_

---
