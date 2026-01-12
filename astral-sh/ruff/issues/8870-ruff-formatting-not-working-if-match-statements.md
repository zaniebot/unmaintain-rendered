```yaml
number: 8870
title: "ruff formatting not working if `match` statements exists anywhere (vscode; vscode extension; ipynb)"
type: issue
state: closed
author: Julian-J-S
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-11-28T16:02:22Z
updated_at: 2023-11-28T20:38:27Z
url: https://github.com/astral-sh/ruff/issues/8870
synced_at: 2026-01-12T15:54:48Z
```

# ruff formatting not working if `match` statements exists anywhere (vscode; vscode extension; ipynb)

---

_@Julian-J-S_

# Problem

ruff formatting does nothing in a vscode notebook (`.ipunb`) for specific code statements.
Seems to be related to `match/case` statements

# Example

![image](https://github.com/astral-sh/ruff/assets/25177421/56bfc147-090b-48b1-ba51-654a91a13fd7)

The command `Ruff: Format document` does NOTHING with this setup.
Once I comment out the `match` cell it will again format everything correctly.

![image](https://github.com/astral-sh/ruff/assets/25177421/486a95d8-bef6-4bbf-8705-ff6e22380f24)

code (NOTE: this works in a python file but not in ipynb files): 
```python
x = ['a'   ,     'b', 'c'      ,]

text = input()

match text:
    case 'hello': print('HI')
    case 'world': print('hey')
    case other: print(other)

x2 = ['a'   ,     'b', 'c'      ,]

```

# Version
vscode Ruff extension `v2023.50.0`

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Comment by @zanieb on 2023-11-28 18:31_

Thanks for the report! cc @dhruvmanila 

---

_Label `bug` added by @zanieb on 2023-11-28 18:31_

---

_Label `formatter` added by @zanieb on 2023-11-28 18:31_

---

_Comment by @dhruvmanila on 2023-11-28 20:04_

Thanks, looking into it now...

---

_Comment by @dhruvmanila on 2023-11-28 20:20_

Ok, found the issue, pushing a fix for it. Thanks again for highlighting this!

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-11-28 20:20_

---

_Closed by @dhruvmanila on 2023-11-28 20:38_

---
