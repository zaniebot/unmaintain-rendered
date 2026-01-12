```yaml
number: 7903
title: Fix comparison when using greater than or equal in UP036
type: pull_request
state: closed
author: zanieb
labels:
  - bug
assignees: []
draft: true
base: main
head: zanie/fix-up036
created_at: 2023-10-10T19:22:17Z
updated_at: 2023-10-11T15:24:39Z
url: https://github.com/astral-sh/ruff/pull/7903
synced_at: 2026-01-12T15:55:25Z
```

# Fix comparison when using greater than or equal in UP036

---

_@zanieb_

Closes https://github.com/astral-sh/ruff/issues/7902

---

_Label `bug` added by @zanieb on 2023-10-10 19:22_

---

_@charliermarsh reviewed on 2023-10-10 19:28_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs`:259 on 2023-10-10 19:28_

I feel like we need different tie-breaking logic here depending on whether it's a great-than comparison or a less-than comparison, because this condition seems _correct_ (and the comment does too) for less-than.

---

_@zanieb reviewed on 2023-10-10 19:32_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs`:259 on 2023-10-10 19:32_

Yeah this fix is incorrect — this is really hurting my brain haha

---

_Comment by @github-actions[bot] on 2023-10-10 19:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @zanieb on 2023-10-11 15:24_

Replaced by https://github.com/astral-sh/ruff/pull/7920

---

_Closed by @zanieb on 2023-10-11 15:24_

---
