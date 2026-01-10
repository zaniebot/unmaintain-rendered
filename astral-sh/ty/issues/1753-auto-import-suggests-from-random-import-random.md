```yaml
number: 1753
title: "auto-import suggests `from random import random` but not `import random`"
type: issue
state: closed
author: Gankra
labels:
  - bug
  - completions
assignees: []
created_at: 2025-12-04T13:55:55Z
updated_at: 2025-12-04T22:37:39Z
url: https://github.com/astral-sh/ty/issues/1753
synced_at: 2026-01-10T01:56:41Z
```

# auto-import suggests `from random import random` but not `import random`

---

_Issue opened by @Gankra on 2025-12-04 13:55_

### Summary

You want the stdlib module `random` here but instead you get suggested `random.random` (`from random import random`). It's fine to suggest `random.random` here as an *option* (especially when you're in auto-complete and have only typed `random`) but it's weird that `import random` isn't suggested here at all.

```py
random.randint(0, 1)
```

https://play.ty.dev/33291b61-8019-41a4-b32f-222a8ed26af7

<img width="414" height="177" alt="Image" src="https://github.com/user-attachments/assets/230ce938-c91d-43dd-ad16-e401d8bae5a3" />

### Version

_No response_

---

_Label `completions` added by @Gankra on 2025-12-04 13:56_

---

_Comment by @BurntSushi on 2025-12-04 13:58_

I think this might be a dupe of https://github.com/astral-sh/ty/issues/1530? Although this is specifically for the auto-import code action, so it might be worth keeping both open.

---

_Label `bug` added by @Gankra on 2025-12-04 13:58_

---

_Comment by @AlexWaygood on 2025-12-04 13:59_

> I think this might be a dupe of [#1530](https://github.com/astral-sh/ty/issues/1530)? Although this is specifically for the auto-import code action, so it might be worth keeping both open.

heh, I was 5 seconds away from closing this as a dupe 😆

---

_Comment by @Gankra on 2025-12-04 13:59_

I would say it's a dupe, it happens in auto-complete too.

---

_Closed by @Gankra on 2025-12-04 13:59_

---

_Closed by @BurntSushi on 2025-12-04 22:37_

---
