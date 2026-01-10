```yaml
number: 22413
title: "feat(ty): Recognize string literals as subtypes of Sequence[Literal[chars]]"
type: pull_request
state: closed
author: jhartum
labels:
  - ty
assignees: []
base: main
head: feat/string-literal-sequence-subtype
created_at: 2026-01-06T09:21:01Z
updated_at: 2026-01-06T09:33:58Z
url: https://github.com/astral-sh/ruff/pull/22413
synced_at: 2026-01-10T16:30:32Z
```

# feat(ty): Recognize string literals as subtypes of Sequence[Literal[chars]]

---

_Pull request opened by @jhartum on 2026-01-06 09:21_

## Summary

Implements https://github.com/astral-sh/ty/issues/2128: `Literal["abba"]` is now recognized as a subtype of `Sequence[Literal["a", "b"]]`.

## Changes

- **Add `KnownClass::Sequence`** - New known class for `typing.Sequence`, properly integrated as a protocol
- **Extend `has_relation_to_impl`** - When checking if a string literal is a subtype of `Sequence[X]`, verify that each unique character in the string is a subtype of `X`
- **Add mdtests** - Cover positive cases, negative cases, and edge cases (empty strings, unicode, multi-char literals)

## Example

```python
from collections.abc import Sequence
from typing import Literal

def func(tags: Sequence[Literal["a", "b"]]) -> None:
    pass

func("abba")  # Now OK - was incorrectly flagged as error
```

## Implementation Details

The implementation in `has_relation_to_impl`:
1. First tries the standard `str` fallback (for cases like `Sequence[str]`)
2. If that fails and the target is `Sequence[X]`, extracts unique characters from the string literal
3. Verifies each character is a subtype of `X` using `when_all` to properly accumulate constraints
4. Returns the combined constraint set

Edge cases handled:
- Empty strings (always valid)
- Unicode characters
- Multi-char literals in element type (correctly rejected - `Literal["ab"]` ≠ `Literal["a", "b"]`)

## Test Plan

- Added mdtests in `is_subtype_of.md`
- All 307 existing mdtests pass
- Manual testing with various edge cases

Closes https://github.com/astral-sh/ty/issues/2128

---

_Review requested from @carljm by @jhartum on 2026-01-06 09:21_

---

_Review requested from @AlexWaygood by @jhartum on 2026-01-06 09:21_

---

_Review requested from @sharkdp by @jhartum on 2026-01-06 09:21_

---

_Review requested from @dcreager by @jhartum on 2026-01-06 09:21_

---

_Label `ty` added by @MichaReiser on 2026-01-06 09:32_

---

_Comment by @jhartum on 2026-01-06 09:33_

Closing in favor of new PR with rebased branch

---

_Closed by @jhartum on 2026-01-06 09:33_

---
