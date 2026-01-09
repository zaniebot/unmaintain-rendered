---
number: 20929
title: False positive for UP046
type: issue
state: closed
author: NeilGirdhar
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-10-16T19:53:51Z
updated_at: 2025-10-30T16:59:08Z
url: https://github.com/astral-sh/ruff/issues/20929
synced_at: 2026-01-07T13:12:16-06:00
---

# False positive for UP046

---

_Issue opened by @NeilGirdhar on 2025-10-16 19:53_

### Summary

UP046 triggers on Python 3.12 even when a default is given.  On Python 3.12, you can't specify a default value for a PEP695 type parameter.  Therefore, the rule should not trigger.

### Version

0.14.1

---

_Comment by @ntBre on 2025-10-16 19:59_

Could you share the code triggering UP046? I thought the `default` field for `typing.TypeVar` was also added in 3.13, so it seemed okay to transform a `TypeVar` with a default into a type parameter with a default. I could have missed something, though.

---

_Label `question` added by @ntBre on 2025-10-16 20:01_

---

_Comment by @NeilGirdhar on 2025-10-16 23:12_

> was also added in 3.13

It was.  The problem is that this transformation is occurring on Python 3.12.

> Could you share the code triggering UP046?

Do you mind pulling this repo? https://github.com/NeilGirdhar/efax

Its minimum version is 3.12, so it should not be triggering UP046.

---

_Comment by @ntBre on 2025-10-17 02:51_

Ah okay, I see. You're using `typing_extensions.TypeVar`, which backports the `default` argument to earlier versions. If you try to use `typing.TypeVar` with a default on 3.12, you also get an error, which is why I thought this transformation was okay. I forgot about `typing_extensions`.

We should just add the version check before suggesting a default.

---

_Label `question` removed by @ntBre on 2025-10-17 02:51_

---

_Label `bug` added by @ntBre on 2025-10-17 02:51_

---

_Label `help wanted` added by @ntBre on 2025-10-17 02:52_

---

_Comment by @NeilGirdhar on 2025-10-17 03:49_

Oh, nice catch!  Yes, your solution makes sense.  Thanks for taking the time to look at this :)

---

_Comment by @ntBre on 2025-10-17 12:46_

No problem, thanks for the report and example!

---

_Comment by @prakhar1144 on 2025-10-18 04:56_

I'm working on a fix for this.

(Posting to avoid duplicate work)

---

_Assigned to @prakhar1144 by @ntBre on 2025-10-18 16:03_

---

_Referenced in [astral-sh/ruff#21045](../../astral-sh/ruff/pulls/21045.md) on 2025-10-23 13:22_

---

_Closed by @ntBre on 2025-10-30 16:59_

---
