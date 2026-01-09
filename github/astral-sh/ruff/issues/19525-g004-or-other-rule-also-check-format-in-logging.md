---
number: 19525
title: G004 or other rule also check .format in logging
type: issue
state: closed
author: asukaminato0721
labels:
  - question
assignees: []
created_at: 2025-07-24T10:17:05Z
updated_at: 2025-07-24T12:48:09Z
url: https://github.com/astral-sh/ruff/issues/19525
synced_at: 2026-01-07T13:12:16-06:00
---

# G004 or other rule also check .format in logging

---

_Issue opened by @asukaminato0721 on 2025-07-24 10:17_

### Summary

currently <https://docs.astral.sh/ruff/rules/logging-f-string/> can check f-str in logging. But some old lib use

```py
logging.info("foo{}bar".format(...))
```

Idk whether this should be checked.


---

_Comment by @MichaReiser on 2025-07-24 11:57_

That would have to be its own rule because it's not an f-string in a logging call. 

Ruff will suggest and fix this to an f-string if you've UP032 enabled which will then trigger UP032

---

_Label `question` added by @MichaReiser on 2025-07-24 11:57_

---

_Comment by @ntBre on 2025-07-24 12:37_

I think [logging-string-format (G001)](https://docs.astral.sh/ruff/rules/logging-string-format/#logging-string-format-g001) will catch this: https://play.ruff.rs/5c305652-5236-44d0-9cc4-380a6dc82dfa.

---

_Closed by @asukaminato0721 on 2025-07-24 12:48_

---
