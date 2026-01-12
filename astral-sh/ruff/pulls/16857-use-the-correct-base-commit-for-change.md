```yaml
number: 16857
title: Use the correct base commit for change determination
type: pull_request
state: merged
author: zanieb
labels:
  - ci
assignees: []
merged: true
base: main
head: zb/test-merge-base
created_at: 2025-03-19T20:10:03Z
updated_at: 2025-03-20T13:03:05Z
url: https://github.com/astral-sh/ruff/pull/16857
synced_at: 2026-01-12T15:55:59Z
```

# Use the correct base commit for change determination

---

_@zanieb_

`base.sha` appears to be the commit of the base branch when the pull request was opened, not the base commit that's used to construct the test merge commit — which can lead to incorrect "determine changes" results where commits made to the base ref since the pull request are opened are included in the results.

We use `git merge-base` to find the correct sha, as I don't think that GitHub provides this. They provide `merge_commit_sha` but my understanding is that is equivalent to the actual merge commit we're testing in CI.

I tested this locally on an example pull request. I don't think it's worth trying to reproduce a specific situation here.

---

_Label `ci` added by @zanieb on 2025-03-19 20:10_

---

_Comment by @github-actions[bot] on 2025-03-19 20:21_

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

_Comment by @zanieb on 2025-03-20 12:56_

Huh how does this fix the possible injection? It doesn't seem any safer as written now.

---

_Comment by @AlexWaygood on 2025-03-20 12:57_

GitHub's template strings expand differently to standard shell quoting: https://woodruffw.github.io/zizmor/audits/#template-injection

---

_Comment by @AlexWaygood on 2025-03-20 13:00_

see also https://securitylab.github.com/resources/github-actions-untrusted-input/, which suggests this strategy as a remediation to avoid template strings being able to expand into code

---

_Comment by @zanieb on 2025-03-20 13:02_

I see using `${VAR}` relies on the shell escaping, this makes sense. Thanks!

---

_Marked ready for review by @zanieb on 2025-03-20 13:02_

---

_Merged by @zanieb on 2025-03-20 13:03_

---

_Closed by @zanieb on 2025-03-20 13:03_

---

_Branch deleted on 2025-03-20 13:03_

---
