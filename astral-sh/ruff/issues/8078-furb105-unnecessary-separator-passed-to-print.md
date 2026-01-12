```yaml
number: 8078
title: "FURB105 [*] Unnecessary separator passed to `print`"
type: issue
state: closed
author: SHEscher
labels:
  - bug
assignees: []
created_at: 2023-10-19T21:50:13Z
updated_at: 2023-10-19T22:30:14Z
url: https://github.com/astral-sh/ruff/issues/8078
synced_at: 2026-01-12T15:54:47Z
```

# FURB105 [*] Unnecessary separator passed to `print`

---

_@SHEscher_

Hi, there seems to be a mismatch: 

```python
# print each element of a list to a new line
print(*a_list_with_elements, sep="\n")
```

```shell
ruff . --select FURB105  --preview
# out: ... FURB105 [*] Unnecessary separator passed to `print`
```

However, FURB105 apparently checks for empty prints:
```shell
ruff rule FURB105
# out: Checks for `print` calls with an empty string as the only positional ...
```

Version `ruff 0.1.1`


---

_Comment by @charliermarsh on 2023-10-19 21:59_

This seems like a bug, since the separator _is_ necessary here.

---

_Label `bug` added by @charliermarsh on 2023-10-19 21:59_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-19 22:08_

---

_Comment by @SHEscher on 2023-10-19 22:12_

For 
```python 
print(*a_list_with_elements, sep=",")
```
it is the same behaviour.

Plus description of rule and application don't seem to match, since it is not an empty print()



---

_Comment by @charliermarsh on 2023-10-19 22:20_

Yeah the rule is slightly more expansive than the description suggests -- I've updated it. It also looks for `print` calls with one or fewer arguments that define a custom separator, since the separator is redundant in that case. I've also fixed the bug here related to starred arguments.

---

_Closed by @charliermarsh on 2023-10-19 22:30_

---
