```yaml
number: 3571
title: "E721 False negative when variable name is \"types\""
type: issue
state: closed
author: strokirk
labels:
  - bug
  - good first issue
assignees: []
created_at: 2023-03-17T09:22:06Z
updated_at: 2023-03-17T18:29:07Z
url: https://github.com/astral-sh/ruff/issues/3571
synced_at: 2026-01-12T15:54:43Z
```

# E721 False negative when variable name is "types"

---

_@strokirk_

Hi!

If you alias a variable to something like `types`, `E721` will trigger incorrectly.

```
class NS:
    X = "x"


x = "x"
types = NS
if x == types.X:
    print("x")
```

![image](https://user-images.githubusercontent.com/1257824/225863532-32471613-2275-49fd-851e-f043d51871cd.png)

**ruff version**: 0.0.254

---

_Label `bug` added by @charliermarsh on 2023-03-17 14:46_

---

_Label `good first issue` added by @charliermarsh on 2023-03-17 14:46_

---

_Comment by @charliermarsh on 2023-03-17 14:46_

👍 We need to verify that `types` hasn't been overridden. We do this in a lot of places, but must've missed this one!

---

_Closed by @charliermarsh on 2023-03-17 18:29_

---
