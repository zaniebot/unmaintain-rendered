```yaml
number: 15792
title: FURB171 Does not trigger when calling set(...)
type: issue
state: closed
author: naslundx
labels:
  - rule
  - preview
assignees: []
created_at: 2025-01-28T20:10:20Z
updated_at: 2025-05-29T18:59:50Z
url: https://github.com/astral-sh/ruff/issues/15792
synced_at: 2026-01-12T15:54:54Z
```

# FURB171 Does not trigger when calling set(...)

---

_@naslundx_

### Description

Minimal code snippet:

```python
if 1 in {1}:  # This triggers
    print("Single-element set")

if 1 in set([1]):  # This does not, but is equivalent
    print("Single-element set")
```

Command:
`ruff check --isolated testfile.py --select FURB171 --preview`


No other settings. 

Ruff version 0.9.3.

---

_Comment by @InSyncWithFoo on 2025-01-28 21:07_

These two are not equivalent:

```pycon
>>> set(1)
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    set(1)
    ~~~^^^
TypeError: 'int' object is not iterable
```

---

_Comment by @naslundx on 2025-01-29 08:01_

Sorry about that. Edited the original post, it should be `set([1])`. Indeed:

```python
>>> set([1]) == {1}
True
```



---

_Comment by @dylwil3 on 2025-01-29 17:25_

I'm not sure if the scope of the rule should be extended to include _composite_ constructions of singleton collections. There would be a lot of special cases to consider and it's not clear to me how one would draw the line for what should or shouldn't be considered. Already in this case if we add `set([1])` shouldn't we also add `set("a")` or `set((1,))`

---

_Label `rule` added by @dylwil3 on 2025-01-29 17:26_

---

_Label `needs-decision` added by @dylwil3 on 2025-01-29 17:26_

---

_Comment by @dylwil3 on 2025-01-30 13:32_

> I'm not sure if the scope of the rule should be extended to include _composite_ constructions of singleton collections. There would be a lot of special cases to consider and it's not clear to me how one would draw the line for what should or shouldn't be considered. Already in this case if we add `set([1])` shouldn't we also add `set("a")` or `set((1,))`

@AlexWaygood has convinced me that this extension in scope has more pros than cons - thanks for suggesting it, and sorry for  delaying things a bit!

---

_Label `needs-decision` removed by @dylwil3 on 2025-01-30 13:32_

---

_Label `preview` added by @dylwil3 on 2025-01-30 13:32_

---

_Closed by @ntBre on 2025-05-29 18:59_

---
