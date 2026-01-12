```yaml
number: 12402
title: "`loop-iterator-mutation` (`B909`) - logic to determine whether the mutation is followed by a `break` statement is incorrect"
type: issue
state: open
author: DetachHead
labels:
  - bug
  - rule
assignees: []
created_at: 2024-07-19T05:17:33Z
updated_at: 2024-07-19T06:00:16Z
url: https://github.com/astral-sh/ruff/issues/12402
synced_at: 2026-01-12T15:54:51Z
```

# `loop-iterator-mutation` (`B909`) - logic to determine whether the mutation is followed by a `break` statement is incorrect

---

_@DetachHead_

the rule seems to check whether or not the loop has a `break` after the mutation, because that makes the mutation safe:

```py
items = [1, 2, 3]

for item in items:
    items.append(item) # no error
    break
```

however i've noticed two issues with this logic:

1. it doesn't seem to account for when there's another statement between the mutation and the `break`:
   
    ```py
    items = [1, 2, 3]
    
    for item in items:
        items.append(item)
        if True:
            pass
        break # error
    ```
    this should still be considered safe, because the statement does not effect the flow of the loop at all.

2. if there's a `continue` statement followed by a `break` statement, it does not report an error, even though the mutation is unsafe:
  
    ```py
    items = [1, 2, 3]
    
    for item in items:
        items.remove(item)
        continue
        break
    ```

[playground](https://play.ruff.rs/27b883ac-e6f2-4235-990d-30152e30a668)

---

_Label `bug` added by @MichaReiser on 2024-07-19 06:00_

---

_Label `rule` added by @MichaReiser on 2024-07-19 06:00_

---
