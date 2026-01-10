```yaml
number: 13234
title: Add meta descriptions to rule pages
type: pull_request
state: merged
author: calumy
labels:
  - documentation
assignees: []
merged: true
base: main
head: add-meta-descriptions-to-rule-pages
created_at: 2024-09-03T19:38:41Z
updated_at: 2024-09-09T14:02:05Z
url: https://github.com/astral-sh/ruff/pull/13234
synced_at: 2026-01-10T21:38:32Z
```

# Add meta descriptions to rule pages

---

_Pull request opened by @calumy on 2024-09-03 19:38_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR updates the `scripts/generate_mkdocs.py` to add meta descriptions to each rule as well as a fallback `site_description`. 

I was initially planning to add this to `generate_docs.rs`; however running `mdformat` on the rules caused the format of the additional description to change into a state that mkdocs could not handle. 

Fixes #13197 

## Test Plan

- Run  `python scripts/generate_mkdocs.py` to build the documentation
- Run `mkdocs serve -f mkdocs.public.yml` to serve the docs site locally
- Navigate to a rule on both the local site and the current production site and note the addition of the description head tag. For example:
  - http://127.0.0.1:8000/ruff/rules/unused-import/
  ![image](https://github.com/user-attachments/assets/f47ae4fa-fe5b-42e1-8874-cb36a2ef2c9b)
  - https://docs.astral.sh/ruff/rules/unused-import/
  ![image](https://github.com/user-attachments/assets/6a650bff-2fcb-4df2-9cb6-40f66a2a5b8a)


---

_Comment by @github-actions[bot] on 2024-09-03 19:57_

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

_Label `documentation` added by @AlexWaygood on 2024-09-03 22:13_

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-09-04 04:43_

---

_Merged by @charliermarsh on 2024-09-09 14:01_

---

_Closed by @charliermarsh on 2024-09-09 14:02_

---

_Comment by @charliermarsh on 2024-09-09 14:02_

Thank you! That's great.

---
