```yaml
number: 1436
title: Returning self as dict value does validate against return type
type: issue
state: closed
author: gjcarneiro
labels:
  - bidirectional inference
assignees: []
created_at: 2025-10-24T15:01:00Z
updated_at: 2025-10-24T20:15:27Z
url: https://github.com/astral-sh/ty/issues/1436
synced_at: 2026-01-10T02:06:25Z
```

# Returning self as dict value does validate against return type

---

_Issue opened by @gjcarneiro on 2025-10-24 15:01_

### Summary

https://play.ty.dev/fcecf4ca-df7b-4dd5-8922-97219ceb68cf

```py

from __future__ import annotations

class Account:

    def get_account(self) -> dict[str, Account]:
        return dict(loan=self)
```


```
$ mypy /tmp/xxx.py 
Success: no issues found in 1 source file
$ ty check /tmp/xxx.py 
Checking ------------------------------------------------------------ 1/1 files                                                                                                                                                                                          
/tmp/xxx.py:6:16: error[invalid-return-type] 
Return type does not match returned value: expected `dict[str, Account]`, found `dict[str, Self@get_account]`
Found 1 diagnostic
```


### Version

_No response_

---

_Label `bidirectional inference` added by @AlexWaygood on 2025-10-24 15:13_

---

_Comment by @ibraheemdev on 2025-10-24 20:15_

I believe we need to special case the `dict` constructor for this to work.

---

_Closed by @ibraheemdev on 2025-10-24 20:15_

---
