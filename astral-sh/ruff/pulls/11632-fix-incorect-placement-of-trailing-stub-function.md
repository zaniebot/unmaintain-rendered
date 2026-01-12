```yaml
number: 11632
title: Fix incorect placement of trailing stub function comments
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: fix-function-trailing-comments
created_at: 2024-05-31T08:54:28Z
updated_at: 2024-05-31T12:13:00Z
url: https://github.com/astral-sh/ruff/pull/11632
synced_at: 2026-01-12T15:55:38Z
```

# Fix incorect placement of trailing stub function comments

---

_@MichaReiser_

## Summary

This fixes an issue in the formatter where it reorder comments or, worse, moved trailing stub function comments into the next's function body. 

The problem is that the formatter uses `line_suffix` for the trailing comments and the line suffixes are only flushed after the next newline. However, there's no such newline directly after the comment(s) because the formatter now allows newlines to be omitted between stub functions. 

The fix of this PR is to match Black's behavior to insert the empty lines if a collapsed stub has a trailing comment. I'm not very fond of the empty lines, because I think the comments should remain close to the stub body and not be separated by empty lines but it is consistent with how we format trailing function comments in other places and matches Black.

Closes https://github.com/astral-sh/ruff/issues/11569

## Test Plan

See added tests

---

_Comment by @github-actions[bot] on 2024-05-31 09:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `bug` added by @MichaReiser on 2024-05-31 09:18_

---

_Label `formatter` added by @MichaReiser on 2024-05-31 09:18_

---

_@MichaReiser reviewed on 2024-05-31 09:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:244 on 2024-05-31 09:18_

This is the actual fix

---

_Marked ready for review by @MichaReiser on 2024-05-31 09:18_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-05-31 09:19_

---

_Review requested from @dhruvmanila by @MichaReiser on 2024-05-31 09:19_

---

_Renamed from "fix function trailing comments" to "Fix misplacement of trailing stub function comments" by @MichaReiser on 2024-05-31 09:19_

---

_Renamed from "Fix misplacement of trailing stub function comments" to "Fix incorecty placement of trailing stub function comments" by @MichaReiser on 2024-05-31 09:19_

---

_Renamed from "Fix incorecty placement of trailing stub function comments" to "Fix incorect placement of trailing stub function comments" by @MichaReiser on 2024-05-31 09:19_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/stub_functions_trailing_comments.py`:25 on 2024-05-31 11:10_

nit: maybe also add a comment before end of file? I think that should also give the same behavior where the formatter will add two empty lines between the function (`baz`) and the comment.

---

_@dhruvmanila approved on 2024-05-31 11:11_

---

_Merged by @MichaReiser on 2024-05-31 12:06_

---

_Closed by @MichaReiser on 2024-05-31 12:06_

---

_Branch deleted on 2024-05-31 12:06_

---
