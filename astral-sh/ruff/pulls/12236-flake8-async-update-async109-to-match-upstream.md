```yaml
number: 12236
title: "[`flake8-async`] Update `ASYNC109` to match upstream"
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: async109
created_at: 2024-07-08T05:27:06Z
updated_at: 2024-07-09T04:53:02Z
url: https://github.com/astral-sh/ruff/pull/12236
synced_at: 2026-01-12T15:55:40Z
```

# [`flake8-async`] Update `ASYNC109` to match upstream

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Update the name of `ASYNC109` to match [upstream](https://flake8-async.readthedocs.io/en/latest/rules.html).

Also update to the functionality to match upstream by supporting additional context managers from `asyncio` and `anyio`. This doesn't change any of the detection functionality, but recommends additional context managers from `asyncio` and `anyio` depending on context.

Part of https://github.com/astral-sh/ruff/issues/12039.

## Test Plan

Added fixture for asyncio recommendation


---

_Label `rule` added by @MichaReiser on 2024-07-08 07:50_

---

_@MichaReiser approved on 2024-07-08 07:54_

Thank you. This looks good to me. 

The only change is that I think we should gate this behind preview-mode because it increases the scope of a non-preview rule. 

Would you mind to extend the PR summary with a short explanation of what "match upstream" means (support anyio and asyncio). The person who has to write the changelog and people navigating to the PR from the changelog might find that helpful.

---

_Comment by @augustelalande on 2024-07-08 19:28_

Ok I restricted the rule to trio only unless preview is enabled

---

_Comment by @augustelalande on 2024-07-08 19:30_

I expanded the PR summary to make things a little clearer.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-09 02:39_

---

_@charliermarsh approved on 2024-07-09 04:10_

---

_Label `preview` added by @charliermarsh on 2024-07-09 04:10_

---

_Merged by @charliermarsh on 2024-07-09 04:14_

---

_Closed by @charliermarsh on 2024-07-09 04:14_

---

_Branch deleted on 2024-07-09 04:53_

---
