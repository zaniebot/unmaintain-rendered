```yaml
number: 2035
title: "error[invalid-type-form]: Invalid subscript of object of type `def list(self) -> Unknown` in type expression"
type: issue
state: closed
author: kracekumar
labels: []
assignees: []
created_at: 2025-12-17T20:48:59Z
updated_at: 2025-12-18T00:47:00Z
url: https://github.com/astral-sh/ty/issues/2035
synced_at: 2026-01-10T01:53:59Z
```

# error[invalid-type-form]: Invalid subscript of object of type `def list(self) -> Unknown` in type expression

---

_Issue opened by @kracekumar on 2025-12-17 20:48_

### Summary

Using `list[str]` as return value 
```
error[invalid-type-form]: Invalid subscript of object of type `def list(self) -> Unknown` in type expression
```

Snippet

```python

    def list(self) -> list[str]:
        section = cast(dict[str, str], self.config.get(self.section_name, {}))
        return list(section.keys())
```

A complete reproducible code in mypy and ty. 

Mypy: https://mypy-play.net/?mypy=latest&python=3.9&gist=cc18442dcaaab05b33f84ae1f1becba6
ty: https://play.ty.dev/b7794d31-a9ba-468d-b641-8f26d68c1f5f

### Version

ty 0.0.2 (42835578d 2025-12-16), Python: 3.9

---

_Comment by @carljm on 2025-12-17 21:02_

Thanks for the report! This is an intentional choice in ty to choose more consistent and forward-compatible (with runtime behavior) semantics for name resolution when `from __future__ import annotations` is active. For a more in-depth discussion, see https://github.com/astral-sh/ty/issues/1747#issuecomment-3608682796 and following thread.

Since this behavior is not standardized, I recommend to disambiguate your code by using e.g. `builtins.list` (or some other alias) for the annotation.

I'll close this issue as duplicate of #1747.

---

_Closed by @carljm on 2025-12-17 21:02_

---

_Comment by @kracekumar on 2025-12-18 00:47_

Ack. Thank you for suggesting the work around as well and linking the useful reference. 

---
