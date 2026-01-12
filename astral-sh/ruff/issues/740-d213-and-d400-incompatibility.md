```yaml
number: 740
title: D213 and D400 incompatibility
type: issue
state: closed
author: andriilahuta
labels:
  - bug
assignees: []
created_at: 2022-11-14T17:42:09Z
updated_at: 2022-11-15T00:56:18Z
url: https://github.com/astral-sh/ruff/issues/740
synced_at: 2026-01-12T15:54:40Z
```

# D213 and D400 incompatibility

---

_@andriilahuta_

`D213` rule is not compatible with `D400` and some others.

Running `ruff --select D --ignore D100,D212` for the following file:
```py
def foo():
    """
    Foo.

    Bar.
    """
```
gives the following errors:
```
D400 First line should end with a period
D415 First line should end with a period, question mark, or exclamation point
```

---

_Label `bug` added by @charliermarsh on 2022-11-14 18:03_

---

_Comment by @charliermarsh on 2022-11-14 18:03_

Thanks, this looks like a bug since `pydocstyle` doesn't throw here.

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-14 18:05_

---

_Closed by @charliermarsh on 2022-11-14 18:07_

---

_Comment by @andriilahuta on 2022-11-15 00:56_

@charliermarsh thanks! I've also found a couple of new bugs:

- Apparently `noqa` doesn't work for multiline docstrings. The code below still gives the error I'm trying to suppress:
  ```py
  def foo():
      """
      Foo.
      """  # noqa: D200
  ```
- Spaceless multiline strings don't throw `E501`.
  This throws an error:
  ```py
  foo = '''
  xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  '''
  ```  
  And this doesn't:
  ```py
  foo = '''
  xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  '''
  ```

---
