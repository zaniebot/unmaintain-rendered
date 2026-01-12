```yaml
number: 11319
title: "[Rule request] Missing return statement"
type: issue
state: open
author: mrcljx
labels:
  - rule
assignees: []
created_at: 2024-05-07T09:11:20Z
updated_at: 2024-05-15T12:35:13Z
url: https://github.com/astral-sh/ruff/issues/11319
synced_at: 2026-01-12T15:54:51Z
```

# [Rule request] Missing return statement

---

_@mrcljx_

I just stumbled over a bug in our codebase:

```
def f() -> int:
  return 42

def g() -> int | None:
  f()
```

The last line should have been `return f()`.

While [Mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=5b3ede9342f1ab85c577a6373fda57ea) reports a `Missing return statement`, [Pyright](https://pyright-play.net/?strict=true&code=CYUwZgBA5gFAlBAtAPggSwHYBcBcAoCQiAJxCwFdiMIAWAJjz1EjHiVUywgB8IA5APYYQ%2BItHh4gA) does not. This is a [deliberate decision by the Pyright maintainer (point 2)](https://github.com/microsoft/pyright/issues/4027#issuecomment-1272636513) as there is no type issue here. In this case Mypy actually does some static analysis outside of its main responsibility.

I'm wondering whether there could be a Ruff rule for this. Often, Ruff lacks the type information. But in this case type information isn't needed. If a function:

1. has a return annotation,
2. and it's different than `-> None` or `-> NoReturn`,
3. and the body
    1. isn't a stub (`...` or `pass`) 
    2. doesn't have a `return` *anywhere* (in that case RET503 takes over)
    3. and control flow can reach the end of the body
4. report an error

```python
def ok_1() -> int | None:
  return 2

def ok_2() -> int | None:
  return # < maybe a second rule later could reject `return` without a value in this scenario

def ok_3() -> int | None:
  raise NotImplementedError

def ok_4() -> int | None: ...

def ok_5() -> int | None:
  pass

def ok_6() -> int | None:
  if ...:
    return 42 # RET503 will cause an error here

def ok_7():
  f()
```

```python
def bad_1() -> int | None:
  f()

def bad_2() -> int | None:
  if ...:
    raise ValueError()
```

---

_Comment by @autinerd on 2024-05-08 04:28_

I think it would be a good idea to extend RET503 to take this into account.

---

_Label `rule` added by @charliermarsh on 2024-05-08 14:21_

---

_Comment by @JonathanPlasse on 2024-05-15 12:35_

I think this could be a good first issue.

---
