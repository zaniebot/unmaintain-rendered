```yaml
number: 7040
title: "Refactor ruff_cli's run method to return on each branch"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/main
created_at: 2023-09-01T12:50:55Z
updated_at: 2023-09-01T13:16:21Z
url: https://github.com/astral-sh/ruff/pull/7040
synced_at: 2026-01-12T02:45:38Z
```

# Refactor ruff_cli's run method to return on each branch

---

_Pull request opened by @charliermarsh on 2023-09-01 12:50_

## Summary

I think the fallthrough here for some branches is a little confusing. Now each branch either runs a command that returns `Result<ExitStatus>`, or runs a command that returns `Result<()>` and then explicitly returns `Ok(ExitStatus::SUCCESS)`.


---

_Review requested from @dhruvmanila by @charliermarsh on 2023-09-01 12:50_

---

_Comment by @charliermarsh on 2023-09-01 12:51_

@dhruvmanila - Do you find this clearer?

---

_Label `internal` added by @charliermarsh on 2023-09-01 12:52_

---

_@MichaReiser approved on 2023-09-01 12:59_

---

_Comment by @github-actions[bot] on 2023-09-01 13:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @charliermarsh on 2023-09-01 13:15_

---

_Closed by @charliermarsh on 2023-09-01 13:15_

---

_Branch deleted on 2023-09-01 13:15_

---

_@dhruvmanila approved on 2023-09-01 13:16_

Yeah, this is much clearer now. Thanks!

---
