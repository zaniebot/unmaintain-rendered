```yaml
number: 11934
title: ☂️ Syntax errors raised by the compiler
type: issue
state: closed
author: dhruvmanila
labels:
  - tracking
assignees: []
created_at: 2024-06-19T03:26:50Z
updated_at: 2025-04-23T19:46:06Z
url: https://github.com/astral-sh/ruff/issues/11934
synced_at: 2026-01-12T15:54:51Z
```

# ☂️ Syntax errors raised by the compiler

---

_@dhruvmanila_

This is an umbrella issue to collect all the syntax errors that are raised by the CPython compiler. These are different than the ones that are raised by the parser. IOW, the source code which raises these errors are valid as per the grammar.

- [x] [Duplicate parameter names](https://github.com/astral-sh/ruff/pull/16858#r2005396519) (https://github.com/astral-sh/ruff/pull/17131)
- [x] `await` outside async function (PLE1142) (https://github.com/astral-sh/ruff/pull/17363)
- [x] `yield` outside function (F704) (https://github.com/astral-sh/ruff/pull/17298)
- [x] `return` outside function (F706) (https://github.com/astral-sh/ruff/pull/17300)
- [x] `await` in annotations (extending the `yield` and named expression handling from #17101) (https://github.com/astral-sh/ruff/pull/17282)
- [x] PEP 649 annotations in annotated assignments (https://github.com/astral-sh/ruff/pull/17283)
- [x] https://github.com/astral-sh/ruff/issues/11118 (https://github.com/astral-sh/ruff/pull/17101)
  - [PEP 649](https://peps.python.org/pep-0649/#changes-to-allowable-annotations-syntax)
- [x] Duplicate key in mapping `match` pattern (`case {"x": 1, "x": 2}:`) (https://github.com/astral-sh/ruff/pull/17129)
- [x] Name assigned before `global` declaration (PLE0118) (https://github.com/astral-sh/ruff/pull/17135)
- [x] https://github.com/astral-sh/ruff/issues/16520 (https://github.com/astral-sh/ruff/pull/17134)
- [x] https://github.com/astral-sh/ruff/issues/13759 ([cpython#100581](https://github.com/python/cpython/pull/100581))
- [x] Multiple assignments to name 'a' in pattern (`case [a, b, a]:`)
- [x] [Irrefutable patterns](https://github.com/astral-sh/ruff/blob/ff3bf583b28c455a19571a54cd4a4e2bc024b5b8/crates/ruff_python_ast/src/nodes.rs#L3024-L3035) are syntax errors
	- `SyntaxError: name capture 'x' makes remaining patterns unreachable`
	- `SyntaxError: wildcard makes remaining patterns unreachable`
- [x] https://github.com/astral-sh/ruff/issues/11119
- [x] #14395

A lot of them are mentioned in this [comment](https://github.com/astral-sh/ruff/issues/7633#issuecomment-1740424031). These all would be part of `E999` rule.

## Version-related errors

The following are both version-related and detected by the compiler. Many of them were originally tracked in #6591.

### 3.13
- [x] ~~`global` declarations in `try` nodes~~ ([cpython#111123](https://github.com/python/cpython/issues/111123)) (https://github.com/astral-sh/ruff/pull/17285)
  - Are now allowed in `except` blocks if the variable is only referenced in an `else` block
  - Are now disallowed in `else` blocks if the variable is only referenced in an `except` block
### 3.11
- [x] Async comprehensions are now allowed when nested inside sync comprehensions in async functions ([cpython#77527](https://github.com/python/cpython/issues/77527)) (https://github.com/astral-sh/ruff/pull/17177)
### 3.10
- [x] `del __debug__` is a syntax error on Python 3.10+ ([cpython#89163](https://github.com/python/cpython/issues/89163))

---

_Label `tracking` added by @dhruvmanila on 2024-06-19 03:26_

---

_Assigned to @ntBre by @ntBre on 2025-03-05 21:26_

---

_Closed by @ntBre on 2025-04-23 19:45_

---
