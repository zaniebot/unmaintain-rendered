```yaml
number: 1253
title: Option to squelch pre-release warning
type: issue
state: closed
author: gtwohig
labels: []
assignees: []
created_at: 2025-09-25T12:51:46Z
updated_at: 2025-09-28T12:22:38Z
url: https://github.com/astral-sh/ty/issues/1253
synced_at: 2026-01-12T15:54:24Z
```

# Option to squelch pre-release warning

---

_@gtwohig_

Agentic coding really gets thrown for a loop due to this warning output. I don't know how many times it has tried to switch to mypy or ignored valid type errors. I'd like to be able to disable the warning via a setting in my pyproject config.

---

_Comment by @MichaReiser on 2025-09-25 12:58_

For now, you should be able to use `TY_LOG=ty=error ty` but it will suppress other warnings too (but not diagnostics with warning severity). 

I think we'll remove the message soon.

---

_Comment by @gtwohig on 2025-09-28 12:22_

Thanks! that works

---

_Closed by @gtwohig on 2025-09-28 12:22_

---
