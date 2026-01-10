```yaml
number: 1603
title: "Empty set from `set()` assignable to `MySet` but not to `MySet | None`"
type: issue
state: closed
author: BHSPitMonkey
labels:
  - bug
  - bidirectional inference
assignees: []
created_at: 2025-11-20T23:25:24Z
updated_at: 2025-11-21T22:05:34Z
url: https://github.com/astral-sh/ty/issues/1603
synced_at: 2026-01-10T01:58:59Z
```

# Empty set from `set()` assignable to `MySet` but not to `MySet | None`

---

_Issue opened by @BHSPitMonkey on 2025-11-20 23:25_

### Summary

Playground: https://play.ty.dev/b6d2c6ad-1986-425e-9619-12434e33a223

```python
type StyleSet = set[
    Literal[
        "bold",
        "italic",
        "strike",
    ]
]

s: StyleSet = set()
# Works as expected

s: StyleSet | None = set()
# Object of type `set[Unknown]` is not assignable to `StyleSet | None` (invalid-assignment)
```

The custom subscripted `set` subtype above has no trouble accepting an empty `set()`, but `ty` produces an `invalid-assignment` when assigned to `StyleSet | None` (despite this union being wider)

### Version

eb7c098d6

---

_Label `bidirectional inference` added by @carljm on 2025-11-20 23:50_

---

_Added to milestone `Beta` by @carljm on 2025-11-20 23:51_

---

_Comment by @carljm on 2025-11-20 23:52_

@ibraheemdev Haven't looked into this in depth, but it certainly looks like a bidi issue -- even if we infer `set[Unknown]` for the RHS (and it seems like we should be able to do better than that with the annotation?) it should definitely be assignable to `StyleSet | None`.

---

_Label `bug` added by @carljm on 2025-11-20 23:52_

---

_Comment by @ibraheemdev on 2025-11-21 21:43_

This should be resolved by https://github.com/astral-sh/ruff/pull/21574. We are correctly using the type context, but promoting to `set[str]` because we don't see through the union. The confusing diagnostic is because of reinference — we should probably to prioritize https://github.com/astral-sh/ruff/pull/21267 to avoid cases like this.

---

_Closed by @ibraheemdev on 2025-11-21 22:05_

---
