```yaml
number: 11427
title: Add automation for updating our vendored typeshed stubs
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: main
head: typeshed-automation
created_at: 2024-05-14T17:23:50Z
updated_at: 2024-05-15T00:41:11Z
url: https://github.com/astral-sh/ruff/pull/11427
synced_at: 2026-01-10T22:05:26Z
```

# Add automation for updating our vendored typeshed stubs

---

_Pull request opened by @AlexWaygood on 2024-05-14 17:23_

## Summary

This PR adds a workflow that will create an automated PR every two weeks to update our vendored typeshed stubs. Two notes:
- In order for a GitHub Action to be able to create automated PRs, we will need to tick this checkbox in our repo settings:

  ---
  ![image](https://github.com/astral-sh/ruff/assets/66076021/7ca99c44-5cde-47af-8b9d-c00fc356d7ff)
  
  ---
  
- CI will not be triggered on these automated PRs unless the PR is closed and reopened by a maintainer. I think there are workarounds for this limitation, but we haven't succeeded in figuring out how to do them properly at typeshed or mypy, where we also have workflows to trigger automated PRs.


## Test Plan

A GitHub workflow cannot be tested unless it is run, and a workflow cannot be run unless it is already merged to the `main` branch of a repository. So, that's what I did -- with my Ruff fork! I temporarily merged this PR to the `main` branch of my Ruff fork, and ran the workflow. You can see a demo PR here that was automatically created by running this workflow from my fork: https://github.com/AlexWaygood/ruff/pull/9

---

_Review requested from @carljm by @AlexWaygood on 2024-05-14 17:23_

---

_Comment by @github-actions[bot] on 2024-05-14 17:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-05-14 17:39_

Looks good to me!

---

_Review comment by @dhruvmanila on `.github/workflows/sync_typeshed.yaml`:61 on 2024-05-14 18:08_

Should we use the "internal" label to avoid adding this to the changelog?

---

_@dhruvmanila reviewed on 2024-05-14 18:08_

---

_@AlexWaygood reviewed on 2024-05-14 18:13_

---

_Review comment by @AlexWaygood on `.github/workflows/sync_typeshed.yaml`:61 on 2024-05-14 18:13_

Hmm, good point. Eventually these updates will have user-facing impacts, so at that point we will want these updates listed in our changelog. But right now, `red_knot` is a separate crate to the rest of Ruff, so these changes are indeed strictly internal right now. So I'll change it for now so that it adds the `internal` label.

---

_@AlexWaygood reviewed on 2024-05-14 18:17_

---

_Review comment by @AlexWaygood on `.github/workflows/sync_typeshed.yaml`:61 on 2024-05-14 18:17_

```suggestion
          gh pr create --title "Sync vendored typeshed stubs" --body "Close and reopen this PR to trigger CI" --label "internal"
```

---

_Merged by @AlexWaygood on 2024-05-15 00:39_

---

_Closed by @AlexWaygood on 2024-05-15 00:39_

---

_Branch deleted on 2024-05-15 00:39_

---

_Comment by @AlexWaygood on 2024-05-15 00:41_

I manually triggered the workflow to check that everything was working. Looks like it is! #11428

---
