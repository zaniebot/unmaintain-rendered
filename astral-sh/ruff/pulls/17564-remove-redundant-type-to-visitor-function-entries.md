```yaml
number: 17564
title: "Remove redundant `type_to_visitor_function` entries"
type: pull_request
state: merged
author: Glyphack
labels:
  - internal
assignees: []
merged: true
base: main
head: generator-skip-visit
created_at: 2025-04-22T19:49:27Z
updated_at: 2025-04-23T07:42:00Z
url: https://github.com/astral-sh/ruff/pull/17564
synced_at: 2026-01-12T15:56:02Z
```

# Remove redundant `type_to_visitor_function` entries

---

_@Glyphack_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Based on the discussion in the #17180 I removed some redundant entries from the script.

I'm open to ideas on what else to do to make it better. I think that another improvement is to adjust the not auto generated code so we can simplify the generator. For example `visit_body` can become `visit_stmt` where we iterate over statements and call it. Then that entry can be removed. Same as with other entries in the table that accept a sequence.

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-04-22 20:00_

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

_@MichaReiser approved on 2025-04-23 07:26_

Thanks

---

_Label `internal` added by @MichaReiser on 2025-04-23 07:26_

---

_Merged by @MichaReiser on 2025-04-23 07:27_

---

_Closed by @MichaReiser on 2025-04-23 07:27_

---

_Branch deleted on 2025-04-23 07:42_

---
