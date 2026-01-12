```yaml
number: 15022
title: Bump zizmor pre-commit hook to the latest version and fix new warnings
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/upgrade-zizmor
created_at: 2024-12-16T14:41:58Z
updated_at: 2024-12-16T17:45:47Z
url: https://github.com/astral-sh/ruff/pull/15022
synced_at: 2026-01-12T15:55:49Z
```

# Bump zizmor pre-commit hook to the latest version and fix new warnings

---

_@AlexWaygood_

## Summary

(This PR is stacked on top of https://github.com/astral-sh/ruff/pull/15021; review that first.)

This updates our zizmor pre-commit hook to the latest version. Zizmor detects potential security vulnerabilities in GitHub Actions workflows. The latest version of zizmor finds some more potential vulnerabilities in our workflows, which this PR fixes.

As with the PR that initially added zizmor to our CI, most of the required changes stem from zizmor's `template-injection` check: https://woodruffw.github.io/zizmor/audits/#template-injection.

## Test Plan

- `uvx pre-commit run -a` passes locally
- Let's see if CI passes on this PR...


---

_Label `ci` added by @AlexWaygood on 2024-12-16 14:41_

---

_Comment by @github-actions[bot] on 2024-12-16 14:50_

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

_Marked ready for review by @AlexWaygood on 2024-12-16 14:54_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-12-16 14:54_

---

_Review comment by @dhruvmanila on `.github/workflows/pr-comment.yaml`:17 on 2024-12-16 17:23_

Is this change to isolate the permissions only to a single job (although we only have one in this workflow file)? I'm just trying to understand the change.

---

_@dhruvmanila reviewed on 2024-12-16 17:23_

---

_@dhruvmanila approved on 2024-12-16 17:24_

Looks good!

---

_@AlexWaygood reviewed on 2024-12-16 17:27_

---

_Review comment by @AlexWaygood on `.github/workflows/pr-comment.yaml`:17 on 2024-12-16 17:27_

Yes -- here's the complaint from the latest version of zizmor regarding the workflow as it exists on the Ruff `main` branch:

```
error[excessive-permissions]: overly broad workflow or job-level permissions
  --> /Users/alexw/dev/ruff/.github/workflows/pr-comment.yaml:13:1
   |
13 | / permissions:
14 | |   pull-requests: write
   | |______________________^ pull-requests: write is overly broad at the workflow level
   |
   = note: audit confidence → High

163 findings (1 ignored, 161 suppressed): 0 unknown, 0 informational, 0 low, 0 medium, 1 high
```

Docs for the zizmor rule: https://woodruffw.github.io/zizmor/audits/#excessive-permissions

---

_@dhruvmanila reviewed on 2024-12-16 17:33_

---

_Review comment by @dhruvmanila on `.github/workflows/pr-comment.yaml`:17 on 2024-12-16 17:33_

Makes sense, thanks!

---

_@AlexWaygood reviewed on 2024-12-16 17:35_

---

_Review comment by @AlexWaygood on `.pre-commit-config.yaml`:91 on 2024-12-16 17:35_

```suggestion
  # zizmor detects security vulnerabilities in GitHub Actions workflows.
  # Additional configuration for the tool is found in `.github/zizmor.yml`
```

---

_Merged by @AlexWaygood on 2024-12-16 17:45_

---

_Closed by @AlexWaygood on 2024-12-16 17:45_

---

_Branch deleted on 2024-12-16 17:45_

---
