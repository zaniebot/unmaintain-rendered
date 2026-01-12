```yaml
number: 1773
title: "No completions offered for `import importlib; x = importlib.machi<CURSOR>`"
type: issue
state: open
author: AlexWaygood
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-05T14:28:04Z
updated_at: 2025-12-08T13:07:33Z
url: https://github.com/astral-sh/ty/issues/1773
synced_at: 2026-01-12T15:54:25Z
```

# No completions offered for `import importlib; x = importlib.machi<CURSOR>`

---

_@AlexWaygood_

I would expect auto-import to offer a completion here that would automatically add an `import importlib.machinery` import to the top of the module. But no completions are offered at all currently:

```py
import importlib
x = importlib.machin<CURSOR>
```

Cc. @BurntSushi 

---

_Label `server` added by @AlexWaygood on 2025-12-05 14:28_

---

_Label `completions` added by @AlexWaygood on 2025-12-05 14:28_

---
