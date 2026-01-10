```yaml
number: 9007
title: "False positive for `PLR1706`"
type: issue
state: closed
author: UnknownPlatypus
labels:
  - bug
assignees: []
created_at: 2023-12-05T14:08:43Z
updated_at: 2024-02-01T19:35:05Z
url: https://github.com/astral-sh/ruff/issues/9007
synced_at: 2026-01-10T11:09:51Z
```

# False positive for `PLR1706`

---

_Issue opened by @UnknownPlatypus on 2023-12-05 14:08_

Here is a minimal example for the issue I had with PLR1706 and the autofix using ruff v0.1.7.

```diff
-print(True and False or 2) # This print '2'
+print(False if True else 2) # This print 'False'
```

I used this command:
```shell
echo "print(True and False or 2)" > t.py
ruff check --select PLR1706 --isolated --preview --fix --unsafe-fixes t.py
```

Generally speaking, `A and B or C` is transformed into `B if A else C` but this is not equivalent when `A` is truphy and `B` is falsy because the first one returns `C` and the second returns `B`.

Let me know if I'm missing something.

---

_Label `bug` added by @charliermarsh on 2023-12-05 14:45_

---

_Comment by @charliermarsh on 2023-12-05 14:45_

Does look like a bug, thanks for filing.

---

_Comment by @charliermarsh on 2023-12-05 19:50_

It turns out that this rule relies on type inference. If `B` is false-y, we shouldn't be triggering it.

---

_Comment by @charliermarsh on 2023-12-05 19:55_

Specifically, you have to be able to resolve `B` to a _specific_ value.

Even `x >= y and x or y` is not safe to convert when `x` and `y` are both integers -- it fails for `x, y = 0, -1`. (Pylint correctly does not flag this example, but I'm more pointing out that it's a very limited rule.)

We may want to deprecate.

---

_Comment by @UnknownPlatypus on 2023-12-05 20:37_

Ye I feel like with the current infrastructure it might be hard to get this right. 
This rule is targeting a very limited use case anyway since it's supposed to be a pattern for python <2.5.

I would probably deprecate it too, and remove the autofix altogether (it only introduced behavior changes when I tried it myself so I'd rather let people do manual changes if they really want the rule).

Anyway thanks for the quick reply and it's a pleasure to use ruff. 

---

_Comment by @charliermarsh on 2023-12-05 21:25_

👍 Makes sense. I'll mark it in the 0.2.0 issue. Thanks for the kind words, I appreciate it.

---

_Added to milestone `v0.2.0` by @MichaReiser on 2024-01-19 14:21_

---

_Assigned to @zanieb by @zanieb on 2024-01-30 01:21_

---

_Comment by @zanieb on 2024-01-30 01:23_

See #9691 which removes this rule

---

_Closed by @zanieb on 2024-02-01 19:35_

---
