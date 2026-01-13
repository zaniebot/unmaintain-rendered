```yaml
number: 2476
title: Support docstrings on TypedDict fields
type: issue
state: closed
author: max-kamps
labels:
  - server
assignees: []
created_at: 2026-01-13T08:02:58Z
updated_at: 2026-01-13T08:16:48Z
url: https://github.com/astral-sh/ty/issues/2476
synced_at: 2026-01-13T08:22:54Z
```

# Support docstrings on TypedDict fields

---

_@max-kamps_

### Summary

It's possible to add docstrings to class fields like this:

```py
class Person:
    """A person in the database"""

    name: str
    """The person's full legal name"""
    
    birth_year: int | tuple[int, int]
    """The person's year of birth, or a range if not known"""
```

Pyright also supports the same syntax for `TypedDict`s

Here is an example of a TypedDict with field docstrings:
```py
class HttpScope(TypedDict):
    """ASGI scope for HTTP requests"""
    
    type: Literal["http"]
    """Scope type, always "http" for HTTP scopes"""
    
    version: str
    """ASGI spec version the server supports"""
    
    spec_version: NotRequired[str]
    """ASGI HTTP spec version the server supports. Default to "2.0" if not provided"""
```

Here, the docstrings should be shown if hovering over indexing operations involving a string literal. For example, in `scope["spec_version"]` the docstring should be shown when hovering over any part of `["spec_version"]`.

This also relates to #2189 


### Version

0.0.11

---

_Renamed from "Support docstrings on class / TypedDict fields" to "Support docstrings on TypedDict fields" by @max-kamps on 2026-01-13 08:05_

---

_Comment by @AlexWaygood on 2026-01-13 08:07_

Hey, thanks for the report!

We support attribute docstrings, but I thought the convention was for these to come below the attribute rather than above the attribute — we currently only recognise them if they come below. I'm surprised that pyright also supports them if they come above the attribute declaration!

---

_Label `server` added by @AlexWaygood on 2026-01-13 08:07_

---

_Comment by @max-kamps on 2026-01-13 08:10_

Sorry Alex, you're totally right. I really messed up this issue 😅 Looks like this is 100% user error! Please disregard!

---

_Closed by @max-kamps on 2026-01-13 08:10_

---

_Comment by @MichaReiser on 2026-01-13 08:11_

I don't think Pyright supports docstrings before the attribute. See how it always shows the docstring from after the attribute

<img width="3472" height="1867" alt="Image" src="https://github.com/user-attachments/assets/6c33765c-e57e-4ce2-b439-71521f5de9e4" />

---

_Closed by @AlexWaygood on 2026-01-13 08:13_

---

_Comment by @max-kamps on 2026-01-13 08:13_

Just to make sure, does `ty` support docstrings on TypedDict fields? If not, it might be worth it to have an issue for that, specifically. (maybe a new clean one, or reopen this one)


---

_Comment by @AlexWaygood on 2026-01-13 08:15_

> Just to make sure, does `ty` support docstrings on TypedDict fields? If not, it might be worth it to have an issue for that, specifically. (maybe a new clean one, or reopen this one)

We support docstrings on class fields in general. It's possible our special casing for TypedDicts gets in the way of that; not sure. I'd have to check whether that's something that's specifically supported 

---

_Comment by @AlexWaygood on 2026-01-13 08:16_

A clean issue that specifically states the missing functionality (if there is some missing functionality) would be great @max-kamps, thanks!

---
