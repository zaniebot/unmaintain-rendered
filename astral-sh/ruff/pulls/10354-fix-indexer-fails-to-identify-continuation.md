```yaml
number: 10354
title: "Fix Indexer fails to identify continuation preceded by newline #10351"
type: pull_request
state: merged
author: augustelalande
labels:
  - bug
assignees: []
merged: true
base: main
head: indexer-bug
created_at: 2024-03-12T02:59:16Z
updated_at: 2024-03-12T04:54:51Z
url: https://github.com/astral-sh/ruff/pull/10354
synced_at: 2026-01-10T22:47:01Z
```

# Fix Indexer fails to identify continuation preceded by newline #10351

---

_Pull request opened by @augustelalande on 2024-03-12 02:59_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #10351

It seems the bug was caused by this section of code
https://github.com/astral-sh/ruff/blob/b669306c87ec304182fd50872b50858e2faf0262/crates/ruff_python_index/src/indexer.rs#L55-L58

It's true that newline tokens cannot be immediately followed by line continuations, but only outside parentheses. e.g. the exception
```
(
    1
    \
    + 2)
```

But why was this check put there in the first place? Is it guarding against something else?



## Test Plan

New test was added to indexer


---

_Comment by @github-actions[bot] on 2024-03-12 03:17_

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

_Comment by @charliermarsh on 2024-03-12 03:19_

Can you find anything via `git blame` about why this is here?

---

_Comment by @charliermarsh on 2024-03-12 03:19_

(It's possible that something changed in the lexer over time that made this not necessary.)

---

_Comment by @augustelalande on 2024-03-12 03:37_

This was the original implementation
https://github.com/astral-sh/ruff/blob/3791ca721aca9af04929d8caaa4678f0e252fbc3/src/source_code/indexer.rs#L22-L48
which had slightly different logic and needed the check to avoid flagging every newline. The checks just got kept through the edits.

Note: This implementation would also have failed to parse the code in question correctly.

---

_Comment by @charliermarsh on 2024-03-12 04:34_

Okay, I think it was this change, long ago: https://github.com/RustPython/Parser/pull/27

---

_Merged by @charliermarsh on 2024-03-12 04:35_

---

_Closed by @charliermarsh on 2024-03-12 04:35_

---

_Label `bug` added by @charliermarsh on 2024-03-12 04:35_

---

_Branch deleted on 2024-03-12 04:54_

---
