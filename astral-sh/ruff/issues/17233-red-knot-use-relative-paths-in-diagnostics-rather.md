```yaml
number: 17233
title: "[red-knot] Use relative paths in diagnostics rather than absolute paths"
type: issue
state: closed
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
created_at: 2025-04-06T15:20:59Z
updated_at: 2025-04-28T18:32:35Z
url: https://github.com/astral-sh/ruff/issues/17233
synced_at: 2026-01-12T15:54:55Z
```

# [red-knot] Use relative paths in diagnostics rather than absolute paths

---

_@AlexWaygood_

Diagnostics are currently reported with absolute paths, which makes them quite verbose even if you specify `--output-format=concise`. E.g. here's a diagnostic I got running red-knot from my `/Users/alexw/dev/typeshed-stats` directory:

```
error[lint:invalid-parameter-default] /Users/alexw/dev/typeshed-stats/scripts/runtests.py:17:5: Default value of type `EllipsisType` is not assignable to annotated parameter type `Literal[False]`
```

It would be nicer if we used the relative path instead, which is what Ruff does:

```
error[lint:invalid-parameter-default] scripts/runtests.py:17:5: Default value of type `EllipsisType` is not assignable to annotated parameter type `Literal[False]`
```

---

_Label `diagnostics` added by @AlexWaygood on 2025-04-06 15:20_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-06 15:21_

---

_Added to milestone `Red Knot Alpha` by @AlexWaygood on 2025-04-06 15:21_

---

_Comment by @MichaReiser on 2025-04-06 16:56_

Yes! @BurntSushi this one would be great to get into the alpha. 

One question is whether the paths should be relative to the project root or the current directory. I think it should be the current directory, so that the paths are "clickable"

---

_Assigned to @BurntSushi by @BurntSushi on 2025-04-07 12:14_

---

_Closed by @BurntSushi on 2025-04-28 18:32_

---

_Closed by @BurntSushi on 2025-04-28 18:32_

---
