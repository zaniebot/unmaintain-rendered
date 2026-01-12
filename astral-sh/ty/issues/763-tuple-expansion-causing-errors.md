```yaml
number: 763
title: Tuple expansion causing errors
type: issue
state: closed
author: Ryang20718
labels: []
assignees: []
created_at: 2025-07-04T01:50:34Z
updated_at: 2025-07-04T02:06:20Z
url: https://github.com/astral-sh/ty/issues/763
synced_at: 2026-01-12T15:54:23Z
```

# Tuple expansion causing errors

---

_@Ryang20718_

### Summary



 No arguments provided for required parameters `a`, `b` of bound method `randint`

repro
```
import random
ran= [1,4]
print("GG")
print(random.randint(*ran))
```


### Version

ty==0.0.1a13

---

_Comment by @carljm on 2025-07-04 02:06_

Thanks for the report! This is #247 -- and there's already a PR up to add this support!

---

_Closed by @carljm on 2025-07-04 02:06_

---
