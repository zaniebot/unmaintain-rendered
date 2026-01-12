```yaml
number: 17493
title: "[`flake8-errmsg`] Add fix safety section to docs (`EM101`, `EM102`, `EM103`) "
type: pull_request
state: open
author: Kalmaegi
labels:
  - documentation
assignees: []
base: main
head: doc_fix_safety_for_string_in_exception
created_at: 2025-04-20T11:42:06Z
updated_at: 2025-04-22T19:34:17Z
url: https://github.com/astral-sh/ruff/pull/17493
synced_at: 2026-01-12T15:56:02Z
```

# [`flake8-errmsg`] Add fix safety section to docs (`EM101`, `EM102`, `EM103`) 

---

_@Kalmaegi_

## Summary

add `fix safety` section to `EM101`, `EM102`, `EM103` in `string_in_exception.rs` for #15584 

This rule is only executed when the parameter is a character; if it's something else like a function, it will be skipped. I'm not sure if this point needs to be mentioned.

This rule modifies the original character content, such as inserting new variables, etc. If there are comments, they will remain in their original position. In the case of a single line, it doesn't seem problematic, but for multiple lines, the comments may end up in incorrect positions.

Before:

```python
raise RuntimeError("string") #comment 1

raise RuntimeError(
    "multi-line " # comment 1
    "string" # comment 2
) # comment 3


raise RuntimeError(
    f"{42} is "  # f-string comment1
    f"wrong"     # f-string comment2
)  # f-string comment3


raise RuntimeError(
    "value is {}".format(  # format comment1
        42                 # format comment2
    )
)  # format comment3


def side_effect():
    print("side effect!")
    return "danger"

raise RuntimeError(side_effect())
```


After:

```python
msg = "string"
raise RuntimeError(msg) #comment 1

msg = (
    "multi-line " # comment 1
    "string"
)
raise RuntimeError(
    msg # comment 2
) # comment 3


msg = (
    f"{42} is "  # f-string comment1
    f"wrong"
)
raise RuntimeError(
    msg     # f-string comment2
)  # f-string comment3


msg = (
    "value is {}".format(  # format comment1
        42                 # format comment2
    )
)
raise RuntimeError(
    msg
)  # format comment3


def side_effect():
    print("side effect!")
    return "danger"

raise RuntimeError(side_effect())
```


---

_Comment by @github-actions[bot] on 2025-04-20 11:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @VascoSch92 on 2025-04-20 12:10_

What if the script is already containing a `msg` variable? Then with the fix you are overridden the original `msg` variable and probably changing the program behaviour. 

---

_Comment by @Kalmaegi on 2025-04-20 13:37_

> What if the script is already containing a `msg` variable? Then with the fix you are overridden the original `msg` variable and probably changing the program behaviour.如果脚本中已经包含 `msg` 变量怎么办？修复后，原始的 `msg` 变量将被覆盖，并可能改变程序的行为。

Indeed, I didn't notice the issue with inserting the variable name!

> What if the script is already containing a `msg` variable? Then with the fix you are overridden the original `msg` variable and probably changing the program behaviour.

Indeed, I didn't notice the issue with inserting the variable name!

---

_Renamed from "[flake8-errmsg] Add fix safety section to docs (EM101, EM102, EM103) " to "[`flake8-errmsg`] Add fix safety section to docs (`EM101`, `EM102`, `EM103`) " by @Kalmaegi on 2025-04-20 14:58_

---

_Label `documentation` added by @dhruvmanila on 2025-04-21 15:10_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-04-22 19:34_

---
