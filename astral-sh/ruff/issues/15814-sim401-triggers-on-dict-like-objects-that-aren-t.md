---
number: 15814
title: "SIM401 triggers on dict like objects that aren't proper dictionaries"
type: issue
state: closed
author: jogo-openai
labels:
  - good first issue
  - rule
  - help wanted
assignees: []
created_at: 2025-01-29T18:45:50Z
updated_at: 2025-02-07T08:25:22Z
url: https://github.com/astral-sh/ruff/issues/15814
synced_at: 2026-01-10T01:22:57Z
---

# SIM401 triggers on dict like objects that aren't proper dictionaries

---

_Issue opened by @jogo-openai on 2025-01-29 18:45_

### Description

```
$ ruff --version
ruff 0.9.3
```

```python
class Foo:
    def __init__(self):
        self._dict = {}

    def __getitem__(self, key):
        return self._dict[key]

    def __setitem__(self, key, value):
        self._dict[key] = value

    def __iter__(self):
        return self._dict.__iter__()



foo = Foo()
foo['key'] = 'value'


if "key" in foo:
    value = foo["key"]
else:
    value = None

print(value)


# print(foo.get('key', None))
# Triggers `AttributeError: 'Foo' object has no attribute 'get'`
```

`ruff check --fix  --unsafe-fixes --diff --select=SIM401  foo.py`
```
--- foo.py
+++ foo.py
@@ -17,10 +17,7 @@
 foo['key'] = 'value'


-if "key" in foo:
-    value = foo["key"]
-else:
-    value = None
+value = foo.get('key', None)

 print(value)


Would fix 1 error.
```

---

_Label `type-inference` added by @dylwil3 on 2025-01-29 19:07_

---

_Comment by @dylwil3 on 2025-01-29 19:12_

I'm slightly in favor of just updating the documentation (which is part of #15584) for this unsafe fix. An alternative is to restrict to instances where our limited type inference can tell that we have an honest `dict` object, but that would be a significant restriction in applicability. That said, false positives are more annoying than false negatives for this sort of rule, so I could be convinced of the second route.

---

_Label `needs-decision` added by @dylwil3 on 2025-01-29 19:12_

---

_Comment by @jogo-openai on 2025-01-30 17:17_

I have a slight preference for removing false positives and making sure things are actually dicts. That being said,  just updating the documentation for this unsafe fix also seems good to me (I suspect this means llarge code bases would opt to not use this rule).

---

_Comment by @MichaReiser on 2025-02-03 08:41_

SIM401 was added in 2023 when we didn't had any type inference in Ruff, which is why we didn't restrict it to *known dicts*. We've since then started to restrict new rules to known dicts only (e.g. RUF051) and I think it makes sense to do the same here. 

This is technically a breaking change for users that suppressed `SIM401` in cases where the value isn't a true dict but removing the false positives seems like a clear improvement. 

---

_Label `type-inference` removed by @MichaReiser on 2025-02-03 08:42_

---

_Label `needs-decision` removed by @MichaReiser on 2025-02-03 08:42_

---

_Label `good first issue` added by @MichaReiser on 2025-02-03 08:42_

---

_Label `help wanted` added by @MichaReiser on 2025-02-03 08:42_

---

_Label `rule` added by @MichaReiser on 2025-02-03 08:42_

---

_Referenced in [astral-sh/ruff#15995](../../astral-sh/ruff/pulls/15995.md) on 2025-02-06 14:38_

---

_Comment by @junhsonjb on 2025-02-06 15:05_

Hi! I'm super excited to be able to participate in this awesome project! I'm looking forward to working with you all towards a fix on this! Let me know if there's anything I need to change/fix/add/etc. 

I really like ruff so thanks for all y'all do on this project and for the community! Cheers!

---

_Closed by @MichaReiser on 2025-02-07 08:25_

---

_Closed by @MichaReiser on 2025-02-07 08:25_

---
