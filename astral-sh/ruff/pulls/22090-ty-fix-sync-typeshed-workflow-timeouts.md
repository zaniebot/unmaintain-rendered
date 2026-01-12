```yaml
number: 22090
title: "[ty] Fix sync-typeshed workflow timeouts"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/typeshed-sync-timeout
created_at: 2025-12-19T17:54:48Z
updated_at: 2025-12-19T18:09:10Z
url: https://github.com/astral-sh/ruff/pull/22090
synced_at: 2026-01-12T15:57:41Z
```

# [ty] Fix sync-typeshed workflow timeouts

---

_@AlexWaygood_

## Summary

Our most recent typeshed-sync workflow failed, and failed again when I re-ran it: https://github.com/astral-sh/ruff/actions/runs/20216754532. The cause of the failure was that it took too long to build ty in the last stage, so the job timed out. Because the job timed out rather than an error occurring, there also wasn't an issue created telling us that the typeshed sync failed.

This PR fixes the timeout by removing the part where we try to run ty's tests from the workflow. This part was added in https://github.com/astral-sh/ruff/pull/20892, but it's never worked as intended: https://github.com/astral-sh/ruff/pull/20892#issuecomment-3536537553. So we may as well remove it.

I also fixed the issue-creation logic so that an issue will be created whenever the outcome `!= 'success'`, instead of an issue only being created when the outcome `== 'failure'`. According to https://docs.github.com/en/actions/reference/workflows-and-actions/contexts#needs-context, `needs.<job_id>.result` could be `success`, `failure`, `cancelled`, or `skipped`.

Fixes https://github.com/astral-sh/ty/issues/1996

## Test Plan

I'll run the sync-typeshed workflow on this branch to test it: https://github.com/astral-sh/ruff/actions/runs/20378210302


---

_Label `ci` added by @AlexWaygood on 2025-12-19 17:54_

---

_Label `ty` added by @AlexWaygood on 2025-12-19 17:54_

---

_@MichaReiser approved on 2025-12-19 18:01_

---

_Marked ready for review by @AlexWaygood on 2025-12-19 18:08_

---

_Comment by @AlexWaygood on 2025-12-19 18:08_

It worked: https://github.com/astral-sh/ruff/pull/22091

---

_Merged by @AlexWaygood on 2025-12-19 18:09_

---

_Closed by @AlexWaygood on 2025-12-19 18:09_

---

_Branch deleted on 2025-12-19 18:09_

---
