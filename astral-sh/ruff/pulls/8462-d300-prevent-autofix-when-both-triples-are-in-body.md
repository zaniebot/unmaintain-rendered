```yaml
number: 8462
title: "`D300`: prevent autofix when both triples are in body"
type: pull_request
state: merged
author: T-256
labels:
  - bug
assignees: []
merged: true
base: main
head: D300-triples
created_at: 2023-11-03T04:59:35Z
updated_at: 2023-11-03T16:56:00Z
url: https://github.com/astral-sh/ruff/pull/8462
synced_at: 2026-01-12T15:55:26Z
```

# `D300`: prevent autofix when both triples are in body

---

_@T-256_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Addresses https://github.com/astral-sh/ruff/issues/8402#issuecomment-1788782750


<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Added associated test
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-11-03 05:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @charliermarsh on 2023-11-03 16:22_

---

_Comment by @T-256 on 2023-11-03 16:29_

Sorry for disabling auto-merge, but I think this should have autofix:
```py
def contains_triples(t):
    '''(\'''|\""")'''
```
Into
```py
def contains_triples(t):
    """(\'''|\""")"""
```

---

_Comment by @charliermarsh on 2023-11-03 16:31_

I think this is fine to merge as-is because we already get that "wrong" today for the simpler case, I think:

```python
def contains_triples(t):
    '''(\""")'''
```

Getting it right requires that we parse the entire docstring to track escapes.

---

_Comment by @T-256 on 2023-11-03 16:44_

> Getting it right requires that we parse the entire docstring to track escapes

Correct, I was looking for escape-processed of docstring content to implement it, but didn't find a proper way.
BTW I added a todo example, I also think it's safe to merge for now.

---

_Merged by @charliermarsh on 2023-11-03 16:49_

---

_Closed by @charliermarsh on 2023-11-03 16:49_

---

_Comment by @charliermarsh on 2023-11-03 16:51_

@T-256 - We have some stuff like that in `crates/ruff_linter/src/rules/flake8_quotes/rules/avoidable_escaped_quote.rs`.

---
