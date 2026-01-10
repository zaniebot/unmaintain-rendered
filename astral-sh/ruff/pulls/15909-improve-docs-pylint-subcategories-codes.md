```yaml
number: 15909
title: "Improve Docs: Pylint subcategories' codes"
type: pull_request
state: merged
author: VascoSch92
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/improve_PL_section
created_at: 2025-02-03T11:56:10Z
updated_at: 2025-02-03T12:54:52Z
url: https://github.com/astral-sh/ruff/pull/15909
synced_at: 2026-01-10T19:57:22Z
```

# Improve Docs: Pylint subcategories' codes

---

_Pull request opened by @VascoSch92 on 2025-02-03 11:56_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR addresses the issue #15168. 

Now, the documentation looks like:
![Screenshot 2025-02-03 at 12 30 00](https://github.com/user-attachments/assets/691e8ba8-b3d7-49bc-b2eb-e65289d21ec6)
as discussed in the issue.

However, I’m not sure if I implemented the best solution. If another set of rules with subcategories is added, this fix won’t work unless updated manually.

Let me know what you think.



---

_Label `documentation` added by @AlexWaygood on 2025-02-03 11:58_

---

_Comment by @github-actions[bot] on 2025-02-03 12:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@MichaReiser approved on 2025-02-03 12:53_

Thanks. I think this is fine. The whole upstream thing is rather hacky and I doubt we'll add any new once with more than one code (and if we do, we should rework the system)

---

_Merged by @MichaReiser on 2025-02-03 12:53_

---

_Closed by @MichaReiser on 2025-02-03 12:53_

---

_Branch deleted on 2025-02-03 12:54_

---
