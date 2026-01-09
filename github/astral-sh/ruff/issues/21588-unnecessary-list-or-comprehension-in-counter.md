---
number: 21588
title: "Unnecessary `list()` or comprehension in `Counter()`"
type: issue
state: open
author: injust
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-11-23T10:32:16Z
updated_at: 2025-12-03T05:24:11Z
url: https://github.com/astral-sh/ruff/issues/21588
synced_at: 2026-01-07T13:12:16-06:00
---

# Unnecessary `list()` or comprehension in `Counter()`

---

_Issue opened by @injust on 2025-11-23 10:32_

### Summary

`Counter` (as a dict subclass) should have a rule similar to https://docs.astral.sh/ruff/rules/unnecessary-comprehension-in-call/ and https://docs.astral.sh/ruff/rules/unnecessary-list-comprehension-dict/:

```python
Counter([(x, f(x)) for x in foo])
Counter([x.id for x in foo])

# above lines should be rewritten to:
Counter((x, f(x)) for x in foo)
Counter(x.id for x in foo)
```

As well as https://docs.astral.sh/ruff/rules/unnecessary-comprehension/:

```python
Counter([x for x in foo])
# above line is rewritten to:
Counter(list(foo))
# but it should really be rewritten to:
Counter(foo)
# this should be rewritten too:
Counter(x for x in foo)
```

---

_Renamed from "`unnecessary-list-comprehension-dict` (C404) for `Counter`" to "`unnecessary-comprehension-in-call` (C419) for `Counter`" by @injust on 2025-11-23 10:49_

---

_Renamed from "`unnecessary-comprehension-in-call` (C419) for `Counter`" to "Unnecessary comprehensions in `Counter()`" by @injust on 2025-11-23 10:51_

---

_Renamed from "Unnecessary comprehensions in `Counter()`" to "Unnecessary `list()` or comprehension in `Counter()`" by @injust on 2025-11-23 10:51_

---

_Label `rule` added by @amyreese on 2025-11-24 19:26_

---

_Label `needs-decision` added by @amyreese on 2025-11-24 19:27_

---

_Comment by @amyreese on 2025-11-24 19:27_

Feels like a reasonable extension of the existing rules to me.

---

_Comment by @MichaReiser on 2025-11-25 07:52_

I think the challenge here will be to infer whether `Counter` is a dict subclass or not and this requires multifile analysis to do reliable. Only supporting this pattern in the same file as the class is defined seems to limited to me and gives users a false sense of safety.

---

_Label `type-inference` added by @MichaReiser on 2025-11-25 07:52_

---

_Comment by @amyreese on 2025-11-25 21:49_

> I think the challenge here will be to infer whether `Counter` is a dict subclass or not and this requires multifile analysis to do reliable. Only supporting this pattern in the same file as the class is defined seems to limited to me and gives users a false sense of safety.

`Counter` is [part of the stdlib](https://docs.python.org/3/library/collections.html#collections.Counter), so we should always know that it's a subtype of dict.

---

_Label `type-inference` removed by @MichaReiser on 2025-11-26 07:08_

---

_Comment by @Avasam on 2025-12-03 05:24_

On my phone right now so I can't validate, but wouldn't this have the same issues as https://github.com/astral-sh/ruff/issues/12912 ?

---
