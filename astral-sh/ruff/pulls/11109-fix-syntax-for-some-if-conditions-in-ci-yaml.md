```yaml
number: 11109
title: "Fix syntax for some if-conditions in `ci.yaml`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: forks
created_at: 2024-04-23T17:43:03Z
updated_at: 2024-04-23T18:47:02Z
url: https://github.com/astral-sh/ruff/pull/11109
synced_at: 2026-01-12T15:55:34Z
```

# Fix syntax for some if-conditions in `ci.yaml`

---

_@AlexWaygood_

## Summary

These CI jobs always fail on my fork whenever I sync my fork with the upstream repo. I have the power to create branches on the upstream `ruff` repo these days but I still occasionally sync and push branches to my fork out of habit, so this is kinda annoying 😛


---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:278 on 2024-04-23 17:43_

The syntax is incorrect here; this currently seems to always evaluate to `true` when I sync the `main` branch of my fork with the upstream `main`

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:555 on 2024-04-23 17:45_

This should be skipped for forks because our codspeed token is only valid for `astral-sh/ruff`.

This is quite a long line. I think there are ways of doing multiline if-conditions in GitHub Actions, but they seem to be pretty poorly documented, and I've got it wrong frequently in the past. I'd prefer to just do a very long single-line if-condition, as I currently have.

---

_@AlexWaygood reviewed on 2024-04-23 17:45_

---

_Comment by @github-actions[bot] on 2024-04-23 18:00_

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

_Label `ci` added by @AlexWaygood on 2024-04-23 18:24_

---

_@zanieb approved on 2024-04-23 18:44_

---

_Merged by @AlexWaygood on 2024-04-23 18:46_

---

_Closed by @AlexWaygood on 2024-04-23 18:46_

---

_Branch deleted on 2024-04-23 18:47_

---
