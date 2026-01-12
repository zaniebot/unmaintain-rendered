```yaml
number: 1214
title: support transitive imports
type: issue
state: closed
author: KotlinIsland
labels:
  - imports
assignees: []
created_at: 2025-09-20T06:55:01Z
updated_at: 2025-09-22T12:41:26Z
url: https://github.com/astral-sh/ty/issues/1214
synced_at: 2026-01-12T15:54:24Z
```

# support transitive imports

---

_@KotlinIsland_

### Summary

here the module `package.b` is loaded via an import in another module, mypy supports this feature, pyright does not, what do you think?

```py
my-app/
├─ package/
│  ├─ b.py
│  ├─ __init__.py
├─ main.py
├─ mod.py
```

```py
# main.py
import package
import mod

reveal_type(package.b)
 # ty: Type `<module 'package'>` has no attribute `b`
 # mypy: "types.ModuleType"
 # runtime: `package.b`
```

```py
# mod.py
import package.b
```

### Version

_No response_

---

_Label `imports` added by @AlexWaygood on 2025-09-20 11:59_

---

_Comment by @AlexWaygood on 2025-09-22 12:41_

I don't think we intend to support this feature. Brian gave a nice description of how mypy is able to support this [here](https://github.com/python/typeshed/issues/14246#issuecomment-2959398061) -- but to me it doesn't sound particularly consistent, and I feel like I've seen lots of issues on the mypy bug tracker from users being confused about mypy's behaviour.

Since there's precedent for our current behaviour (pyright), I'd prefer to stick with what we currently have (with some small modifications, such as https://github.com/astral-sh/ty/issues/133)

---

_Closed by @AlexWaygood on 2025-09-22 12:41_

---
