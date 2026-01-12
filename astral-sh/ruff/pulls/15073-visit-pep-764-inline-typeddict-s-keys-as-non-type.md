```yaml
number: 15073
title: "Visit PEP 764 inline `TypedDict`s' keys as non-type-expressions"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
assignees: []
merged: true
base: main
head: inline-typeddicts
created_at: 2024-12-20T00:34:24Z
updated_at: 2024-12-30T09:48:29Z
url: https://github.com/astral-sh/ruff/pull/15073
synced_at: 2026-01-12T15:55:50Z
```

# Visit PEP 764 inline `TypedDict`s' keys as non-type-expressions

---

_@InSyncWithFoo_

## Summary

Resolves #10812.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-12-20 00:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @InSyncWithFoo on 2024-12-20 00:41_

The `INLINE_TYPEDDICT` bitflag was added "just in case". I'll remove it if it is not desired.

---

_Comment by @dhruvmanila on 2024-12-24 06:29_

> The `INLINE_TYPEDDICT` bitflag was added "just in case". I'll remove it if it is not desired.

Yeah, we should remove it if it's not being used currently.

I'll also make a general recommendation to use `typed_dict` instead of `typeddict` as I'd consider them two separate words.

Apart from this, I'm not exactly sure what the scope of this should be as the PEP is still in draft so we don't know where all we need to change this. What about when the slice element is not a dictionary like the one in your test case? I don't see it being supported in the PEP yet. Should we only limit it to the following syntax (https://github.com/Viicos/peps/blob/inlined-typed-dictionaries/peps/pep-0764.rst#typing-specification-changes)?

```
<TypedDict> '[' '{' (string: ':' annotation_expression ',')* '}' ']'
```

---

_Label `rule` added by @dhruvmanila on 2024-12-24 06:30_

---

_Comment by @InSyncWithFoo on 2024-12-24 06:57_

@dhruvmanila When the expression is not a dictionary literal, `TypedDict[expr]` is invalid as a type expression. In this case, I think `expr` should be visited as a normal (non-type) expression, which is [already the case](https://github.com/astral-sh/ruff/pull/15073/files#diff-3c2207aa8a10ed7a5c8ed3a0ed0c89a0228b95a6c4d6e12436313f8431287f51R1521) with this PR.

This PR could be considered provisional support for the PEP. Pyright (and Basedpyright) already did so; parity by other tools could help the PEP gain wider recognition.

---

_@dhruvmanila approved on 2024-12-30 09:33_

Thanks!

---

_Merged by @dhruvmanila on 2024-12-30 09:34_

---

_Closed by @dhruvmanila on 2024-12-30 09:34_

---

_Branch deleted on 2024-12-30 09:48_

---
