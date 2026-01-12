```yaml
number: 10776
title: "D403: Require capitalizing single word sentence"
type: pull_request
state: merged
author: MichaReiser
labels:
  - rule
assignees: []
merged: true
base: main
head: d403-single-word
created_at: 2024-04-04T16:20:35Z
updated_at: 2024-04-05T06:44:53Z
url: https://github.com/astral-sh/ruff/pull/10776
synced_at: 2026-01-12T15:55:33Z
```

# D403: Require capitalizing single word sentence

---

_@MichaReiser_

## Summary

Closes https://github.com/astral-sh/ruff/issues/10775

D403 requires capitalizing the first word in docstrings, but only if the word is all ASCII letters (e.g. not a `.`). 

D403 doesn't enforce capitalizing the first word today if it consists of a single word terminated by a `.` because a `.` is not an ASCII letter. 
This matches pydocstyles behavior. However, it doesn enforce capitalizing the first word if it isn't terminated with a `.`

This PR fixes `D403` to also flag docstrings that start with a single word terminated with a `.`.

## Test Plan

Added unit tests


---

_Label `rule` added by @MichaReiser on 2024-04-04 16:20_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/capitalized.rs`:91 on 2024-04-04 16:21_

I moved this down to avoid checking all characters of the first word if the character is uppercase anyway.

---

_@MichaReiser reviewed on 2024-04-04 16:21_

---

_Marked ready for review by @MichaReiser on 2024-04-04 16:27_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-04-04 16:33_

---

_Comment by @github-actions[bot] on 2024-04-04 16:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-04-04 21:04_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydocstyle/rules/capitalized.rs`:64 on 2024-04-04 21:04_

Should we trim other punctuation? Like `!?.`

---

_@charliermarsh approved on 2024-04-04 21:04_

---

_@charliermarsh reviewed on 2024-04-04 21:04_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydocstyle/rules/capitalized.rs`:64 on 2024-04-04 21:04_

(Docstrings are allowed to end with those, per our rules.)

---

_Merged by @MichaReiser on 2024-04-05 06:42_

---

_Closed by @MichaReiser on 2024-04-05 06:42_

---

_Branch deleted on 2024-04-05 06:42_

---
