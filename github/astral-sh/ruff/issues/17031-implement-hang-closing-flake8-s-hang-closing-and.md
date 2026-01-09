---
number: 17031
title: "Implement hang closing (`flake8`'s `hang_closing` and `E123`/`E133`)"
type: issue
state: closed
author: andrei-korshikov
labels: []
assignees: []
created_at: 2025-03-28T07:57:17Z
updated_at: 2025-03-28T15:17:37Z
url: https://github.com/astral-sh/ruff/issues/17031
synced_at: 2026-01-07T13:12:16-06:00
---

# Implement hang closing (`flake8`'s `hang_closing` and `E123`/`E133`)

---

_Issue opened by @andrei-korshikov on 2025-03-28 07:57_

### Summary

`E123`—"closing bracket does not match indentation of opening bracket's line":

```python
dict_sample = {
    'key': 'value',
    }
```

with

```ini
[flake8]
hang_closing = False
```


`E133`—"closing bracket is missing indentation":

```python
dict_sample = {
    'key': 'value',
}
```

with

```ini
[flake8]
hang_closing = True
```

I use flake8's `hang_closing = True` so I'm interested if it will be implemented in Ruff.

`E123` and `E133` are not listed in #2402. Does that mean that you don't plan to support hang closing?

Related issue: #13897.

---

_Comment by @MichaReiser on 2025-03-28 15:17_

Yes, we don't plan to implement `E133` because it directly conflicts with `ruff format`.

---

_Closed by @MichaReiser on 2025-03-28 15:17_

---
