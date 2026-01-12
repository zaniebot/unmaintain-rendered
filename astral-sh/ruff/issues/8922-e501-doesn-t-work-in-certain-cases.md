```yaml
number: 8922
title: "E501 doesn't work in certain cases"
type: issue
state: closed
author: yaroslavsadin
labels:
  - bug
assignees: []
created_at: 2023-11-30T11:15:45Z
updated_at: 2023-11-30T14:27:50Z
url: https://github.com/astral-sh/ruff/issues/8922
synced_at: 2026-01-12T15:54:48Z
```

# E501 doesn't work in certain cases

---

_@yaroslavsadin_

```
$ ruff --version
ruff 0.1.6
```

No custom TOML config used.

Good code snippet:
```python
{
    i: i for i in ['somtehingverylongsomtehingverylongsomtehingverylongsomtehingverylongsomtehingverylongsomtehingverylongsomtehingverylong']
}
```
```console
$ ruff --select=E501 test.py
test.py:2:89: E501 Line too long (141 > 88)
Found 1 error.
```

Bad code snippet:
```python
{
    i: i for i in
    ['somtehingverylongsomtehingverylongsomtehingverylongsomtehingverylongsomtehingverylongsomtehingverylongsomtehingverylong']
}
```
```console
$ ruff --select=E501 test.py
$
```


---

_Comment by @charliermarsh on 2023-11-30 14:27_

Thanks for the clear issue. I think this is roughly the same as https://github.com/astral-sh/ruff/issues/8383 and there's some 
more commentary in that issue + a few ideas for improvement. The gist of it is that if a line doesn't contain any splittable whitespace, we avoid flagging E501, which is more often helpful than hurtful, since a line without splittable whitespace tends to be, e.g., a single long URL or other long token. In this case, though, we should _probably_ be flagging E501, since we can split on the bracket at least.

---

_Closed by @charliermarsh on 2023-11-30 14:27_

---

_Comment by @charliermarsh on 2023-11-30 14:27_

(Closing to merge with #8383.)

---

_Label `bug` added by @charliermarsh on 2023-11-30 14:27_

---
