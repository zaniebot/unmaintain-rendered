---
number: 5027
title: UP013 does not account for functional use-cases
type: issue
state: closed
author: ergoithz
labels:
  - question
assignees: []
created_at: 2023-06-12T15:54:39Z
updated_at: 2023-06-12T21:23:41Z
url: https://github.com/astral-sh/ruff/issues/5027
synced_at: 2026-01-07T13:12:15-06:00
---

# UP013 does not account for functional use-cases

---

_Issue opened by @ergoithz on 2023-06-12 15:54_

pyupgrade UP013 does not check for when typed dict cannot be expressed as a class (in this example because of reserved keys).

```py
import typing
MyDictType = typing.TypedDict('MyDictType', {'id': str})
``` 

UP013 tells to migrate to class like:

```py
import typing
class MyDictType(typing.TypedDict):
    id : str
```
Which is a violation of A003 and one of the many reasons of functional APIs exist.

Version: 0.0.272

---

_Comment by @charliermarsh on 2023-06-12 15:59_

Can you say a bit more about the contexts in which you wouldn't want UP013 to trigger? The fact that it triggers A003 doesn't seem like a fully satisfying explanation to me, since in theory this should _still_ be an A003 violation, given that A003 is about creating classes with confusing attributes, and not the mechanism through which those attributes are defined.

---

_Label `question` added by @charliermarsh on 2023-06-12 15:59_

---

_Comment by @ergoithz on 2023-06-12 16:15_

@charliermarsh Since dict key's aren't subject to the same restrictions as class attributes, there are situations where a typed dict cannot be expressed using a class.

The explanation is simple, `{'__dict__': 'something'}` is a perfectly valid dictionary which cannot be expressed as per UP013 suggests.

---

_Comment by @charliermarsh on 2023-06-12 16:20_

Yup -- and we handle some of those! For example, we validate that the keys are valid Python identifiers before suggesting this rewrite. What I'm trying to understand is: what are other such use-cases? It looks like one that you're proposing is: special dunder methods.

---

_Comment by @ergoithz on 2023-06-12 16:36_

I may suggest including an extra check for keys that are valid identifiers in the strict sense, but will cause issues in a class definition context: builtins and reserved attribute names, preventing UP013 from triggering.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-12 20:10_

---

_Referenced in [astral-sh/ruff#5038](../../astral-sh/ruff/pulls/5038.md) on 2023-06-12 21:16_

---

_Closed by @charliermarsh on 2023-06-12 21:23_

---
