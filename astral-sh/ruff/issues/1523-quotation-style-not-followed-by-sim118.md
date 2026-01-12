```yaml
number: 1523
title: "Quotation style not followed by `SIM118`"
type: issue
state: closed
author: thomasdesr
labels:
  - bug
assignees: []
created_at: 2023-01-01T03:02:45Z
updated_at: 2023-01-01T21:52:51Z
url: https://github.com/astral-sh/ruff/issues/1523
synced_at: 2026-01-12T15:54:41Z
```

# Quotation style not followed by `SIM118`

---

_@thomasdesr_

I saw https://github.com/charliermarsh/ruff/pull/1413 merged and re-ran but SIM118 seems to still be generating single quotes.

I see it wasn't touched by this #1413 so its probably just an oversight.

But many thanks, this tool has been great :D

Repro:

```python
# $ cat main.py
a = {
    "values": {i:i for i in range(10)}
}

for x in a['values']:
    print(x)
```

```
$ ruff --no-fix main.py
main.py:5:5: SIM118 Use `x in a['values']` instead of `x in a['values'].keys()`
  |
5 | for x in a["values"].keys():
  |     ^^^^^^^^^^^^^^^^^^^^^^^ SIM118
  |
  = help: Convert to `x in a['values']`

Found 1 error(s).
1 potentially fixable with the --fix option.
```

```
$ ruff --no-fix main.py --fix
main.py:5:12: Q000 Single quotes found but double quotes preferred
  |
5 | for x in a['values']:
  |            ^^^^^^^^ Q000
  |

Found 2 error(s) (1 fixed, 1 remaining).
```

```python
# $ cat main.py
a = {
    "values": {i:i for i in range(10)}
}

for x in a['values']:
    print(x)
```

---

_Comment by @charliermarsh on 2023-01-01 03:19_

Thanks, we’ll take a look at this. But probably not tonight :)

---

_Label `bug` added by @charliermarsh on 2023-01-01 03:20_

---

_Closed by @charliermarsh on 2023-01-01 17:53_

---

_Comment by @thomasdesr on 2023-01-01 21:42_

Thanks! :D

---

_Comment by @charliermarsh on 2023-01-01 21:52_

All credit to @saadmk11!

---
