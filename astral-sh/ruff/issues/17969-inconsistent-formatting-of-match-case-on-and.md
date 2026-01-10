```yaml
number: 17969
title: "Inconsistent formatting of match-case on `[]` and `_`"
type: issue
state: closed
author: adamchainz
labels:
  - formatter
  - style
assignees: []
created_at: 2025-05-08T21:37:50Z
updated_at: 2025-05-22T05:52:22Z
url: https://github.com/astral-sh/ruff/issues/17969
synced_at: 2026-01-10T11:09:58Z
```

# Inconsistent formatting of match-case on `[]` and `_`

---

_Issue opened by @adamchainz on 2025-05-08 21:37_

### Summary

[Playground link](https://play.ruff.rs/5a7bdd18-182f-4d70-b855-2fdd149ddae7)

Source:

```py
a = []
b = []
match a, b:
    case [], []:
        ...
    case [], _:
        ...
    case _, []:
        ...
    case _, _:
        ...
```

Formatted:

```py
a = []
b = []
match a, b:
    case [[], []]:
        ...
    case [[], _]:
        ...
    case _, []:
        ...
    case _, _:
        ...
```

It seems rather inconsistent that the formatter adds wrapping `[]` on the first two cases but not the others.

Edit: for reference, Black leaves the original file unchanged.

### Version

ruff 0.11.7 (f7b48510b 2025-04-24)

---

_Label `formatter` added by @AlexWaygood on 2025-05-08 21:47_

---

_Label `style` added by @MichaReiser on 2025-05-09 06:09_

---

_Closed by @MichaReiser on 2025-05-22 05:52_

---
