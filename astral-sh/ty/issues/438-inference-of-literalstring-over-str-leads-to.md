```yaml
number: 438
title: Inference of LiteralString over str leads to spurious variance error
type: issue
state: closed
author: brk
labels: []
assignees: []
created_at: 2025-05-18T03:57:21Z
updated_at: 2025-05-18T14:11:50Z
url: https://github.com/astral-sh/ty/issues/438
synced_at: 2026-01-12T15:54:23Z
```

# Inference of LiteralString over str leads to spurious variance error

---

_@brk_

### Summary

https://play.ty.dev/a56e1ef2-6ab6-4b83-a082-456047a5cdba

```python
def foo(xs: list[str]) -> str:
    return "".join(xs)

foo("bar baz".split())
#     ^ inferred type of this expr is list[LiteralString] which is not compatible with list[str]
```

Apologies if this is a duplicate. I searched the issue tracker for "list[str]" and "LiteralString" and didn't see any direct matches.


### Version

ty 0.0.1-alpha.5

---

_Comment by @dcreager on 2025-05-18 14:11_

Yep, this is a duplicate of #336.  I think it was hard to find because the issue is not specific to `list`, it applies to any class with an invariant typevar. We have a fix that should land soon that will hopefully address this.

---

_Closed by @dcreager on 2025-05-18 14:11_

---
