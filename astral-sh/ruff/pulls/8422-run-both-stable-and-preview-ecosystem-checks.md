```yaml
number: 8422
title: Run both stable and preview ecosystem checks
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zanie/eco-preview
created_at: 2023-11-01T21:44:08Z
updated_at: 2023-11-02T01:51:22Z
url: https://github.com/astral-sh/ruff/pull/8422
synced_at: 2026-01-12T15:55:26Z
```

# Run both stable and preview ecosystem checks

---

_@zanieb_

Closes https://github.com/astral-sh/ruff/issues/8076
Follow-up to #8358 

Doubles the amount of ecosystem checks we do, adding separate groups for the stable sections.

We're likely to run into GitHub comment length restrictions if there are significant deviations. However, it should not be common for changes in stable and preview to occur at the same time, nor should it be common for linter and formatter changes to occur at the same time.

---

_@zanieb reviewed on 2023-11-01 21:46_

---

_Review comment by @zanieb on `.github/workflows/pr-comment.yaml`:52 on 2023-11-01 21:46_

These changes will not take effect until after merge. I can manually dispatch to test if desired.

---

_Label `internal` added by @zanieb on 2023-11-01 21:51_

---

_Comment by @github-actions[bot] on 2023-11-01 22:05_

## `ruff-ecosystem` results
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser reviewed on 2023-11-02 01:00_

---

_Review comment by @MichaReiser on `.github/workflows/pr-comment.yaml`:52 on 2023-11-02 01:00_

Doing a manual dispatch might be good. Just be to sure

---

_Comment by @MichaReiser on 2023-11-02 01:01_

> ## PR Check Results
> ### Ecosystem
> 
> ✅ ecosystem check detected no linter changes.
> ### Linter (preview)
> 
> ✅ ecosystem check detected no linter changes.
> ### Formatter (stable)
> 
> ✅ ecosystem check detected no format changes.
> ### Formatter (preview)
> 
> ✅ ecosystem check detected no format changes.

Should we rename `Ecosystem` to `Linter`? Or use different headings:

## Linter

### Stable

### Preview

## Formatter

---

_@MichaReiser approved on 2023-11-02 01:02_

---

_Comment by @zanieb on 2023-11-02 01:23_

@MichaReiser you commented on the output from the old `pr-comment`. I can run manual dispatch and it should reset to the correct output.

---

_Comment by @zanieb on 2023-11-02 01:47_

Sure is slow!

---

_Comment by @github-actions[bot] on 2023-11-02 01:49_

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

_Comment by @zanieb on 2023-11-02 01:51_

https://github.com/astral-sh/ruff/actions/runs/6727302928

---

_Merged by @zanieb on 2023-11-02 01:51_

---

_Closed by @zanieb on 2023-11-02 01:51_

---

_Branch deleted on 2023-11-02 01:51_

---
