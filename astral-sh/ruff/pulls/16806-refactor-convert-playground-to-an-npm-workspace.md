```yaml
number: 16806
title: "[refactor] Convert playground to an NPM workspace"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
assignees: []
merged: true
base: main
head: micha/convert-playground-to-workspace
created_at: 2025-03-17T14:26:44Z
updated_at: 2025-03-17T17:00:10Z
url: https://github.com/astral-sh/ruff/pull/16806
synced_at: 2026-01-12T15:55:58Z
```

# [refactor] Convert playground to an NPM workspace

---

_@MichaReiser_

## Summary

This is prep-work for the Red Knot playground. We'll have two playgrounds, one for Red Knot and Ruff.
I want to share some components between the two, a "shared" NPM package in a local workspace is a great fit for that. 
I also want to share the dev dependencies and dev scripts. Again, NPM workspaces are great for that. 

This PR also sets up a CI workflow for the playground to prevent surprises during the release.

## Test Plan

CI, local `npm install`, `npm start`, ...

I verified that the new CI step fails if there's a typescript or formatting error.

* [Deployment test run](https://github.com/astral-sh/ruff/actions/runs/13904914480/job/38905524353)


---

_Label `playground` added by @MichaReiser on 2025-03-17 14:26_

---

_Comment by @charliermarsh on 2025-03-17 14:28_

No objection from me conceptually, feel free to merge at your own discretion.

---

_Review comment by @MichaReiser on `.github/workflows/publish-playground.yml`:39 on 2025-03-17 14:29_

We now install wasm-pack with NPM. This also removes the need to version it manually in the workflow

---

_@MichaReiser reviewed on 2025-03-17 14:29_

---

_Comment by @github-actions[bot] on 2025-03-17 15:07_

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

_Marked ready for review by @MichaReiser on 2025-03-17 16:53_

---

_Merged by @MichaReiser on 2025-03-17 16:56_

---

_Closed by @MichaReiser on 2025-03-17 16:56_

---

_Branch deleted on 2025-03-17 16:56_

---
