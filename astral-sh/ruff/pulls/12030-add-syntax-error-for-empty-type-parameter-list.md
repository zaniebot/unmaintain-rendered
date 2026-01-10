```yaml
number: 12030
title: Add syntax error for empty type parameter list
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
assignees: []
merged: true
base: main
head: dhruv/empty-type-params
created_at: 2024-06-25T16:31:08Z
updated_at: 2024-06-26T02:40:36Z
url: https://github.com/astral-sh/ruff/pull/12030
synced_at: 2026-01-10T21:56:00Z
```

# Add syntax error for empty type parameter list

---

_Pull request opened by @dhruvmanila on 2024-06-25 16:31_

## Summary

(I'm pretty sure I added this in the parser re-write but must've got lost in the rebase?)

This PR raises a syntax error if the type parameter list is empty.

As per the grammar, there should be at least one type parameter:
```
type_params: 
    | invalid_type_params
    | '[' type_param_seq ']' 

type_param_seq: ','.type_param+ [','] 
```

Verified via the builtin `ast` module as well:
```console    
$ python3.13 -m ast parser/_.py
Traceback (most recent call last):
  [..]
  File "parser/_.py", line 1
    def foo[]():
            ^
SyntaxError: Type parameter list cannot be empty
```

## Test Plan

Add inline test cases and update the snapshots.


---

_Label `parser` added by @dhruvmanila on 2024-06-25 16:31_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-06-25 16:31_

---

_Comment by @github-actions[bot] on 2024-06-25 16:51_

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

_@MichaReiser approved on 2024-06-25 20:12_

---

_Merged by @dhruvmanila on 2024-06-26 02:40_

---

_Closed by @dhruvmanila on 2024-06-26 02:40_

---

_Branch deleted on 2024-06-26 02:40_

---
