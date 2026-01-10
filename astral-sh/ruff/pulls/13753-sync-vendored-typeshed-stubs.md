```yaml
number: 13753
title: Sync vendored typeshed stubs
type: pull_request
state: merged
author: github-actions
labels:
  - internal
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2024-10-15T00:26:29Z
updated_at: 2024-10-15T13:45:26Z
url: https://github.com/astral-sh/ruff/pull/13753
synced_at: 2026-01-10T20:59:37Z
```

# Sync vendored typeshed stubs

---

_Pull request opened by @github-actions on 2024-10-15 00:26_

Close and reopen this PR to trigger CI

---

_Review requested from @carljm by @github-actions[bot] on 2024-10-15 00:26_

---

_Label `internal` added by @github-actions[bot] on 2024-10-15 00:26_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2024-10-15 00:26_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2024-10-15 00:26_

---

_Closed by @AlexWaygood on 2024-10-15 06:51_

---

_Reopened by @AlexWaygood on 2024-10-15 06:51_

---

_Comment by @dhruvmanila on 2024-10-15 06:57_

I wonder if `gh workflow run` can trigger the CI here which uses the `workflow_dispatch` event because as per https://docs.github.com/en/actions/security-for-github-actions/security-guides/automatic-token-authentication#using-the-github_token-in-a-workflow it should (?):

> When you use the repository's `GITHUB_TOKEN` to perform tasks, events triggered by the `GITHUB_TOKEN`, with the exception of `workflow_dispatch` and `repository_dispatch`, will not create a new workflow run.

---

_Comment by @AlexWaygood on 2024-10-15 07:01_

The failing test was broken by https://github.com/python/typeshed/pull/12714 (actually a change we asked for 😄). It should be a pretty simple fix.

---

_Comment by @AlexWaygood on 2024-10-15 07:02_

> I wonder if `gh workflow run` can trigger the CI here which uses the `workflow_dispatch` event

Ooh, that would be great if we can get it to work!

---

_Merged by @AlexWaygood on 2024-10-15 13:36_

---

_Closed by @AlexWaygood on 2024-10-15 13:36_

---

_Branch deleted on 2024-10-15 13:36_

---

_Comment by @github-actions[bot] on 2024-10-15 13:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
