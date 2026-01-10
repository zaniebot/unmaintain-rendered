---
number: 2373
title: "Unannotated properties *but* fully infered in __init__ are Unknown"
type: issue
state: closed
author: Salamandar
labels: []
assignees: []
created_at: 2026-01-06T22:28:26Z
updated_at: 2026-01-07T15:19:17Z
url: https://github.com/astral-sh/ty/issues/2373
synced_at: 2026-01-10T01:51:14Z
---

# Unannotated properties *but* fully infered in __init__ are Unknown

---

_Issue opened by @Salamandar on 2026-01-06 22:28_

### Summary

Maybe it's actually fully voluntary? If so, please ignore and close.

Minimal example in the playground: https://play.ty.dev/523187df-e4a6-4ba3-960c-0ac72457f222

The property is fully inferred as str in the class constructor, but in other methods, it's `Unknown | str`.

`ty` will *not* show errors when checking, but text editors will *not* provide autocompletion in methods based on `str`.


EDIT: OK, i just found https://github.com/astral-sh/ty/issues/1240 thanks to the doc. Closing.

---

_Closed by @Salamandar on 2026-01-06 22:30_

---

_Comment by @AlexWaygood on 2026-01-07 15:19_

> text editors will _not_ provide autocompletion in methods based on `str`.

this part we just fixed in https://github.com/astral-sh/ruff/pull/22436 FYI -- it should be resolved in the next release!

---
