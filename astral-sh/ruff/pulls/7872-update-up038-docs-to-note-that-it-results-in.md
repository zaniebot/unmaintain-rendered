```yaml
number: 7872
title: Update UP038 docs to note that it results in slower code
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
assignees: []
merged: true
base: main
head: up038-docs
created_at: 2023-10-09T15:42:38Z
updated_at: 2023-10-09T20:59:39Z
url: https://github.com/astral-sh/ruff/pull/7872
synced_at: 2026-01-12T02:32:41Z
```

# Update UP038 docs to note that it results in slower code

---

_Pull request opened by @AlexWaygood on 2023-10-09 15:42_

See discussion in #7871. I tried to use language similar to the existing performance warnings in the `flake8-use-pathlib` docs, e.g. https://docs.astral.sh/ruff/rules/os-path-abspath/#os-path-abspath-pth100

---

_@AlexWaygood reviewed on 2023-10-09 15:47_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/use_pep604_isinstance.rs`:42 on 2023-10-09 15:47_

I also changed this line, since the rule only makes code more concise if the tuple of types has exactly length 2. If the tuple is 3 elements long, the rule results in code that has exactly the same level of verbosity. If the tuple is 4 or more elements long, the rule results in code that is more verbose than it was originally.

---

_@konstin approved on 2023-10-09 15:59_

Thanks!

---

_Comment by @github-actions[bot] on 2023-10-09 15:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @zanieb on 2023-10-09 16:45_

---

_Closed by @zanieb on 2023-10-09 16:45_

---

_Label `documentation` added by @zanieb on 2023-10-09 16:45_

---

_Branch deleted on 2023-10-09 20:59_

---
