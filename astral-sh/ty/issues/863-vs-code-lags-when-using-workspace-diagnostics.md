```yaml
number: 863
title: VS Code lags when using workspace diagnostics
type: issue
state: closed
author: MichaReiser
labels:
  - performance
  - server
assignees: []
created_at: 2025-07-21T12:28:18Z
updated_at: 2025-07-21T16:20:55Z
url: https://github.com/astral-sh/ty/issues/863
synced_at: 2026-01-12T15:54:24Z
```

# VS Code lags when using workspace diagnostics

---

_@MichaReiser_

VS code starts to lag when using ty's workspace diagnostics if the project has many diagnostics. 

https://github.com/user-attachments/assets/963f4836-962c-4c43-9758-cadd2969c688


See how the scrolling sometimes "hangs". The frequency roughly aligns with the 2s poll interval used by VS code. 

I suspect the issue is that we send all diagnostics as part of every workspace diagnostic response. Alone deserializing the 30k diagnsotics will take a meaningful time in JS. The solution here is to do proper long-polling and sending diffs instead of full reports on every request.

CC: @dhruvmanila 

---

_Label `performance` added by @MichaReiser on 2025-07-21 12:28_

---

_Label `server` added by @MichaReiser on 2025-07-21 12:28_

---

_Comment by @dhruvmanila on 2025-07-21 16:18_

I think we could merge this into #824, but I'm fine to keep this open as well

---

_Comment by @MichaReiser on 2025-07-21 16:20_

Oh, I did a quick search and missed this issue. Yes, I think we can merge it. 

Using it on homeassistant made me realise that the optimization is more than a nice to have. Workspace diagnostics without it are somewhat painful to use.

---

_Closed by @MichaReiser on 2025-07-21 16:20_

---
