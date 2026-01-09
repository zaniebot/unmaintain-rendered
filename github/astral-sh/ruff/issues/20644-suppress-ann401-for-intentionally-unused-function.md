---
number: 20644
title: Suppress ANN401 for intentionally-unused function arguments
type: issue
state: closed
author: colinrsmall
labels:
  - question
assignees: []
created_at: 2025-09-30T09:33:00Z
updated_at: 2025-12-10T17:35:35Z
url: https://github.com/astral-sh/ruff/issues/20644
synced_at: 2026-01-07T13:12:16-06:00
---

# Suppress ANN401 for intentionally-unused function arguments

---

_Issue opened by @colinrsmall on 2025-09-30 09:33_

### Summary

When overriding functions in Python, you are generally required to use an identical function signature to the function you are overriding. If you do not need to use an argument in the function signature, it is a common pattern to prepend the name of the argument with an underscore. Knowing that the arguments are unused, it doesn't make sense to strictly require type hinting of those arguments.

In my particular use case, this applies to defining `*args` and `**kwargs` in overridden functions. A simple solution may be to type hint these as `Any`, but this would require the completion of https://github.com/astral-sh/ruff/issues/677.

---

_Comment by @ntBre on 2025-09-30 12:22_

If you use the `typing.override` decorator, `ANN401` should be suppressed: [playground](https://play.ruff.rs/0f1fd892-d722-4e9d-9684-83296835b783). Would that work for your use case?

---

_Label `question` added by @ntBre on 2025-09-30 12:22_

---

_Closed by @MichaReiser on 2025-12-10 17:30_

---

_Comment by @colinrsmall on 2025-12-10 17:35_

@ntBre Apologies -- missed your message way back when! Yes, that would work and is likely preferable. Looks like this was handled either way. Thanks @MichaReiser!

---
