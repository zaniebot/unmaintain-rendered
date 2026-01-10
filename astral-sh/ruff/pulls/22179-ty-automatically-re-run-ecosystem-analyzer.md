```yaml
number: 22179
title: "[ty] Automatically re-run ecosystem-analyzer workflow on subsequent pushes to a PR, if the PR has the `ecosystem-analyzer` label"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/ecosystem-analyzer-workflow
created_at: 2025-12-24T17:31:05Z
updated_at: 2025-12-29T09:08:05Z
url: https://github.com/astral-sh/ruff/pull/22179
synced_at: 2026-01-10T16:36:18Z
```

# [ty] Automatically re-run ecosystem-analyzer workflow on subsequent pushes to a PR, if the PR has the `ecosystem-analyzer` label

---

_Pull request opened by @AlexWaygood on 2025-12-24 17:31_

## Summary

This PR reworks our ecosystem-analyzer workflow so that it automatically reruns if a PR with the `ecosystem-analyzer` label has new commits pushed to it, or is reopened after previously being closed. It's currently easy to forget that you need to remove and re-add the label to trigger a fresh workflow run, which can then mean that there are stale (misleading) results in the PR comment posted by the bot. It also means that it takes longer for CI to finish than it would otherwise, because it might be a few minutes after pushing new commits to the PR before you remember that you also need to remove and re-add the label.

To write this PR, I consulted:
- The GitHub workflow trigger documentation: https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#pull_request
- This Stack Overflow answer: https://stackoverflow.com/a/59588725/13990016

## Test Plan

I experimented with pushing commits to this PR and closing/reopening it, and both of these actions triggered fresh runs of the ecosystem-analyzer worfklow when the label was present on the PR. However, removing the label again meant that the workflow was no longer triggered by these actions.


---

_Label `ci` added by @AlexWaygood on 2025-12-24 17:31_

---

_Label `ty` added by @AlexWaygood on 2025-12-24 17:31_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-24 17:39_

---

_Closed by @AlexWaygood on 2025-12-24 17:43_

---

_Reopened by @AlexWaygood on 2025-12-24 17:43_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-24 17:45_

---

_Closed by @AlexWaygood on 2025-12-24 17:46_

---

_Reopened by @AlexWaygood on 2025-12-24 17:46_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-24 17:46_

---

_Comment by @astral-sh-bot[bot] on 2025-12-24 17:51_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 0 | 1 | 11 |
| `invalid-assignment` | 0 | 0 | 5 |
| `invalid-argument-type` | 0 | 0 | 4 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **1** | **1** | **20** |


**[Full report with detailed diff](https://3de597d7.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://3de597d7.ty-ecosystem-ext.pages.dev/timing))



---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-24 17:52_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-12-24 17:52_

---

_Marked ready for review by @AlexWaygood on 2025-12-24 17:52_

---

_@MichaReiser approved on 2025-12-25 17:09_

---

_Merged by @AlexWaygood on 2025-12-25 17:41_

---

_Closed by @AlexWaygood on 2025-12-25 17:41_

---

_Branch deleted on 2025-12-25 17:41_

---

_Comment by @sharkdp on 2025-12-29 09:08_

Cool, thank you!

---
