```yaml
number: 12057
title: "Formatter doesn't combine strings"
type: issue
state: closed
author: KotlinIsland
labels: []
assignees: []
created_at: 2024-06-26T23:46:33Z
updated_at: 2024-06-27T05:33:32Z
url: https://github.com/astral-sh/ruff/issues/12057
synced_at: 2026-01-10T11:09:54Z
```

# Formatter doesn't combine strings

---

_Issue opened by @KotlinIsland on 2024-06-26 23:46_

# before:
```py
    (
        ""
        ""
    )
```
# after:
```py
    ("" "")
```
# expected
```py
    ("")
    # OR
    ""
```

---

_Comment by @DetachHead on 2024-06-26 23:48_

related: #6936

---

_Comment by @dhruvmanila on 2024-06-27 05:33_

I'll merge this with #6936 

---

_Closed by @dhruvmanila on 2024-06-27 05:33_

---
