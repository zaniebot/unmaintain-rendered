```yaml
number: 16796
title: "[ci] Use `git diff` instead of `changed-files` GH action"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/replace-changed-files
created_at: 2025-03-17T10:25:06Z
updated_at: 2025-03-21T22:27:29Z
url: https://github.com/astral-sh/ruff/pull/16796
synced_at: 2026-01-10T19:40:36Z
```

# [ci] Use `git diff` instead of `changed-files` GH action

---

_Pull request opened by @MichaReiser on 2025-03-17 10:25_

## Summary

Use bash and `git diff` to determine which steps need to run. 

We previously used the `changed-files` github actions but using `git` directly seems simple enough. 

All credit for the bash magic goes to @zanieb  and @geofft. All I did was replace the paths arguments.


## Test Plan

* [Linter only change](https://github.com/astral-sh/ruff/pull/16800): See how the fuzzer and formatter steps, and the linter ecosystem checks are skipped
* [Formatter only change](https://github.com/astral-sh/ruff/pull/16799): See how the fuzzer and linter ecosystem checks are skipped


---

_Label `ci` added by @MichaReiser on 2025-03-17 10:25_

---

_Comment by @github-actions[bot] on 2025-03-17 11:19_

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

_Marked ready for review by @MichaReiser on 2025-03-17 11:39_

---

_Comment by @MichaReiser on 2025-03-17 11:40_

I go ahead and merge this because it reduces the number of jobs that we run on every PR. Feel free to leave a comment here if there's something that should be changed, happy to do a follow up PR

---

_Merged by @MichaReiser on 2025-03-17 11:40_

---

_Closed by @MichaReiser on 2025-03-17 11:40_

---

_Branch deleted on 2025-03-17 11:40_

---

_@zanieb reviewed on 2025-03-17 20:48_

---

_Review comment by @zanieb on `.github/workflows/ci.yaml`:48 on 2025-03-17 20:48_

It'd be nice to put the pathspecs on newlines or in an expanded variable for readability.

---

_Review comment by @zanieb on `.github/workflows/ci.yaml`:48 on 2025-03-21 22:27_

I went ahead and did this in #16869 

---

_@zanieb reviewed on 2025-03-21 22:27_

---
