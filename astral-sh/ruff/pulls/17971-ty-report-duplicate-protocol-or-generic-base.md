```yaml
number: 17971
title: "[ty] Report duplicate `Protocol` or `Generic` base classes with `[duplicate-base]`, not `[inconsistent-mro]`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/dup-protocols
created_at: 2025-05-08T22:11:43Z
updated_at: 2025-05-08T22:41:24Z
url: https://github.com/astral-sh/ruff/pull/17971
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Report duplicate `Protocol` or `Generic` base classes with `[duplicate-base]`, not `[inconsistent-mro]`

---

_Pull request opened by @AlexWaygood on 2025-05-08 22:11_

## Summary

Currently, if you do something like this:

```py
from typing import Protocol

class Foo(Protocol, Protocol): ...
```

then we'll emit an `[inconsistent-mro]` diagnostic complaining about it. That isn't _wrong_, but it's a bit weird, because for any other duplicated base we'd complain about it using the `[duplicate-base]` error code. This PR improves our logic so that we use that error code instead, even for the very special classes `Protocol` and `Generic` that need to be heavily special-cased by ty.

## Test Plan

Existing mdtest updated; new mdtest added


---

_Review requested from @carljm by @AlexWaygood on 2025-05-08 22:11_

---

_Label `ty` added by @AlexWaygood on 2025-05-08 22:11_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-08 22:11_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-08 22:11_

---

_Comment by @github-actions[bot] on 2025-05-08 22:17_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-05-08 22:38_

---

_Merged by @AlexWaygood on 2025-05-08 22:41_

---

_Closed by @AlexWaygood on 2025-05-08 22:41_

---

_Branch deleted on 2025-05-08 22:41_

---
