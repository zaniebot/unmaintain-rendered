---
number: 1050
title: I001 weird asterisk behavior
type: issue
state: closed
author: guyrosin
labels:
  - bug
assignees: []
created_at: 2022-12-05T08:14:29Z
updated_at: 2022-12-05T23:05:37Z
url: https://github.com/astral-sh/ruff/issues/1050
synced_at: 2026-01-07T13:12:14-06:00
---

# I001 weird asterisk behavior

---

_Issue opened by @guyrosin on 2022-12-05 08:14_

Given the following two imports:
```
from some_module import *
from some_module import some_class
```
Ruff raises `I001` and autofixes to this invalid import statement: 
```from some_module import *, some_class```

---

_Renamed from "`I0001` weird asterisk behavior" to "I001 weird asterisk behavior" by @guyrosin on 2022-12-05 08:15_

---

_Comment by @guyrosin on 2022-12-05 08:24_

In addition: `# noqa: I001` doesn't seem to ignore this error

---

_Comment by @UnknownPlatypus on 2022-12-05 10:34_

I had a similar issue with multiline wrapping resulting in invalid python code:

```diff
-from .subscription import * # type: ignore  # some very long comment explaining why this needs a type ignore
+from .subscription import ( # type: ignore  # some very long comment explaining why this needs a type ignore
+    *,
+)
```

I think star import should probably be special cased.

---

_Label `bug` added by @charliermarsh on 2022-12-05 14:03_

---

_Comment by @charliermarsh on 2022-12-05 14:03_

Good call, will fix today.

---

_Comment by @charliermarsh on 2022-12-05 16:18_

@guyrosin - In the interim, you can use `isort: off` to ignore this --

```py
# isort: off
from some_module import *
from some_module import some_class
# isort: on
```

---

_Referenced in [astral-sh/ruff#1066](../../astral-sh/ruff/pulls/1066.md) on 2022-12-05 16:48_

---

_Closed by @charliermarsh on 2022-12-05 16:48_

---

_Comment by @UnknownPlatypus on 2022-12-05 23:02_

@charliermarsh fyi, the other star import related case I mentioned above is still broken with `0.0.161`

Should I create another issue for that ?


---

_Comment by @charliermarsh on 2022-12-05 23:03_

@UnknownPlatypus - Oh sorry! I missed that. Feel free to create another issue, I'll fix it tonight.

---

_Comment by @UnknownPlatypus on 2022-12-05 23:05_

No worries! 

thank for the great work on this project, would love ton contribute but I'm not yet skilled enough with rust.
I'm creating another issue for the above use case!

Cheers

---
