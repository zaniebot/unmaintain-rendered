```yaml
number: 2187
title: ty does not detect mutation of Mapping (read-only) types
type: issue
state: closed
author: rick-jennings
labels:
  - question
assignees: []
created_at: 2025-12-23T16:02:06Z
updated_at: 2025-12-23T19:28:45Z
url: https://github.com/astral-sh/ty/issues/2187
synced_at: 2026-01-12T15:54:26Z
```

# ty does not detect mutation of Mapping (read-only) types

---

_@rick-jennings_

### Question

Hi,

First, I want to say thanks to the developers of ty for making an awesome type checker!

## Background

Phable is a modern Python toolkit for working with [Project Haystack](https://project-haystack.org/) tagged data, [Xeto](https://github.com/Project-Haystack/xeto?tab=readme-ov-file#overview) specs, and [Haxall](https://haxall.io/). 

## Possible issue

Below are from the phable docs:

Project Haystack data types are immutable, but Python's lists and dicts are mutable. Type checkers can use `typing.Sequence` and `typing.Mapping` to detect mutations while giving programmers flexibility to use either mutable (list/dict) or immutable (tuple/frozendict) types at runtime. Native frozendict support may be added in Python 3.15.

It would be great to use ty with Phable, however, ty does not raise an error when expected.  Below is an example.  Any feedback would be appreciated.  Thanks in advance.

## Example

```python
from typing import Any, Mapping

x: Mapping[str, Any] = {"a": 1}
x["b"] = 2  # Should error: Mapping doesn't support item assignment
```

### Version

_No response_

---

_Label `question` added by @rick-jennings on 2025-12-23 16:02_

---

_Comment by @carljm on 2025-12-23 19:28_

Thanks for the report! The issue here is that we currently will infer a more precise type than the annotated type, so we know that `x` is actually a dict even though you've annotated it as a `Mapping`. If you use a `Mapping` annotation in a place where we #136 t have any more precise information, you will get an error on mutating it. See https://play.ty.dev/b4fac217-c389-4d80-ad41-aed130a34eff

That said, we plan to change this behavior, see #136

---

_Closed by @carljm on 2025-12-23 19:28_

---
