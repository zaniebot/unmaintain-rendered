---
number: 2156
title: B007 may break code
type: issue
state: closed
author: spaceone
labels:
  - bug
assignees: []
created_at: 2023-01-25T14:58:43Z
updated_at: 2023-01-25T17:53:36Z
url: https://github.com/astral-sh/ruff/issues/2156
synced_at: 2026-01-07T13:12:14-06:00
---

# B007 may break code

---

_Issue opened by @spaceone on 2023-01-25 14:58_

```
$ cat foo.py
FMT = '{foo} {bar}'
for foo, bar in [(1, 2)]:
    if foo:
        print(FMT.format(**locals()))
$ ruff --select B007 --fix foo.py
Found 1 error(s) (1 fixed, 0 remaining).
$ cat foo.py
FMT = '{foo} {bar}'
for foo, _bar in [(1, 2)]:
    if foo:
        print(FMT.format(**locals()))
$ python3 foo.py
Traceback (most recent call last):
  File "foo.py", line 4, in <module>
    print(FMT.format(**locals()))
KeyError: 'bar'
```

I don't know what to do with this. Maybe just ignore it / WONTFIX.
What could be possible to mark some fixers as safe/unsafe for side-effects.

---

_Label `bug` added by @charliermarsh on 2023-01-25 15:17_

---

_Comment by @charliermarsh on 2023-01-25 15:18_

In this case, we should probably detect the use of `locals` and avoid flagging B007 at all. So I think I consider this a bug...

But separately: we are working on marking fixes as safe and unsafe, and allowing users to set their desired fix level.

You can also turn off autofix for B007 or any set of rules by adding this to your `pyproject.toml`:

```toml
[tool.ruff]
unfixable = ["B007"]
```


---

_Referenced in [astral-sh/ruff#2161](../../astral-sh/ruff/pulls/2161.md) on 2023-01-25 17:15_

---

_Closed by @charliermarsh on 2023-01-25 17:53_

---

_Referenced in [astral-sh/ruff#2397](../../astral-sh/ruff/issues/2397.md) on 2023-01-31 14:22_

---
