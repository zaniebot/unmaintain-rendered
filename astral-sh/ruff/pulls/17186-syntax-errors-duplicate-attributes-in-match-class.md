```yaml
number: 17186
title: "[syntax-errors] Duplicate attributes in match class pattern"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: brent/syn-duplicate-pattern-class-attribute
created_at: 2025-04-03T20:35:38Z
updated_at: 2025-04-03T21:55:40Z
url: https://github.com/astral-sh/ruff/pull/17186
synced_at: 2026-01-10T19:40:37Z
```

# [syntax-errors] Duplicate attributes in match class pattern

---

_Pull request opened by @ntBre on 2025-04-03 20:35_

Summary
--

Detects duplicate attributes in a `match` class pattern:

```python
match x:
    case Class(x=1, x=2): ...
```

which are more analogous to the similar check for mapping patterns than to the
multiple assignments rule.

I also realized that both this and the mapping check would only work on
top-level patterns, despite the possibility that they can be nested inside other
patterns:

```python
match x:
    case [{"x": 1, "x": 2}]: ...  # false negative in the old version
```

and moved these checks into the recursive pattern visitor instead.

I also tidied up some of the names like the `multiple_case_assignment` function
and the `MultipleCaseAssignmentVisitor`, which are now doing more than checking
for multiple assignments.

Test Plan
--

New inline tests for both classes and mappings.

This is currently stacked on #17184.


---

_Label `rule` added by @ntBre on 2025-04-03 20:35_

---

_Label `preview` added by @ntBre on 2025-04-03 20:35_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-03 20:35_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-03 20:35_

---

_Comment by @github-actions[bot] on 2025-04-03 20:44_

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

_@dhruvmanila approved on 2025-04-03 20:48_

Looks good. Can we add one or two test cases which uses nested and mixed patterns? For example, a class pattern that contains mapping pattern and another class pattern. This is to make sure that the patterns are visited correctly.

---

_Comment by @ntBre on 2025-04-03 20:51_

Sure, I threw in the list case as a minimal example, but I'm happy to expand that a bit. Thanks for the reviews!

---

_Merged by @ntBre on 2025-04-03 21:55_

---

_Closed by @ntBre on 2025-04-03 21:55_

---

_Branch deleted on 2025-04-03 21:55_

---
