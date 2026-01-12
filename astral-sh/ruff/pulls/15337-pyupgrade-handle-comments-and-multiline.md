```yaml
number: 15337
title: "[`pyupgrade`] Handle comments and multiline expressions correctly (`UP037`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - fixes
assignees: []
merged: true
base: main
head: UP037
created_at: 2025-01-08T07:30:11Z
updated_at: 2025-01-10T09:39:15Z
url: https://github.com/astral-sh/ruff/pull/15337
synced_at: 2026-01-12T15:55:51Z
```

# [`pyupgrade`] Handle comments and multiline expressions correctly (`UP037`)

---

_@InSyncWithFoo_

## Summary

Resolves #7102.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-08 07:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/quoted_annotation.rs`:80 on 2025-01-08 14:18_

```suggestion
    let placeholder_range = TextRange::up_to(annotation.text_len());
    let spans_multiple_lines = annotation.count_lines(placeholder_range) > 1;
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP037_2.pyi.snap`:87 on 2025-01-08 14:36_

Are we adding the parentheses in case the quoted annotation is in an unquoted context (e.g. annotated assignment)? Would it be possible to reduce the cases in which we add the parentheses?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/quoted_annotation.rs`:81 on 2025-01-08 14:36_


```suggestion
    let spans_multiple_lines = annotation.contains_line_break(placeholder_range);
```

---

_@MichaReiser reviewed on 2025-01-08 14:36_

Sweet

---

_Label `fixes` added by @MichaReiser on 2025-01-08 14:37_

---

_@InSyncWithFoo reviewed on 2025-01-08 15:09_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP037_2.pyi.snap`:87 on 2025-01-08 15:09_

Here's an(other) edge case where parentheses are necessary:

```python
def f(a: '''
	list[int]
''' = []): ...
```

As for reducing, that would require some complex parsing logic. For example, here are two valid annotations that cannot simply be unquoted:

```python
a: '''list
[int]''' = [42]

a: '''\\
list[int]''' = [42]
```

All of these have been added as testcases.

---

_@MichaReiser reviewed on 2025-01-08 16:58_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP037_2.pyi.snap`:87 on 2025-01-08 16:58_

That makes sense. I'm suggesting adding a simple check that detects if we're currently in a parenthesized context (e.g., is this an annotation for a parameter, is the parent a subscript, ...) and, if so, avoid adding the parentheses. 

Because this

```py
def f(a: ''' 
	list[int]
	 ''' = []): ...
```

can be safely rewritten to, without the need for extra parentheses:

```py
def f(a: 
	list[int]
	 = []): ...
```


---

_Merged by @MichaReiser on 2025-01-10 07:46_

---

_Closed by @MichaReiser on 2025-01-10 07:46_

---

_Branch deleted on 2025-01-10 09:39_

---
