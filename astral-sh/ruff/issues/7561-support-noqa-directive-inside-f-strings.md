```yaml
number: 7561
title: Support NoQA directive inside f-strings
type: issue
state: open
author: dhruvmanila
labels:
  - suppression
  - python312
assignees: []
created_at: 2023-09-21T02:30:00Z
updated_at: 2023-10-05T05:12:56Z
url: https://github.com/astral-sh/ruff/issues/7561
synced_at: 2026-01-10T11:09:49Z
```

# Support NoQA directive inside f-strings

---

_Issue opened by @dhruvmanila on 2023-09-21 02:30_

Currently, the NoQA directives are detected and added on the last line of a multi-line string / f-string. With PEP 701, we can add comments inside f-strings, so we should support NoQA directives inside f-strings.

Relevant discussion: https://github.com/astral-sh/ruff/issues/7291#issuecomment-1722839331

### Implementation

The tool creates [NoQA mapping] which is an offset from the line where the NoQA comment needs to be added and the line where the comment _can_ be added. For the following code snippet, as no comment can go after a line continuation character, there's an offset created from byte offset at the start of line 1 to a byte offset at the start of line 2:

```python
  foo = x \
# ^
  	+ y
# ^
```

This way the NoQA directive will be added at the end of the second line instead of the first. There are some challenges around where to add the comment for f-string w.r.t. the curly brace which is mentioned in the linked discussion.

[NoQA mapping]: https://github.com/astral-sh/ruff/blob/b685ee47499f22a9c1c21657d8a26fa414ee99de/crates/ruff_linter/src/noqa.rs#L731-L736

- [ ] Update the documentation to reflect this change

---

_Label `noqa` added by @dhruvmanila on 2023-09-21 02:30_

---

_Label `python312` added by @dhruvmanila on 2023-09-21 02:30_

---
