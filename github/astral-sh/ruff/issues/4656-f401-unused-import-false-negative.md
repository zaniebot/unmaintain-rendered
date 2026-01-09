---
number: 4656
title: F401 unused import false negative
type: issue
state: open
author: sbrugman
labels:
  - bug
  - needs-decision
assignees: []
created_at: 2023-05-25T13:25:40Z
updated_at: 2025-04-09T08:57:48Z
url: https://github.com/astral-sh/ruff/issues/4656
synced_at: 2026-01-07T13:12:14-06:00
---

# F401 unused import false negative

---

_Issue opened by @sbrugman on 2023-05-25 13:25_

False negative
```
import importlib # this package is unused, but not detected
import importlib.util 

package = "ruff"
is_present = importlib.util.find_spec(package)
```
When `from importlib import util` is used, correct behaviour is observed
```
import importlib # this package is unused, and detected
from importlib import util 

package = "ruff"
is_present = util.find_spec(package)
```

Ruff 'v0.0.270'

(hope I'm overlooking some submodule complexity in the general case, but it seems that in this specific case `import importlib` can be safely removed)


---

_Renamed from "F401 unused import false positive" to "F401 unused import false negative" by @sbrugman on 2023-05-25 13:29_

---

_Comment by @charliermarsh on 2023-05-25 14:08_

This is consistent with Pyflakes and I think it's intentional, but submodule imports are really complex, I'm not certain whether it's safe in the general case to mark `import a` as unused in:

```py
import a
import a.b

a.b.c()
```

It might be safe? I believe we had a PR long ago that addressed this: #58.

(Here's a good example of why / how submodules can be difficult, though it's not exactly the same as above: https://github.com/asottile/scratch/wiki/Why-doesn't-flake8-mark-this-as-an-unused-import%3F.)

---

_Label `question` added by @charliermarsh on 2023-05-25 14:08_

---

_Comment by @sbrugman on 2023-05-25 14:26_

Thanks for sharing the resource.

For what it's worth PyCharm does flag `import a` as unused import in the example above.

Can we think of examples where we would like to treat that example differently from this:

```python
import b
import a.b

a.b.c()
```
(The case in the linked resource above is the inverse)

Thinking out loud, but wouldn't `import a.b` also import `a`, so importing `a` is redundant?

---

_Comment by @konstin on 2023-05-25 14:57_

I'm struggling to find a case where `import a.b` is semantically different from `import a; import a.b` given that `import a.b` also runs `a/__init__.py` (https://docs.python.org/3/reference/import.html#the-meta-path, fourth paragraph)

---

_Comment by @charliermarsh on 2023-05-25 15:33_

Yeah, it might be the case that if we have `import a; import a.b` and then `a.b` is used but not _just_ `a`, then we can mark `import a` as unused.

On the other hand, per Anthony's linked note, if we have `import a; import a.b`, and then `a.z.x` is used, it seems like we can't actually treat `import a.b` as unused, since it _could_ modify `a`.

Similarly, if you have `import a; import a.b`, and then you do `print(a); print(a.b.c)`, I don't think we can treat either import as unused.

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:13_

---

_Label `bug` added by @charliermarsh on 2023-07-10 01:13_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:13_

---

_Referenced in [ZeitOnline/vivi#626](../../ZeitOnline/vivi/pulls/626.md) on 2024-02-21 14:29_

---

_Referenced in [astral-sh/ruff#10674](../../astral-sh/ruff/issues/10674.md) on 2024-03-30 23:12_

---

_Referenced in [astral-sh/ruff#11448](../../astral-sh/ruff/issues/11448.md) on 2024-05-17 14:26_

---

_Referenced in [astral-sh/ruff#13000](../../astral-sh/ruff/issues/13000.md) on 2024-08-20 03:11_

---

_Referenced in [astral-sh/ruff#15057](../../astral-sh/ruff/issues/15057.md) on 2024-12-19 12:07_

---

_Referenced in [astral-sh/ruff#17295](../../astral-sh/ruff/issues/17295.md) on 2025-04-08 13:42_

---

_Comment by @pkulev on 2025-04-09 08:57_

> On the other hand, per Anthony's linked note, if we have import a; import a.b, and then a.z.x is used, it seems like we can't actually treat import a.b as unused, since it could modify a.

Oh, indeed. I think that if something happens on import time, like filling some global variables from other modules (handlers registration or some `__init_subclass__` registration magic) it must be visible, and `# noqa: import for side-effects` is a good indicator of such things. I would prefer throwing a bunch of noqa's for such rare cases, than considering it normal.

---

_Referenced in [astral-sh/ruff#20115](../../astral-sh/ruff/pulls/20115.md) on 2025-08-27 17:36_

---

_Referenced in [astral-sh/ruff#20120](../../astral-sh/ruff/issues/20120.md) on 2025-08-28 01:54_

---

_Referenced in [astral-sh/ruff#20200](../../astral-sh/ruff/pulls/20200.md) on 2025-09-12 19:04_

---
