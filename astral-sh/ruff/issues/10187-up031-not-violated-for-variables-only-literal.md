```yaml
number: 10187
title: UP031 not violated for variables, only literal values
type: issue
state: closed
author: lev-blit
labels:
  - good first issue
  - rule
assignees: []
created_at: 2024-03-01T19:32:34Z
updated_at: 2024-04-22T15:40:52Z
url: https://github.com/astral-sh/ruff/issues/10187
synced_at: 2026-01-12T15:54:50Z
```

# UP031 not violated for variables, only literal values

---

_@lev-blit_

UP031 (`printf-string-formatting`, suggests using `str.format` instead of percent formatting) doesn't trigger when using a variable and not a literal value.

Even using the example from the docs - 

```python
value = 1
print("%s" % value)  # seems to be OK

print("%s" % 1)  # violating UP031
```

I've tried this out in the rust playground as well ([here's an example](https://play.ruff.rs/5477131a-fc56-44d7-bbfb-da4b9607428b)) with version 0.3.0, selecting all `UP` rules


---

_Comment by @charliermarsh on 2024-03-01 19:58_

I believe the docs are wrong the rule is correct. It's not safe to inject an arbitrary variable unless we can definitively infer its type. Here's an example to illustrate why:

```python
>>> value = 1
>>> print("%s" % value)
1
>>> value = (1,)
>>> print("%s" % value)
1
>>> value = 1
>>> print(f"{value}")
1
>>> value = (1,)
>>> print(f"{value}")
(1,)
```

We could improve the logic a bit to allow replacements in cases in which we _can_ definitively infer the type (like above), but in general we won't always be able to do this conversion.

---

_Label `rule` added by @charliermarsh on 2024-03-02 01:29_

---

_Label `good first issue` added by @zanieb on 2024-03-11 17:32_

---

_Comment by @zanieb on 2024-03-11 17:33_

The documentation part seems like a good first issue. Inference of the type seems a bit harder to implement but definitely doable if someone is interested.

---

_Comment by @omar-abdelgawad on 2024-04-13 00:38_

Hi, I am interested in taking the documentation part of this issue. I am also new here :smile:.

However, I need some clarifications regarding what needs to be updated. From what I have understood, The rule implementation is correct and it is successful in detecting both safe (when we know its definitely not a tuple) and unsafe (when there is a chance it might be a tuple) fixes. The only part missing from the rule is further type detection to find all cases where the variable is NOT a tuple. The thing is, when I read the [docs](https://docs.astral.sh/ruff/rules/printf-string-formatting/) now I understand the exact same thing with no mistakes found.

Did some PR fix this already and forget to mention this issue or am I just confused?  

---

_Comment by @zanieb on 2024-04-18 00:07_

Hey @omar-abdelgawad — you're right this example from the docs is already a part of the "Known problems" section. I don't think there's a documentation change to be made here until we improve inference.

---

_Comment by @zanieb on 2024-04-18 00:07_

Welcome to the project :) sorry about the confusion.

---

_Assigned to @plredmond by @plredmond on 2024-04-22 15:30_

---

_Closed by @plredmond on 2024-04-22 15:40_

---
