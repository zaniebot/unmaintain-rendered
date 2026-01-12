```yaml
number: 7842
title: Only show warnings for empty preview selectors when enabling rules
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zanie/preview-warn
created_at: 2023-10-06T21:26:57Z
updated_at: 2023-10-08T17:08:38Z
url: https://github.com/astral-sh/ruff/pull/7842
synced_at: 2026-01-12T02:32:41Z
```

# Only show warnings for empty preview selectors when enabling rules

---

_Pull request opened by @zanieb on 2023-10-06 21:26_

Closes https://github.com/astral-sh/ruff/issues/7491

Users found it confusing that warnings were displayed when ignoring a preview rule (which has no effect without `--preview`). While we could retain the warning with different messaging, I've opted to remove it for now. With this pull request, we will only warn on `--select` and `--extend-select` but not `--fixable`, `--unfixable`, `--ignore`, or `--extend-fixable`.



---

_Label `preview` added by @zanieb on 2023-10-06 21:27_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:769 on 2023-10-06 21:36_

We could also push `kind` to these warning lists to facilitate different warnings by kind.

---

_@zanieb reviewed on 2023-10-06 21:36_

---

_@zanieb reviewed on 2023-10-06 21:36_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/configuration.rs`:67 on 2023-10-06 21:36_

Could be made more specific in the future if needed 🤷‍♀️ 

---

_Comment by @github-actions[bot] on 2023-10-06 21:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh approved on 2023-10-08 13:47_

LGTM!

---

_Merged by @zanieb on 2023-10-08 16:14_

---

_Closed by @zanieb on 2023-10-08 16:14_

---

_Branch deleted on 2023-10-08 16:14_

---

_Comment by @bersbersbers on 2023-10-08 17:08_

> While we could retain the warning with different messaging, I've opted to remove it for now.

This is the right choice IMHO. Thank you!

---
