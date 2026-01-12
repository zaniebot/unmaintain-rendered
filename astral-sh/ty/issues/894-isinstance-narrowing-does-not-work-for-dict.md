```yaml
number: 894
title: "isinstance narrowing does not work for `dict`"
type: issue
state: closed
author: scosman
labels: []
assignees: []
created_at: 2025-07-25T17:29:21Z
updated_at: 2025-07-25T17:31:51Z
url: https://github.com/astral-sh/ty/issues/894
synced_at: 2026-01-12T15:54:24Z
```

# isinstance narrowing does not work for `dict`

---

_@scosman_

### Summary

isinstance based narrowing only seems to work in 1 direction:

Fails: 
```
def do_thing(input: Dict | str) -> str:
    if isinstance(input, dict):
        return json.dumps(input, ensure_ascii=False)

    # Return type does not match returned value: expected `str`, found `(dict[Unknown, Unknown] & ~dict[Unknown, Unknown]) | str`
    return input
```

Works:
```
def do_thing(input: Dict | str) -> str:
    if isinstance(input, str):
        return input

    return json.dumps(input, ensure_ascii=False)
```

similarly:

Gets complex type
```
input_str = (
            json.dumps(input, ensure_ascii=False) if isinstance(input, dict) else input
)
```

Gets str type:
```
input_str: str = (
            input if isinstance(input, str) else json.dumps(input, ensure_ascii=False)
)
```

### Version

a16

---

_Renamed from "isinstance narrowing only works in one direction" to "isinstance narrowing does not work for `dict`" by @scosman on 2025-07-25 17:29_

---

_Comment by @AlexWaygood on 2025-07-25 17:31_

Thanks for the report! We're already tracking this in https://github.com/astral-sh/ty/issues/456

---

_Closed by @AlexWaygood on 2025-07-25 17:31_

---
