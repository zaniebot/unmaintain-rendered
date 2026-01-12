```yaml
number: 16080
title: "[`pylint`] Correct ordering of arguments in fix for `if-stmt-min-max` (`PLR1730`)"
type: pull_request
state: merged
author: VascoSch92
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-min-max-stm
created_at: 2025-02-10T15:07:18Z
updated_at: 2025-02-12T09:27:50Z
url: https://github.com/astral-sh/ruff/pull/16080
synced_at: 2026-01-12T15:55:53Z
```

# [`pylint`] Correct ordering of arguments in fix for `if-stmt-min-max` (`PLR1730`)

---

_@VascoSch92_

The PR addresses the issue #16040 .

---

The logic used into the rule is the following:

Suppose to have an expression of the form 

```python
if a cmp b:
    c = d
```
where `a`,` b`, `c` and `d` are Python obj and `cmp`  one of `<`, `>`, `<=`, `>=`.

Then:

- `if a=c and b=d`
    
    - if `<=` fix with `a = max(b, a)`
    - if `>=`  fix with `a = min(b, a)`
    - if `>` fix with `a = min(a, b)`
    - if `<` fix with `a = max(a, b)`

- `if a=d and b=c`

    - if `<=` fix with `b = min(a, b)`
    - if `>=`  fix with `b = max(a, b)`
    - if `>` fix with `b = max(b, a)`
    - if `<` fix with `b = min(b, a)`
 
- do nothing, i.e., we cannot fix this case.

---

In total we have 8 different and possible cases.

```

| Case  | Expression       | Fix           |
|-------|------------------|---------------|
| 1     | if a >= b: a = b | a = min(b, a) |
| 2     | if a <= b: a = b | a = max(b, a) |
| 3     | if a <= b: b = a | b = min(a, b) |
| 4     | if a >= b: b = a | b = max(a, b) |
| 5     | if a > b: a = b  | a = min(a, b) |
| 6     | if a < b: a = b  | a = max(a, b) |
| 7     | if a < b: b = a  | b = min(b, a) |
| 8     | if a > b: b = a  | b = max(b, a) |
```

I added them in the tests. 

Please double-check that I didn't make any mistakes. It's quite easy to mix up > and <.


---

_Comment by @github-actions[bot] on 2025-02-10 15:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @dylwil3 on 2025-02-10 19:18_

---

_Label `fixes` added by @dylwil3 on 2025-02-10 19:18_

---

_Renamed from "[pylint] Fix rule PLR17130" to "[`pylint`] Correct ordering of arguments in fix for `if-stmt-min-max` (`PLR1730`)" by @dylwil3 on 2025-02-10 19:21_

---

_@MichaReiser reviewed on 2025-02-11 09:00_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/if_stmt_min_max.rs`:134 on 2025-02-11 09:00_

I think this is incorrect and should instead be `false` to match your table from the PR summary.


Please also carefully review the snapshot changes.

---

_@VascoSch92 reviewed on 2025-02-11 10:23_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/pylint/rules/if_stmt_min_max.rs`:134 on 2025-02-11 10:23_

In this case where are in the situation:
```python
if a <= b:
    b = a
```
which is fixed by `b = min(a, b)`.

So you are right ;-) 

> Please also carefully review the snapshot changes.

I corrected the snapshot and I double-checked all the other changes I did. Everything follow the table posted in the description of the PR. 

---

_@MichaReiser approved on 2025-02-12 09:24_

Thank you

---

_Merged by @MichaReiser on 2025-02-12 09:27_

---

_Closed by @MichaReiser on 2025-02-12 09:27_

---
