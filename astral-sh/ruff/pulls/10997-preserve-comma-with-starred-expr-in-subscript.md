```yaml
number: 10997
title: Preserve comma with starred expr in subscript
type: pull_request
state: merged
author: dhruvmanila
labels:
  - formatter
assignees: []
merged: true
base: dhruv/parser
head: dhruv/preserve-comma-with-pep-646
created_at: 2024-04-17T11:41:06Z
updated_at: 2024-04-17T12:58:02Z
url: https://github.com/astral-sh/ruff/pull/10997
synced_at: 2026-01-12T15:55:34Z
```

# Preserve comma with starred expr in subscript

---

_@dhruvmanila_

## Summary

There have been some grammar changes in [PEP 646] which were not accounted for in the old parser. The new parser has been updated with the correct AST. This is the case when there's a starred expression inside a subscript expression like the following example:

```python
data[*x]
```

This gives us the AST where the slice element is actually a tuple expression with one element (starred expression) instead of just a starred expression. Now, the formatter's current behavior is to always add a trailing comma in a tuple with a single element.

This PR updates the formatter to use the "preserve" behavior in trailing comma as well. So, trailing comma will not be added in the above example and if there's a trailing comma in the above example, it'll be preserved. This retains the current behavior without the AST change.

[PEP 646]: https://peps.python.org/pep-0646/#change-1-star-expressions-in-indexes

## Test Plan

Run `cargo insta test -p ruff_python_formatter` and make sure there are no changes.

---

_Label `formatter` added by @dhruvmanila on 2024-04-17 11:41_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-17 11:41_

---

_Comment by @codspeed-hq[bot] on 2024-04-17 11:46_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/preserve-comma-with-pep-646)

### Merging #10997 will **not alter performance**

<sub>Comparing <code>dhruv/preserve-comma-with-pep-646</code> (4a382e5) with <code>dhruv/parser</code> (c30057a)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@MichaReiser reviewed on 2024-04-17 12:10_

We should add a few tests for this new syntax. Or are there pre-existing tests for this?

---

_Comment by @dhruvmanila on 2024-04-17 12:13_

> We should add a few tests for this new syntax. Or are there pre-existing tests for this?

There are: https://github.com/astral-sh/ruff/blob/a2e71a8460b07761bd280eac3ab9dd4bd013ba92/crates/ruff_python_formatter/resources/test/fixtures/black/cases/pep_646.py although it seems there's none for trailing commas. I'll add them.

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-17 12:25_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@statement__function.py.snap`:1029 on 2024-04-17 12:39_

Is it intentional that you test here what happens if you have a trailing parameter comma? Should it instead test if there's a trailing tuple comma (in the subscript?)

---

_@MichaReiser approved on 2024-04-17 12:39_

---

_@dhruvmanila reviewed on 2024-04-17 12:43_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/tests/snapshots/format@statement__function.py.snap`:1029 on 2024-04-17 12:43_

Yes, in the presence of variadic generics, the parameter formatting shouldn't change even though this PR doesn't touch that part of the code. It's mostly to make sure of any future changes, probably not needed.

---

_Merged by @dhruvmanila on 2024-04-17 12:48_

---

_Closed by @dhruvmanila on 2024-04-17 12:48_

---

_Branch deleted on 2024-04-17 12:48_

---

_Comment by @github-actions[bot] on 2024-04-17 12:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
