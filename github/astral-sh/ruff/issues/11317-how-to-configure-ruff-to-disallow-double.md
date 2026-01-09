---
number: 11317
title: How to configure Ruff to disallow double assignments in single lines in Python?
type: issue
state: open
author: javinba
labels:
  - rule
assignees: []
created_at: 2024-05-07T08:21:17Z
updated_at: 2024-11-10T04:53:45Z
url: https://github.com/astral-sh/ruff/issues/11317
synced_at: 2026-01-07T13:12:15-06:00
---

# How to configure Ruff to disallow double assignments in single lines in Python?

---

_Issue opened by @javinba on 2024-05-07 08:21_

I'm looking for a way to configure Ruff to disallow double assignments in single lines, like this:

```python
name, position = files.name, pos
```
Instead, I want the linter to enforce separate assignments on separate lines, like this:
```python
name = files.name
position = pos
```
However, I still want to allow tuple unpacking, like this:
```
name, position = extract_details()
```
Is there a way to configure Ruff to achieve this behavior?

---

_Comment by @MichaReiser on 2024-05-07 09:01_

I don't think such a rule exists today. I just tried to see if Ruff flags the assignment when enabling all rules but it doesn't. 

https://play.ruff.rs/e1feb7f2-7c32-4bfd-ae87-b12102a382a4

---

_Comment by @javinba on 2024-05-07 12:50_

 Hi @MichaReiser , thanks for replying. Do you think it would be possible to add this rule? :slightly_smiling_face: 

---

_Label `rule` added by @zanieb on 2024-05-07 13:15_

---

_Comment by @harupy on 2024-09-11 10:07_

+1

---

_Label `type-inference` added by @MichaReiser on 2024-09-11 12:05_

---

_Label `type-inference` removed by @MichaReiser on 2024-09-11 19:05_

---

_Comment by @MichaReiser on 2024-09-11 19:07_

What would the logic for a rule like this. E.g would the following be allowed?

```python
b = [1, 2]
name, position, b = *b, 2
```



---

_Comment by @harupy on 2024-09-11 23:55_

I think that should be allowed.

---

_Referenced in [astral-sh/ruff#13587](../../astral-sh/ruff/issues/13587.md) on 2024-10-07 07:01_

---
