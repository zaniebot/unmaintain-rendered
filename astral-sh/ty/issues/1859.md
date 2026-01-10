```yaml
number: 1859
title: "Reconsider fallback type (and singleton-ness) for Type::GenericAlias"
type: issue
state: open
author: carljm
labels:
  - bug
  - type properties
assignees: []
created_at: 2025-12-11T20:07:13Z
updated_at: 2025-12-11T20:07:13Z
url: https://github.com/astral-sh/ty/issues/1859
synced_at: 2026-01-10T01:56:41Z
```

# Reconsider fallback type (and singleton-ness) for Type::GenericAlias

---

_Issue opened by @carljm on 2025-12-11 20:07_

@AlexWaygood observes:

> I think we fallback to `types.GenericAlias` in a bunch of places for operations on `Type::GenericAlias` types (the meta-type, I think? Maybe attribute access?). I don't think that's correct anymore now that we consider some class-literal types subtypes of some generic-alias types. We should probably fallback to a `type|types.GenericAlias` union instead?

> Also, we incorrectly consider generic-alias types to be singleton types. I think that was always wrong, but it's even more wrong now with the recent subtyping changes

---

_Label `bug` added by @carljm on 2025-12-11 20:07_

---

_Label `type properties` added by @carljm on 2025-12-11 20:07_

---
