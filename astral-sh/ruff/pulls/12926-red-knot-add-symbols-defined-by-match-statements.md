```yaml
number: 12926
title: "[red-knot] Add symbols defined by `match` statements"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/match-stmt-symbols
created_at: 2024-08-16T10:56:24Z
updated_at: 2024-08-20T05:25:46Z
url: https://github.com/astral-sh/ruff/pull/12926
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] Add symbols defined by `match` statements

---

_Pull request opened by @dhruvmanila on 2024-08-16 10:56_

## Summary

This PR adds symbols introduced by `match` statements.

There are three patterns that introduces new symbols:
* `as` pattern
* Sequence pattern
* Mapping pattern

The recursive nature of the visitor makes sure that all symbols are added.

## Test Plan

Add test case for all types of patterns that introduces a symbol.

---

_Label `red-knot` added by @dhruvmanila on 2024-08-16 10:56_

---

_Comment by @codspeed-hq[bot] on 2024-08-16 11:01_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/match-stmt-symbols)

### Merging #12926 will **not alter performance**

<sub>Comparing <code>dhruv/match-stmt-symbols</code> (c08d870) with <code>main</code> (aefadde)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-08-16 11:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dhruvmanila on 2024-08-16 11:13_

---

_Review requested from @carljm by @dhruvmanila on 2024-08-16 11:13_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-16 11:13_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-16 11:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:1032 on 2024-08-16 15:35_

At runtime this pattern does actually bind the name `_`: 

```
>>> match 3:
...     case _: pass
...
>>> _
3
```

So perhaps we should as well?

---

_@carljm approved on 2024-08-16 15:37_

Looks great!

---

_@dhruvmanila reviewed on 2024-08-16 15:41_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index.rs`:1032 on 2024-08-16 15:41_

Wait what? I tried this locally and it gives me a `NameError` on 3.10 to 3.13:
```py
match 1:
    case _:
        print(_)
```

Running it from the command-line:
```console
✘ python3.13 type_inference/stmt_match.py
Traceback (most recent call last):
  File "/Users/dhruv/playground/ruff/type_inference/stmt_match.py", line 5, in <module>
    print(_)
          ^
NameError: name '_' is not defined
```

Maybe this is specific to the REPL?

---

_@AlexWaygood reviewed on 2024-08-16 15:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index.rs`:1032 on 2024-08-16 15:42_

The REPL always binds the `_` symbol to the result of whatever expression was last evaluated, so this could indeed be something REPL-specific

```pycon
Python 3.12.4 (main, Jun 12 2024, 09:54:41) [Clang 15.0.0 (clang-1500.3.9.4)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> 42
42
>>> _
42
```

---

_@carljm reviewed on 2024-08-16 15:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:1032 on 2024-08-16 15:45_

Ah, good call! Totally forgot about that REPL feature. That's a funny combination of separate special-casing around underscores, it really looked like the match statement was binding 😆 

---

_Merged by @dhruvmanila on 2024-08-20 05:16_

---

_Closed by @dhruvmanila on 2024-08-20 05:16_

---

_Branch deleted on 2024-08-20 05:16_

---
