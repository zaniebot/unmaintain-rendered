```yaml
number: 20894
title: "Don't use codspeed or depot runners in CI jobs on forks"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/fork-ci
created_at: 2025-10-15T14:31:42Z
updated_at: 2025-10-16T12:16:20Z
url: https://github.com/astral-sh/ruff/pull/20894
synced_at: 2026-01-12T15:57:11Z
```

# Don't use codspeed or depot runners in CI jobs on forks

---

_@AlexWaygood_

## Summary

Currently I get a GitHub notification every time I sync my Ruff fork with `astral-sh/ruff`. The notification informs me that [the `ci` workflow failed](https://github.com/AlexWaygood/ruff/actions/runs/18496851594), because it tried to run various jobs on Depot or codspeed runners that my fork doesn't have access to. This is kinda annoying, and -- while I don't really _need_ to sync my fork on regular basis, since I have the ability to create branches directly on the upstream repo -- I'm guessing this also occurs for all our third-party contributors who have forks of Ruff.

The solution to this is not to run CI jobs on forks if they require a Depot or Codspeed runner. Unfortunately there doesn't appear to be a way to say "run this workflow on all pushes to `main`, but skip the whole workflow if it's a fork" -- you have to include the `if github.repository == 'astral-sh/ruff'` check in each separate CI job inside the workflow.

Another solution would be to say that contributors who have forks should simply disable GitHub Actions altogether on their forks. But running CI from a fork can be pretty useful, especially if you're a third-party contributor, as a way to test changes without opening a PR upstream (which can result in notifications for the upstream maintainers, even if the PR is opened in draft mode).

## Test Plan

CI on this PR


---

_Label `ci` added by @AlexWaygood on 2025-10-15 14:31_

---

_@MichaReiser reviewed on 2025-10-15 14:37_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:242 on 2025-10-15 14:37_

I think I slightly prefer `github.event.pull_request.head.repo.fork == false` as it's more explicit. 

An alternative is to disable all CI jobs for forks by adding the same condition at the very top of the file

---

_@AlexWaygood reviewed on 2025-10-15 14:39_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:242 on 2025-10-15 14:39_

> An alternative is to disable all CI jobs for forks by adding the same condition at the very top of the file

hmm, where exactly would I put this condition at the top of the file? I'd love to do that if it's possible, but I didn't think it was

---

_Comment by @github-actions[bot] on 2025-10-15 14:49_

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

_@AlexWaygood reviewed on 2025-10-15 16:56_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:242 on 2025-10-15 16:56_

> I think I slightly prefer `github.event.pull_request.head.repo.fork == false` as it's more explicit.

I don't think this would fix the issue I'm trying to fix, because the specific issue I'm trying to fix is that these jobs are being run on pushes to the `main` branch of the `AlexWaygood/ruff` repo. I.e., that would be a `push` event rather than a `pull_request` event.

But I'd also like these jobs that can't be run on forks disabled if I make a pull request against my own fork (which I don't do that often, but have done in the past, for testing purposes!) 😄

The expression I'm using here is basically identical to the example GitHub has in its docs for `if` conditions, FWIW: https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax#example-only-run-job-for-specific-repository. And it's also what we already use elsewhere in this workflow file: https://github.com/astral-sh/ruff/blob/4b7f184ab7848fee4f6641600b4f52a06e772e7a/.github/workflows/ci.yaml#L1015

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:242 on 2025-10-16 07:42_

Well... 

I guess the ideal would be to use a different runner based on whether this is a fork or not. But I don't know how easily this is possible (and it won't work with codspeed).

---

_@MichaReiser approved on 2025-10-16 07:42_

---

_Renamed from "Don't run codspeed or Depot CI jobs on forks" to "Don't use codspeed or depot runners in CI jobs on forks" by @AlexWaygood on 2025-10-16 11:54_

---

_Comment by @github-actions[bot] on 2025-10-16 11:56_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@AlexWaygood reviewed on 2025-10-16 11:56_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:242 on 2025-10-16 11:56_

> I guess the ideal would be to use a different runner based on whether this is a fork or not. But I don't know how easily this is possible (and it won't work with codspeed).

With Claude's help, I think I figured this out! (Except for Codspeed, of course)

---

_Comment by @github-actions[bot] on 2025-10-16 11:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @AlexWaygood on 2025-10-16 12:16_

---

_Closed by @AlexWaygood on 2025-10-16 12:16_

---

_Branch deleted on 2025-10-16 12:16_

---
