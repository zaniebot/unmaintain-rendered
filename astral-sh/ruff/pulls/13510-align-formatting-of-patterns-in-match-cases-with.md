```yaml
number: 13510
title: Align formatting of patterns in match-cases with expression formatting in clause headers
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: micha/maybe-parenthesize-pattern
created_at: 2024-09-25T11:32:53Z
updated_at: 2024-09-26T06:35:24Z
url: https://github.com/astral-sh/ruff/pull/13510
synced_at: 2026-01-10T20:59:36Z
```

# Align formatting of patterns in match-cases with expression formatting in clause headers

---

_Pull request opened by @MichaReiser on 2024-09-25 11:32_

## Summary

This PR implements a new preview style that aligns the match-case formatting to how we format expressions in other clause headers. 

**Stable**

```python
match x:
    case (
        A
        | [
            aaaaaaaaaaaaaaaaaaaaaaaaaaaa,
            bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb,
            ccccccccccccccccccccccccccccccccc,
        ]
    ):
        pass
```

**Preview**

```python
match x:
    case A | [
        aaaaaaaaaaaaaaaaaaaaaaaaaaaa,
        bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb,
        ccccccccccccccccccccccccccccccccc,
    ]:
        pass
```

The implementation is very close to the same implementation for expressions except that it doesn't support different `OptionalParentheses` layouts. 

Fixes https://github.com/astral-sh/ruff/issues/6933

This PR does not yet implement the Black preview style for parenthesizing the if-guard

## Test plan

Added tests

---

_Label `formatter` added by @MichaReiser on 2024-09-25 11:33_

---

_Label `preview` added by @MichaReiser on 2024-09-25 11:33_

---

_Comment by @github-actions[bot] on 2024-09-25 11:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @MichaReiser on 2024-09-25 15:19_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-09-25 15:19_

---

_@charliermarsh approved on 2024-09-25 21:44_

Nice, good change to bring more standardization to the formatting here. This always bothered me.

---

_Merged by @MichaReiser on 2024-09-26 06:35_

---

_Closed by @MichaReiser on 2024-09-26 06:35_

---

_Branch deleted on 2024-09-26 06:35_

---
