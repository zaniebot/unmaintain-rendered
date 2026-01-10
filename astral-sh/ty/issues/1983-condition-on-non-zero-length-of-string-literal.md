```yaml
number: 1983
title: "Condition on non-zero length of string literal doesn't short circuit `and` statements"
type: issue
state: closed
author: kokes
labels:
  - narrowing
assignees: []
created_at: 2025-12-17T08:20:35Z
updated_at: 2025-12-17T18:15:59Z
url: https://github.com/astral-sh/ty/issues/1983
synced_at: 2026-01-10T01:53:59Z
```

# Condition on non-zero length of string literal doesn't short circuit `and` statements

---

_Issue opened by @kokes on 2025-12-17 08:20_

### Summary

Code that checks `if len(value) and value[0] == ' '` should work just fine, but ty reports it as `index-out-of-bounds` in some cases (when the value is `foo | Literal[""]`).

Minimal code

```py
lines: list[str] = ["foo", "bar", "baz"]
for line in lines:
    if not line:
        continue

    value = line if len(line) < 3 else ""
    if len(value) and value[0] == " ":
        value = value[1:]
```

uvx ty check:
```
error[index-out-of-bounds]: Index 0 is out of bounds for string `Literal[""]` with length 0
 --> foo.py:7:23
  |
6 |     value = line if len(line) < 3 else ""
7 |     if len(value) and value[0] == " ":
  |                       ^^^^^
8 |         value = value[1:]
  |
info: rule `index-out-of-bounds` is enabled by default

Found 1 diagnostic
```

Changing the `if` condition to `if value and value[0]` makes it go away as well. Removing `if not line` (and turning `line` to a plain `str` also makes this go away. Seems like `ty` doesn't see through `Literal` and doesn't infer that `len(Literal[""])` is `AlwaysFalsy` (or something along those lines).

### Version

ty 0.0.2 (42835578d 2025-12-16)

---

_Label `narrowing` added by @AlexWaygood on 2025-12-17 09:26_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-17 14:04_

---

_Closed by @charliermarsh on 2025-12-17 18:16_

---
