```yaml
number: 13744
title: "Fix `mkdocs` CI job"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/ci-fix-mkdocs-job
created_at: 2024-10-14T07:10:49Z
updated_at: 2024-10-14T07:41:34Z
url: https://github.com/astral-sh/ruff/pull/13744
synced_at: 2026-01-10T20:59:37Z
```

# Fix `mkdocs` CI job

---

_Pull request opened by @MichaReiser on 2024-10-14 07:10_

## Summary

This PR specifies sets an explicit python version for `setup-python` because it otherwise skips installing any python version and running pip then fails because it doesn't allow installing into an externally managed python installation. 

I used this as a chance to migrate the workflow to `uv pip`

## Test Plan

See CI

---

_Label `ci` added by @MichaReiser on 2024-10-14 07:11_

---

_Marked ready for review by @MichaReiser on 2024-10-14 07:23_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-14 07:26_

---

_@AlexWaygood approved on 2024-10-14 07:28_

LGTM.

I think the "actual fix" here is the change you made to the setup-python action, which forces the job to install a version of Python that isn't externally managed. Without that, I think uv would also complain about us trying to install into an externally managed Python here unless we used `--break-system-packages`.

But switching to uv is no bad thing!

---

_Merged by @MichaReiser on 2024-10-14 07:31_

---

_Closed by @MichaReiser on 2024-10-14 07:31_

---

_Branch deleted on 2024-10-14 07:31_

---

_Comment by @github-actions[bot] on 2024-10-14 07:41_

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
