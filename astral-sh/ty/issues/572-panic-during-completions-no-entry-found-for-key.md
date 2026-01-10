```yaml
number: 572
title: "Panic during completions: `no entry found for key`"
type: issue
state: closed
author: MichaReiser
labels:
  - server
  - fatal
  - completions
assignees: []
created_at: 2025-06-03T08:26:12Z
updated_at: 2025-10-24T07:49:03Z
url: https://github.com/astral-sh/ty/issues/572
synced_at: 2026-01-10T02:06:24Z
```

# Panic during completions: `no entry found for key`

---

_Issue opened by @MichaReiser on 2025-06-03 08:26_

### Summary

Typing `def m` panics with the exception `no entry found for key`

This can be reproduced in the playground.

The issue seems that the scope id of the function's identifier can't be looked up. We'll need to make sure to register the function identifier during semantic index building

CC: @BurntSushi 

### Version

https://github.com/astral-sh/ruff/commit/67d94d9ec8f3d369af2e2acd8942978ea5c869d1

---

_Label `server` added by @MichaReiser on 2025-06-03 08:26_

---

_Label `fatal` added by @MichaReiser on 2025-06-03 08:26_

---

_Comment by @dhruvmanila on 2025-06-03 08:32_

I think this is related to https://github.com/astral-sh/ruff/issues/13778#issuecomment-2479383240

---

_Comment by @MichaReiser on 2025-06-03 08:35_

I don't think so. The playground also panics if you have 

```py
def m(): pass
```

And start typing after the `m` (only valid identifier characters)

---

_Comment by @BurntSushi on 2025-06-03 13:18_

Same thing happens for `class M<CURSOR>`.

---

_Label `completions` added by @AlexWaygood on 2025-09-30 15:52_

---

_Closed by @MichaReiser on 2025-10-24 07:49_

---
