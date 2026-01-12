```yaml
number: 454
title: "`sys.exit` is not being acknowledged as an exit point of the function"
type: issue
state: closed
author: inoa-jboliveira
labels: []
assignees: []
created_at: 2025-05-19T16:45:44Z
updated_at: 2025-05-19T17:07:32Z
url: https://github.com/astral-sh/ty/issues/454
synced_at: 2026-01-12T15:54:23Z
```

# `sys.exit` is not being acknowledged as an exit point of the function

---

_@inoa-jboliveira_

### Summary

I don't know how to describe it exactly, but running ty check on the following code shows an `invalid-return-type` because the code does not acknowledges that sys.exit is equivalent o a raise SystemExit and thus the function will not return anything.

```
def config(thing) -> dict:
    if thing:
        return {'response': thing}
    sys.exit(-1)
```

This is a dumbed down version of a real function on a real codebase, where the code exits if some config is not passed correctly. In all successful function calls, it will return a dict otherwise it will raise SystemExit. There is no possible "None" being returned as suggested:


```
error[invalid-return-type]: Function can implicitly return `None`, which is not assignable to 
return type `dict[Unknown, Unknown]`
 --> foo.py:4:17
  |
4 | def config(thing) -> dict:
  |                      ^^^^
5 |     if thing:
6 |         return {'response': thing}
  |
``` 


### Version

ty 0.0.1-alpha.5 (3b726d87a 2025-05-17)

---

_Renamed from "[ty] sys.exit is not being acknowledged as an exit point of the function" to "`sys.exit` is not being acknowledged as an exit point of the function" by @MichaReiser on 2025-05-19 17:03_

---

_Closed by @AlexWaygood on 2025-05-19 17:07_

---
