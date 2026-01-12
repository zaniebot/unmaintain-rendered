```yaml
number: 15365
title: "[`flake8-django`] Recognize other magic methods (`DJ012`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
assignees: []
merged: true
base: main
head: DJ012
created_at: 2025-01-09T03:13:54Z
updated_at: 2025-01-09T13:45:33Z
url: https://github.com/astral-sh/ruff/pull/15365
synced_at: 2026-01-12T15:55:51Z
```

# [`flake8-django`] Recognize other magic methods (`DJ012`)

---

_@InSyncWithFoo_

## Summary

Resolves #13892.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-09 03:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @dhruvmanila on 2025-01-09 08:25_

---

_Label `breaking` added by @dhruvmanila on 2025-01-09 08:25_

---

_Added to milestone `v.0.10` by @dhruvmanila on 2025-01-09 08:25_

---

_@MichaReiser approved on 2025-01-09 08:34_

---

_Label `do-not-merge` added by @MichaReiser on 2025-01-09 08:34_

---

_Label `do-not-merge` removed by @MichaReiser on 2025-01-09 08:35_

---

_Comment by @MichaReiser on 2025-01-09 08:37_

I think we have three options here:

* We change it to preview only so that we can merge it here
* We leave it as is and wait for the 0.10 release to merge it
* We merge it as is because we mainly consider this a bugfix. 

I'm leaning toward classifying this as a bugfix and releasing it as part of 0.9 (CC: @AlexWaygood)

---

_Comment by @AlexWaygood on 2025-01-09 13:22_

> I'm leaning toward classifying this as a bugfix and releasing it as part of 0.9 (CC: @AlexWaygood)

Yeah, that makes sense to me!

---

_Label `breaking` removed by @MichaReiser on 2025-01-09 13:36_

---

_Merged by @MichaReiser on 2025-01-09 13:36_

---

_Closed by @MichaReiser on 2025-01-09 13:36_

---

_Removed from milestone `v0.10` by @AlexWaygood on 2025-01-09 13:37_

---

_Added to milestone `v0.9` by @AlexWaygood on 2025-01-09 13:37_

---

_Branch deleted on 2025-01-09 13:45_

---
