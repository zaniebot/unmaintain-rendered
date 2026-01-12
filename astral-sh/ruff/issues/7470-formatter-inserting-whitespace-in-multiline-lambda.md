```yaml
number: 7470
title: "Formatter: Inserting whitespace in multiline lambda "
type: issue
state: closed
author: shughes-uk
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-09-17T19:24:05Z
updated_at: 2023-09-19T19:23:54Z
url: https://github.com/astral-sh/ruff/issues/7470
synced_at: 2026-01-12T15:54:47Z
```

# Formatter: Inserting whitespace in multiline lambda 

---

_@shughes-uk_

With ruff 0.0.289

Before running 
```python
def foo():
    return 1


x = (
    lambda x:
    # a comment
    foo()
)
```

After `ruff format`

```python
def foo():
    return 1


x = (
    lambda x: # there is trailing whitespace here now
    # a comment
    foo()
)
```

---

_Label `bug` added by @charliermarsh on 2023-09-17 19:25_

---

_Label `formatter` added by @charliermarsh on 2023-09-17 19:25_

---

_Added to milestone `Formatter: Beta` by @charliermarsh on 2023-09-17 19:25_

---

_Comment by @charliermarsh on 2023-09-17 19:25_

Thanks!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-17 20:10_

---

_Comment by @charliermarsh on 2023-09-17 20:10_

Note to self: while here, also need to fix the formatting of:

```python
lambda: ( # foo
    bar
)
```

---

_Closed by @charliermarsh on 2023-09-19 19:23_

---
