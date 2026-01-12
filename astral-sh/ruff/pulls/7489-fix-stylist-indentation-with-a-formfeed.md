```yaml
number: 7489
title: Fix stylist indentation with a formfeed
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: stylist-formfeed
created_at: 2023-09-18T10:03:50Z
updated_at: 2023-09-19T10:01:17Z
url: https://github.com/astral-sh/ruff/pull/7489
synced_at: 2026-01-12T02:39:10Z
```

# Fix stylist indentation with a formfeed

---

_Pull request opened by @konstin on 2023-09-18 10:03_

**Summary** In python, a formfeed is technically undefined behaviour (https://docs.python.org/3/reference/lexical_analysis.html#indentation):
> A formfeed character may be present at the start of the line; it will be ignored for
> the indentation calculations above. Formfeed characters occurring elsewhere in the
> leading whitespace have an undefined effect (for instance, they may reset the space
> count to zero).

In practice, they just reset the indentation:

https://github.com/python/cpython/blob/df8b3a46a7aa369f246a09ffd11ceedf1d34e921/Parser/tokenizer.c#L1819-L1821 https://github.com/astral-sh/ruff/blob/a41bb2733fe75a71f4cf6d4bb21e659fc4630b30/crates/ruff_python_parser/src/lexer.rs#L664-L667

The stylist didn't handle formfeeds previously and would produce invalid indents. The remedy is to cut everything before a form feed.

Checks box for https://github.com/astral-sh/ruff/issues/7455#issuecomment-1722458825

**Test Plan** Unit test for the stylist and a regression test for the rule

---

_@MichaReiser approved on 2023-09-18 10:05_

---

_Comment by @github-actions[bot] on 2023-09-18 10:20_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Merged by @konstin on 2023-09-19 10:01_

---

_Closed by @konstin on 2023-09-19 10:01_

---

_Branch deleted on 2023-09-19 10:01_

---
