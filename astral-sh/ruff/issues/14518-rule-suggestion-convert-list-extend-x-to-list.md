```yaml
number: 14518
title: "Rule suggestion: Convert list.extend([x]) to list.append(x)"
type: issue
state: open
author: NeilGirdhar
labels:
  - rule
  - type-inference
  - needs-decision
assignees: []
created_at: 2024-11-22T02:12:43Z
updated_at: 2024-12-09T08:53:07Z
url: https://github.com/astral-sh/ruff/issues/14518
synced_at: 2026-01-12T15:54:53Z
```

# Rule suggestion: Convert list.extend([x]) to list.append(x)

---

_@NeilGirdhar_

This may depend on RedKnot, but with type information, this could hit a lot of code.  If we know `retval` is a list, consider converting:
```
retval.extend([x]) 
```
to
```
retval.append(x)
```
And similarly, merge
```
retval.extend([x, y])
retval.append(v)
retval.extend([z, w])
```
etc.

It may not seem like merging will catch a lot of cases, but having the Ruff action available makes it available in my editor, which is a great convenience.

---

_Label `rule` added by @MichaReiser on 2024-11-22 10:51_

---

_Label `needs-decision` added by @MichaReiser on 2024-11-22 10:51_

---

_Label `needs-decision` removed by @MichaReiser on 2024-11-22 10:51_

---

_Label `type-inference` added by @MichaReiser on 2024-11-22 10:51_

---

_Label `needs-decision` added by @MichaReiser on 2024-12-09 08:53_

---
