```yaml
number: 1051
title: Less error-prone way to examine both declarations and bindings
type: issue
state: open
author: carljm
labels:
  - internal
assignees: []
created_at: 2025-08-19T15:13:18Z
updated_at: 2025-08-19T15:13:34Z
url: https://github.com/astral-sh/ty/issues/1051
synced_at: 2026-01-10T02:06:24Z
```

# Less error-prone way to examine both declarations and bindings

---

_Issue opened by @carljm on 2025-08-19 15:13_

We do this sort of thing a lot in ty's code base: iterating over both bindings and declarations separately, hoping that there is a 1:1 relationship. This is not always the case. For example:
```py
class D: # maybe a protocol, maybe a dataclass with fields, …
    if condition:
        attr: int
    else:
        attr = ""
```

This is unlikely to be a problem for protocols in particular, I think. And it's certainly not something specific to this PR. But I think it would be very beneficial in a lot of places if we could somehow iterate over all *definitions* and then get the declared type (for `attr: int`), the inferred type (for `attr = ""`), or both (for `attr: int = 0`).

Here, it would eliminate the mutation of the entry. And the bug would probably not have happened in the first place if the iterator would yield either a `DeclaredType { ty }`, an `InferredType { ty }`, or `Both { declared_ty, inferred_ty }`. See https://github.com/astral-sh/ruff/pull/19756 for a somewhat related problem that I fixed a few weeks ago.

_Originally posted by @sharkdp in https://github.com/astral-sh/ruff/pull/19950#discussion_r2284603002_
            

---

_Label `internal` added by @carljm on 2025-08-19 15:13_

---
