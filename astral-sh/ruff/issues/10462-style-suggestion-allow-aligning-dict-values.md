```yaml
number: 10462
title: "Style suggestion: Allow aligning dict values"
type: issue
state: open
author: kaddkaka
labels:
  - formatter
  - style
assignees: []
created_at: 2024-03-18T18:27:57Z
updated_at: 2024-03-20T19:32:52Z
url: https://github.com/astral-sh/ruff/issues/10462
synced_at: 2026-01-12T15:54:50Z
```

# Style suggestion: Allow aligning dict values

---

_@kaddkaka_

**Describe the style change**
Allow aligning dict values in dict literals. 

**Example in the current style**
```py
DATA = {
    "aa": 0x000F,
    "bbbb": 0x00F0,
    "cccccccc": 0x0F00,
    "dddddd": 0xF000,
}
```

**Desired style**
Having aligned values can aid readability in some cases.
```python
DATA = {
    "aa":       0x000F,
    "bbbb":     0x00F0,
    "cccccccc": 0x0F00,
    "dddddd":   0xF000,
}
```

If the values are already aligned, that could be an heuristic to leave them aligned, just like trailing comma is a heuristic to control 1 element per line.

The same suggestion got a cold welcome on black: https://github.com/psf/black/issues/4281


---

_Label `formatter` added by @AlexWaygood on 2024-03-18 19:40_

---

_Label `style` added by @AlexWaygood on 2024-03-18 19:40_

---

_Comment by @MichaReiser on 2024-03-19 12:59_

Thanks for creating this style proposal. I don't think there's enough consense to change the existing formatting to use aligned formatting instead. So the ask here is to implement a new formatting style and allow users to pick their preferred style. We haven't yet decided if we want to make the formatter more configurable or not. That's why it's unlikely that we would accept any contributions to this style near term (or implement this ourselves). 


Regarding the style. Would you expect that function parameters are aligned too? 

```python
def perform_some_tasks(
    data:    List[float]           = a,
    arg2:    Union[int, str, bool] = b,
    hello:   Optional[int]         = c,
    good:    Dict[str, Any]        = d,
    morning: float                 = e,
) -> None:
    ...
```

What would you propose how dictionaries are formatted with very long keys, that potentially even exceed the configured line length:

```python
a = {
    "%s_VERSION" % entry.upper().replace("-", "_").replace(".", "_"): DOWNLOADS[entry]["version"],
    other                                                             3,
}


b = {
    "%s_VERSION" 
    % entry.upper().replace("-", "_").replace(".", "_").replace(",", "_").replace("/", "_").replace("more"): DOWNLOADS[
    	entry
    ]["version"],
    b                                                                                                      : 10                                                        
}
```


How would your example format with comments ?

```python
DATA = {
    "aa": # trailing key comment
    		0x000F,
    # line comment in betweeen
    "bbbb":     
    # leading value comment
    0x00F0,
    "cccccccc": 0x0F00, # trailing value comment
    "dddddd":   0xF000,
}
```



---

_Comment by @kaddkaka on 2024-03-19 17:15_

I have only felt the need for dicts, maybe this pattern is less common for function arguments.

I want to clarify the last sentence about heuristic with an example:

Heuristic is "If already aligned, keep it aligned". So
```py
DATA = {
    "aa":       0x000F,
    "bbbb":     0x00F0,
    "cccccccc": 0x0F00,
    "dddddd":   0xF000,
}
```
would stay unchanged. But

```py
DATA = {
    "aa":   0x000F,
    "bbbb":     0x00F0,
    "cccccccc": 0x0F00,
    "dddddd":     0xF000,
}
```

would be formatted (as today) into:
```py
DATA = {
    "aa": 0x000F,
    "bbbb": 0x00F0,
    "cccccccc": 0x0F00,
    "dddddd": 0xF000,
}
```

----

Your example does not have aligned values:
```py
DATA = {
    "aa": # trailing key comment
                0x000F,
    # line comment in betweeen
    "bbbb":
    # leading value comment
    0x00F0,
    "cccccccc": 0x0F00, # trailing value comment
    "dddddd":   0xF000,
}
```
so it would format (as today) into:
```py
DATA = {
    "aa":  # trailing key comment
    0x000F,
    # line comment in betweeen
    "bbbb":
    # leading value comment
    0x00F0,
    "cccccccc": 0x0F00,  # trailing value comment
    "dddddd": 0xF000,
}
```

---

_Comment by @T-256 on 2024-03-19 19:56_

> Heuristic is "If already aligned, keep it aligned

👍

---
