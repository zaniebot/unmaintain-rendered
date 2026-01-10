```yaml
number: 271
title: "Error unreachable executed with `type Foo = str | list[Foo]` in playground"
type: issue
state: closed
author: arjenzorgdoc
labels: []
assignees: []
created_at: 2025-05-08T12:46:51Z
updated_at: 2025-05-08T14:03:15Z
url: https://github.com/astral-sh/ty/issues/271
synced_at: 2026-01-10T02:34:09Z
```

# Error unreachable executed with `type Foo = str | list[Foo]` in playground

---

_Issue opened by @arjenzorgdoc on 2025-05-08 12:46_

### Summary

I get an error in the playground if I try to use a self referencing type:

```python
type Foo = str | list[Foo]

f: Foo 
```

https://play.ty.dev/37400645-e540-4f88-bd3b-12b1e380dce7

### Version

playground says: v0.0.0

![Image](https://github.com/user-attachments/assets/76a3c457-9980-4b33-ba44-7d4151b1d161)

---

_Comment by @sharkdp on 2025-05-08 14:03_

Thank you for reporting this! I will add this as an additional (slightly different) example to https://github.com/astral-sh/ty/issues/256.

---

_Closed by @sharkdp on 2025-05-08 14:03_

---
