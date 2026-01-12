```yaml
number: 1255
title: "FR: flake8-errmsg"
type: issue
state: closed
author: ofek
labels: []
assignees: []
created_at: 2022-12-16T00:31:51Z
updated_at: 2022-12-16T04:11:00Z
url: https://github.com/astral-sh/ruff/issues/1255
synced_at: 2026-01-12T15:54:41Z
```

# FR: flake8-errmsg

---

_@ofek_

https://github.com/henryiii/flake8-errmsg

---

A checker for flake8 that helps format nice error messages.

The issue is that Python includes the line with the raise in the default
traceback (and most other formatters, like Rich and IPython to too). That means
a user gets a message like this:

```python
sub = "Some value"
raise RuntimeError(f"{sub!r} is incorrect")
```

```pytb
Traceback (most recent call last):
  File "tmp.py", line 2, in <module>
    raise RuntimeError(f"{sub!r} is incorrect")
RuntimeError: 'Some value' is incorrect
```

If this is longer or more complex, the duplication can be quite confusing for a
user unaccustomed to reading tracebacks.

While if you always assign to something like `msg`, then you get:

```python
sub = "Some value"
msg = f"{sub!r} is incorrect"
raise RunetimeError(msg)
```

```pytb
Traceback (most recent call last):
  File "tmp.py", line 3, in <module>
    raise RuntimeError(msg)
RuntimeError: 'Some value' is incorrect
```

Now there's a simpler traceback, less code, and no double message. If you have a
long message, this also often formats better when using Black, too.

---

_Comment by @edgarrmondragon on 2022-12-16 03:09_

This seems like a no-brainer. I'm gonna give it a try.

---

_Closed by @charliermarsh on 2022-12-16 04:11_

---
