```yaml
number: 11362
title: "[pyflakes] Update docs to describe WAI behavior (F541)"
type: pull_request
state: merged
author: dougthor42
labels:
  - documentation
assignees: []
merged: true
base: main
head: f541-doc-gh11357
created_at: 2024-05-10T15:00:17Z
updated_at: 2024-05-11T20:35:06Z
url: https://github.com/astral-sh/ruff/pull/11362
synced_at: 2026-01-10T22:05:26Z
```

# [pyflakes] Update docs to describe WAI behavior (F541)

---

_Pull request opened by @dougthor42 on 2024-05-10 15:00_

Addresses this comment:
https://github.com/astral-sh/ruff/issues/11357#issuecomment-2104714029


## Summary

The docs for F541 did not mention some surprising, but WAI, behavior regarding implicit string concatenation. Update the docs to describe the behavior.

Here's how things rendered for me locally:

![image](https://github.com/astral-sh/ruff/assets/5386897/32067121-b190-4268-b987-ff37df11a618)


## Test Plan

N/A


---

_Comment by @github-actions[bot] on 2024-05-10 15:14_

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

_Comment by @dougthor42 on 2024-05-10 17:22_

For the mkdocs formatting failure:

The multi-line string makes things more obvious IMO. How should I disable the autoformatter for this case? Or is the one-line implicit string concatenation easy enough to read?

---

_Label `documentation` added by @charliermarsh on 2024-05-10 18:52_

---

_Comment by @charliermarsh on 2024-05-10 18:56_

We have a way to exclude rules from that check -- I will add it.

---

_@charliermarsh approved on 2024-05-10 19:03_

Thanks!

---

_Merged by @charliermarsh on 2024-05-10 19:10_

---

_Closed by @charliermarsh on 2024-05-10 19:10_

---

_Branch deleted on 2024-05-11 20:35_

---
