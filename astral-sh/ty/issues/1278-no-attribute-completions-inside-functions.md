```yaml
number: 1278
title: No attribute completions inside functions
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - playground
  - completions
assignees: []
created_at: 2025-09-29T13:43:37Z
updated_at: 2025-10-20T09:46:50Z
url: https://github.com/astral-sh/ty/issues/1278
synced_at: 2026-01-10T02:06:25Z
```

# No attribute completions inside functions

---

_Issue opened by @sharkdp on 2025-09-29 13:43_

This seems to be specific to the playground? I would expect completions for `msg.<CURSOR>` here:

<img width="273" height="99" alt="Image" src="https://github.com/user-attachments/assets/fa89687c-dc39-4959-bc8a-6f1426ff251c" />

https://play.ty.dev/299f5fc7-cb2e-4050-96b8-d4d1eac191fc

Compare this with the in-editor LSP response:

<img width="732" height="173" alt="Image" src="https://github.com/user-attachments/assets/bac08594-5080-4372-918c-340be3309f8a" />

---

_Label `bug` added by @sharkdp on 2025-09-29 13:43_

---

_Label `playground` added by @sharkdp on 2025-09-29 13:43_

---

_Label `completions` added by @AlexWaygood on 2025-09-30 15:53_

---

_Closed by @sharkdp on 2025-10-20 09:46_

---
