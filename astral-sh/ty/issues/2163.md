```yaml
number: 2163
title: "TypedDict: inconsistent inferred types for `values()`, and `items()`"
type: issue
state: closed
author: mflova
labels: []
assignees: []
created_at: 2025-12-22T15:56:45Z
updated_at: 2025-12-22T16:38:32Z
url: https://github.com/astral-sh/ty/issues/2163
synced_at: 2026-01-10T01:56:41Z
```

# TypedDict: inconsistent inferred types for `values()`, and `items()`

---

_Issue opened by @mflova on 2025-12-22 15:56_

### Summary

According to the Python documentation (see https://typing.python.org/en/latest/spec/typeddict.html)

> "The return types of the `items()` and `values()` methods can be determined from the union of all item types in the TypedDict (which would include `object` for open TypedDicts). Therefore, type checkers should infer more precise types for TypedDicts that are not open."

However, the inferred types for `values()` and consequently `items()` do not match this behavior in practice. Even for closed `TypedDict`s, the inferred types are broader than expected and do not reflect the union of the declared field types.

I hope it is not suplicated! I did not see any notes regarding this matter in this thread about `TypedDict`: https://github.com/astral-sh/ty/issues/154

https://play.ty.dev/858493ec-361b-450b-a7bc-3243984e13fb

### Version

0.0.5

---

_Comment by @mflova on 2025-12-22 15:58_

Wrong link to the playground one! This is it:

https://play.ty.dev/c8f3f194-0733-4ccd-a939-cde611df8410

---

_Comment by @carljm on 2025-12-22 16:34_

Thanks for the report!

I think for an open `TypedDict` without `extra_items` (which is the default), our current inference for `.values()` and `.items()` is correct. (This is mentioned in the quoted section of the spec: "which would include `object` for open TypedDicts").

Support for `extra_items` and `closed=True` is tracked already in #154 as a bullet point, so I'll close this as duplicate.

---

_Closed by @AlexWaygood on 2025-12-22 16:38_

---
