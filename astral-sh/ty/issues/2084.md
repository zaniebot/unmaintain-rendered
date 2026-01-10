```yaml
number: 2084
title: installable runtime shim for ty_extensions module
type: issue
state: open
author: carljm
labels: []
assignees: []
created_at: 2025-12-18T19:21:29Z
updated_at: 2025-12-18T23:09:20Z
url: https://github.com/astral-sh/ty/issues/2084
synced_at: 2026-01-10T01:54:00Z
```

# installable runtime shim for ty_extensions module

---

_Issue opened by @carljm on 2025-12-18 19:21_

For users who are OK restricting themselves to ty and want to use intersections, there's no reason for us not to make it easier for them to do so.

We should maybe consider splitting `ty_extensions` into two modules, one for things we want to support as public API, and another for things that are really just for internal mdtest use.

---

_Added to milestone `Stable` by @carljm on 2025-12-18 19:21_

---

_Comment by @AlexWaygood on 2025-12-18 19:25_

For some symbols like `Intersection` and `Not`, we could also see if some of these symbols could be added to typing_extensions directly. Given that some of them have been floated in the typing community for many years, and typing_extensions often adds APIs for draft PEPs, this wouldn't be totally without precedent.

---

_Comment by @carljm on 2025-12-18 19:26_

That's a great alternative if we can make it happen! I'm optimistic for `Intersection`; less clear for `Not`.

---

_Comment by @AlexWaygood on 2025-12-18 23:09_

This issue feels like it overlaps with https://github.com/astral-sh/ty/issues/223

---
