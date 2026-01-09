---
number: 3285
title: RET507 false positive only inside a function
type: issue
state: closed
author: trag1c
labels:
  - bug
assignees: []
created_at: 2023-02-28T23:19:27Z
updated_at: 2023-03-01T03:26:03Z
url: https://github.com/astral-sh/ruff/issues/3285
synced_at: 2026-01-07T13:12:14-06:00
---

# RET507 false positive only inside a function

---

_Issue opened by @trag1c on 2023-02-28 23:19_

<table>
  <tr>
    <td>Works fine here:</td>
    <td>RET507 detected here:</td>
  </tr>
  <tr>
    <td>

```py
x = 5
for char in "abc":
    if char == "a":
        x = 1
    elif char == "b":
        x = 2
        continue
    else:
        x = 3
print(x)
```

</td>
<td>

```py
def foo() -> None:
    x = 5
    for char in "abc":
        if char == "a":
            x = 1
        elif char == "b":
            x = 2
            continue
        else:
            x = 3
    print(x)
```

</td>
</tr>
</table>

https://user-images.githubusercontent.com/77130613/222003075-0083d3c5-1996-401d-bdd0-77245c20d446.mov



---

_Label `bug` added by @charliermarsh on 2023-02-28 23:20_

---

_Comment by @trag1c on 2023-02-28 23:58_

Actually, it looks like it could be a https://github.com/charliermarsh/ruff-vscode issue? I tried running `ruff --select RET507 file.py` on the very same code (on both `0.0.247` and `0.0.253`) and had no errors show up.

---

_Comment by @charliermarsh on 2023-03-01 00:43_

Oh, this might've been fixed a while ago.

---

_Comment by @charliermarsh on 2023-03-01 00:43_

I think this was fixed in #2881.

---

_Comment by @charliermarsh on 2023-03-01 00:44_

Do you know what version of the VS Code extension you're on?

---

_Comment by @charliermarsh on 2023-03-01 03:26_

Just confirmed that this is no longer an issue on `main`.

---

_Closed by @charliermarsh on 2023-03-01 03:26_

---
