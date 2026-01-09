---
number: 12267
title: FURB187 autofix can cause breaking changes 
type: issue
state: closed
author: owenlamont
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-07-10T06:04:54Z
updated_at: 2024-07-13T19:45:36Z
url: https://github.com/astral-sh/ruff/issues/12267
synced_at: 2026-01-07T13:12:15-06:00
---

# FURB187 autofix can cause breaking changes 

---

_Issue opened by @owenlamont on 2024-07-10 06:04_

# FURB187 autofix causes breaking changes when applied to function arguments

I have the FURB ruleset set to autofix but found the autofix for [FURB187 ](https://docs.astral.sh/ruff/rules/list-reverse-copy/) was breaking code when applied to arguments of functions since it was mutating objects outside of the function scope.

## Example Code

```python
def local_reverse_print_list(some_list: list[int]):
    some_list = list(reversed(some_list)) # FURB187 will fix to: some_list.reverse()
    print(f"Function shows list as: {some_list}")

a_list = [1,2,3]
local_reverse_print_list(a_list)
print(f"Original list: {a_list}") # Oops - the outside list is now mutated too after the FURB187 fix
```

## Ruff settings (relevant subset of actual settings)

```toml
[tool.ruff.lint]
# See https://docs.astral.sh/ruff/rules/
select = [
    "FURB",
]

# Allow fix for all enabled rules (when `--fix`) is provided.
fixable = ["ALL"]
unfixable = []
```

## Ruff version

0.5.1


---

_Label `bug` added by @MichaReiser on 2024-07-10 07:08_

---

_Label `fixes` added by @MichaReiser on 2024-07-10 07:08_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-12 12:51_

---

_Referenced in [astral-sh/ruff#12303](../../astral-sh/ruff/pulls/12303.md) on 2024-07-12 13:13_

---

_Closed by @charliermarsh on 2024-07-13 19:45_

---
