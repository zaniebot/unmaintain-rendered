```yaml
number: 4181
title: "[feature] Differentiate between safe and unsafe fixes"
type: issue
state: closed
author: MichaReiser
labels:
  - core
  - help wanted
assignees: []
created_at: 2023-05-02T09:07:02Z
updated_at: 2024-01-10T17:07:17Z
url: https://github.com/astral-sh/ruff/issues/4181
synced_at: 2026-01-12T15:54:44Z
```

# [feature] Differentiate between safe and unsafe fixes

---

_@MichaReiser_

This task tracks the work implementing safe and unsafe fixes as described in #1997. 

*Safe* fixes are fixes that don't change the semantics of the program. *Unsafe* fixes may change the program's semantics; therefore, we advise users to manually review and test the changes.

## Tasks 

* [x] #4182
* [x] #4183
* [x] #4340
* [x] #4276
* [x] #4184
* [x] #4185
* [x] #4186
* [x] #4187

---

_Label `help wanted` added by @MichaReiser on 2023-05-02 09:07_

---

_Renamed from "Differentiate between safe and unsafe fixes" to "[feature] Differentiate between safe and unsafe fixes" by @MichaReiser on 2023-05-02 12:22_

---

_Label `core` added by @MichaReiser on 2023-05-10 06:43_

---

_Comment by @evanrittenhouse on 2023-06-19 22:45_

@MichaReiser You can assign this to me, I'd like to take it to completion if it's open!

---

_Comment by @MichaReiser on 2023-06-21 13:52_

> @MichaReiser You can assign this to me, I'd like to take it to completion if it's open!

Hey @evanrittenhouse. Do you plan to work on all of it or individual sub-tasks?

---

_Comment by @evanrittenhouse on 2023-06-21 14:16_

From what I can tell, @zanieb has done quite a bit of progress on #4276 already. I'd happily take on #4184 - 87 though (and 76, if Zanie doesn't want to do it anymore).

 I have a draft PR for #4185 out already that hinges upon the completion of #4184, so I'm trying to knock that out. There's a list of the remaining linters that have `unspecified` fixes in the description for #4185. Since I'm already doing those, I figured I could just also take on #4186 and #4187, if nobody has any objections!

---

_Assigned to @evanrittenhouse by @MichaReiser on 2023-06-21 14:27_

---

_Assigned to @zanieb by @zanieb on 2023-09-07 14:48_

---

_Comment by @MichaReiser on 2023-10-17 00:57_

@zanieb should we close this issue? I'm okay deferring the add "message to fix" for now as it also is somewhat unrelated to the applicability work.

---

_Closed by @zanieb on 2023-10-17 03:03_

---
