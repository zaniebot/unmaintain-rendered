```yaml
number: 18952
title: formatting empty lines in multi-line arrays
type: issue
state: closed
author: vghej
labels:
  - formatter
assignees: []
created_at: 2025-06-26T07:34:00Z
updated_at: 2025-06-26T08:38:20Z
url: https://github.com/astral-sh/ruff/issues/18952
synced_at: 2026-01-12T15:54:56Z
```

# formatting empty lines in multi-line arrays

---

_@vghej_

### Summary

Consider a Python snippet like this:

```python
KNOWN_STUFF = [
    # vegetables
    "carrot",
    "salad",

    # fruits
    "apple",
    "banana",

    # mobility
    "car",
    "train",
]
```

Right now, `ruff format` will delete the empty lines which I added for visual separation / explanation. My current workaround is appending `# fmt:skip` to the array but this, of course, disables all formatting within.

Could you please fix/extend the formatter logic to recognize the grouping comments in this situation?

### Version

ruff 0.11.4

---

_Label `formatter` added by @AlexWaygood on 2025-06-26 08:03_

---

_Closed by @MichaReiser on 2025-06-26 08:09_

---

_Comment by @vghej on 2025-06-26 08:38_

Damn, thanks for the hint - wondering how I did not find that issue :(

---
