```yaml
number: 17365
title: "[red-knot] Move relation methods from `CallableType` to `Signature`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dhruv/move-callable-type-relation
created_at: 2025-04-12T04:42:19Z
updated_at: 2025-04-15T13:38:29Z
url: https://github.com/astral-sh/ruff/pull/17365
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] Move relation methods from `CallableType` to `Signature`

---

_@dhruvmanila_

## Summary

This PR moves all the relation methods from `CallableType` to `Signature`.

The main reason for this is that `Signature` is going to be the common denominator between normal and overloaded callables and the core logic to check a certain relationship is going to just require the information that would exists on `Signature`. For example, to check whether an overloaded callable is a subtype of a normal callable, we need to check whether _every_ overloaded signature is a subtype of the normal callable's signature. This "every" logic would become part of the `CallableType` and the core logic of checking the subtyping would exists on `Signature`.

---

_Label `internal` added by @dhruvmanila on 2025-04-12 04:42_

---

_Label `red-knot` added by @dhruvmanila on 2025-04-12 04:42_

---

_Comment by @github-actions[bot] on 2025-04-12 04:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @dhruvmanila on 2025-04-14 21:43_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-14 21:43_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-14 21:43_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-14 21:43_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-14 21:43_

---

_@dcreager approved on 2025-04-14 21:52_

It's much easier to verify that this is a pure move with `git diff` on the command line than in the PR view... :sweat_smile: 

---

_Comment by @dhruvmanila on 2025-04-14 22:02_

> It's much easier to verify that this is a pure move with `git diff` on the command line than in the PR view... 😅

Interesting! Curious to know what your `.gitconfig` has in the `[diff]` section

---

_Merged by @dhruvmanila on 2025-04-14 22:02_

---

_Closed by @dhruvmanila on 2025-04-14 22:02_

---

_Branch deleted on 2025-04-14 22:02_

---

_Comment by @dcreager on 2025-04-15 13:38_

> Interesting! Curious to know what your `.gitconfig` has in the `[diff]` section

```ini
[diff]
    algorithm = histogram
    colorMoved = zebra
    colorMovedWS = ignore-all-space
```

---
