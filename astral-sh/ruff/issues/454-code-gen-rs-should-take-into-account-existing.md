```yaml
number: 454
title: "`code_gen.rs` should take into account existing code preferences"
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-10-18T21:29:53Z
updated_at: 2022-12-28T00:45:51Z
url: https://github.com/astral-sh/ruff/issues/454
synced_at: 2026-01-12T15:54:40Z
```

# `code_gen.rs` should take into account existing code preferences

---

_@charliermarsh_

It'd be great to generate code that's aware of existing indentation and quotation preferences.


---

_Label `enhancement` added by @charliermarsh on 2022-10-18 21:29_

---

_Label `good first issue` added by @charliermarsh on 2022-10-18 21:29_

---

_Label `good first issue` removed by @charliermarsh on 2022-10-27 17:08_

---

_Comment by @leos on 2022-12-27 19:28_

I just ran into this as well with `E712` on the following example code:
```python
image = {"primary": 23}

if image["primary"] == True:
  print("hello!")
```
Running `ruff` 0.0.196 on this results in:
```python
image = {"primary": 23}

if image['primary'] is True:
  print("hello!")
```

If the intent is to have `ruff` work with `Black` out of the box then it should default to double quotes - although obviously it would be nice to detect the quoting style before the fix as well.

 

---

_Comment by @charliermarsh on 2022-12-27 19:57_

Yeah agreed. It actually used to double-quote, but it required forking RustPython. I’ll look into it again and see if we can at least preserve double quotes, since it’d go a long way towards compatibility.

---

_Comment by @charliermarsh on 2022-12-27 20:33_

(Also: thank you for the clear example + version number etc.)

---

_Comment by @charliermarsh on 2022-12-27 20:53_

I'm gonna ship some improvements to this today.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-27 21:19_

---

_Closed by @charliermarsh on 2022-12-28 00:45_

---
