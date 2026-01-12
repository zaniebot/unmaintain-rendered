```yaml
number: 718
title: "Special-casing `__import__()` for simple cases"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - imports
assignees: []
created_at: 2025-06-27T18:49:01Z
updated_at: 2025-06-29T10:49:24Z
url: https://github.com/astral-sh/ty/issues/718
synced_at: 2026-01-12T15:54:23Z
```

# Special-casing `__import__()` for simple cases

---

_@InSyncWithFoo_

[More often than not](https://github.com/search?q=lang%3APython%20%2F__import__%5C(%5B%27%22%5D%5Cw%2B%5B%27%22%5D%5C)%2F&type=code), [`__import__()`](https://docs.python.org/3/library/functions.html#import__) is called with a single literal string value. For example:

```python
print(__import__('sys').version)
```

Currently, the revealed type of `__import__('sys')` is `ModuleType`, which is not wrong but less helpful than it could be. I think it is possible and probably desired to have ty infer `<module 'sys'>` here instead.

This feature is not without precedence; TypeScript understands [dynamic imports](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import) as it would normal imports ([playground](https://www.typescriptlang.org/play/?#code/MYewdgzgLgBAZhGBeGBDA7qglrLBbABxACcoAKAcgQoEoBuAKAHomY2YA9AfgYYFMAHkVIwA3gF9GQA)):

```typescript
const fs = await import('fs');
//    ^? { default: typeof import("fs"), rename: typeof rename; ... }
```

(It should be noted that `import` in TS/JS is a keyword and thus cannot be overridden like `__import__` can.)

---

_Renamed from "Special-casing `__import__` for simple cases" to "Special-casing `__import__()` for simple cases" by @InSyncWithFoo on 2025-06-27 18:49_

---

_Label `imports` added by @carljm on 2025-06-27 19:25_

---

_Comment by @carljm on 2025-06-27 19:29_

The GH code search does suggest this could be worth doing (it should be a fairly easy change, I think?). Though I think it's low priority, since `ModuleType` already has a `__getattr__` returning `Any`, which should silence false positives arising from this.

FWIW, pyright appears to special-case `__import__` to return `Any` (I wonder why, since the `__getattr__` in typeshed seems like it should be sufficient?), while mypy just follows typeshed and returns `ModuleType`.

---

_Comment by @erictraut on 2025-06-27 19:55_

> I wonder why, since the `__getattr__` in typeshed seems like it should be sufficient?

I'd have to do some historical spelunking to answer that definitively, but I think it's because `ModuleType` didn't previously have a `__getattr__` method. Looks like Alex added it to `types.pyi` a few years back. In any case, I agree with your observation that the special-casing in pyright could be removed now.

---

_Comment by @AlexWaygood on 2025-06-27 20:36_

Yeah, I added it to the `ModuleType` stub specifically to make use cases with dynamic imports like this less annoying for users to deal with. FWIW ty currently ignores `ModuleType.__getattr__` too much -- I think we actually ignore it in all cases, whereas we should really only ignore it when we're falling back to `ModuleType` from an attribute lookup on a module-literal type: https://play.ty.dev/c306c468-2839-461f-86a6-bb8a750b62a7

---

_Closed by @AlexWaygood on 2025-06-29 10:49_

---
