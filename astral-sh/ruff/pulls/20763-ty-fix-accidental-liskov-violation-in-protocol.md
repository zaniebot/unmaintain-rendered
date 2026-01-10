```yaml
number: 20763
title: "[ty] Fix accidental Liskov violation in protocol tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/fix-another-protocol-test
created_at: 2025-10-08T11:56:21Z
updated_at: 2025-10-08T12:04:39Z
url: https://github.com/astral-sh/ruff/pull/20763
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Fix accidental Liskov violation in protocol tests

---

_Pull request opened by @sharkdp on 2025-10-08 11:56_

## Summary

We have the following test in `protocols.md`:
```py
class HasX(Protocol):
    x: int

# […]

class Foo:
    x: int

# […]

class FooBool(Foo):
    x: bool

static_assert(not is_subtype_of(FooBool, HasX))
static_assert(not is_assignable_to(FooBool, HasX))
```

If `Foo` was indeed intended to be a base class of `FooBool`, then `x: bool` should be reported as a Liskov violation. And then it's a matter of definition whether or not these assertions should hold true or not (should the incorrect override take precedence or not?). So it looks to me like this is just an oversight, probably a copy-paste error from another test right before it, where `FooSub` is indeed intended to be a subclass of `Foo`.

I am fixing this because this test started to fail on a branch of mine that changes how attribute lookup in inheritance chains works.

---

_Review requested from @carljm by @sharkdp on 2025-10-08 11:56_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-08 11:56_

---

_Label `testing` added by @sharkdp on 2025-10-08 11:56_

---

_Review requested from @dcreager by @sharkdp on 2025-10-08 11:56_

---

_Label `ty` added by @sharkdp on 2025-10-08 11:56_

---

_@AlexWaygood approved on 2025-10-08 11:58_

---

_Merged by @sharkdp on 2025-10-08 12:04_

---

_Closed by @sharkdp on 2025-10-08 12:04_

---

_Branch deleted on 2025-10-08 12:04_

---
