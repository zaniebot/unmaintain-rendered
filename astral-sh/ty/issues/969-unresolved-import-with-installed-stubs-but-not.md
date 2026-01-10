```yaml
number: 969
title: unresolved-import with installed stubs but not without
type: issue
state: closed
author: Spindel
labels:
  - imports
assignees: []
created_at: 2025-08-11T14:58:13Z
updated_at: 2025-08-11T15:41:07Z
url: https://github.com/astral-sh/ty/issues/969
synced_at: 2026-01-10T02:06:24Z
```

# unresolved-import with installed stubs but not without

---

_Issue opened by @Spindel on 2025-08-11 14:58_

### Summary

This was a fun one.

```
from authlib.integrations.requests_client import  OAuth2Session
assert OAuth2Session
```

with `authlib`  installed, this passes, but with `authlib-stubs` installed one gets:

    error[unresolved-import]: Cannot resolve imported module `authlib.integrations.requests_client`


lib versions:

```
  authlib==1.6.1
  types-authlib==1.6.0.20250711
```


### Version

0.0.1a17

---

_Label `imports` added by @AlexWaygood on 2025-08-11 15:06_

---

_Comment by @MichaReiser on 2025-08-11 15:41_

Thank you for reporting this. 

I checked and `types-authlib` uses partial typing which ty doesn't support yet. 

---

_Closed by @MichaReiser on 2025-08-11 15:41_

---
