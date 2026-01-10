```yaml
number: 14040
title: "[docs] Add rule short code to mkdocs tags"
type: pull_request
state: merged
author: hreeder
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/add-rule-code-to-tags
created_at: 2024-11-01T14:09:13Z
updated_at: 2024-11-01T16:06:06Z
url: https://github.com/astral-sh/ruff/pull/14040
synced_at: 2026-01-10T20:59:37Z
```

# [docs] Add rule short code to mkdocs tags

---

_Pull request opened by @hreeder on 2024-11-01 14:09_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR updates the metadata in the YAML frontmatter of the mkdocs documentation to include the rule short code as a tag, so it can be easily searched.
Ref: #13684

## Test Plan

<!-- How was it tested? -->
This has been tested locally using the documentation provided [here](https://docs.astral.sh/ruff/contributing/#mkdocs) for generating docs.

This generates docs that now have the tags section:
```markdown
---
description: Checks for abstract classes without abstract methods.
tags:
- B024
---

# abstract-base-class-without-abstract-method (B024)
... trimmed
```

I've also verified that this gives the ability to get straight to the page via search when serving mkdocs locally.


---

_Label `documentation` added by @AlexWaygood on 2024-11-01 14:12_

---

_Comment by @github-actions[bot] on 2024-11-01 14:32_

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

_Marked ready for review by @hreeder on 2024-11-01 14:33_

---

_@charliermarsh approved on 2024-11-01 15:41_

This is great, thank you!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-01 15:41_

---

_Merged by @charliermarsh on 2024-11-01 15:50_

---

_Closed by @charliermarsh on 2024-11-01 15:50_

---
