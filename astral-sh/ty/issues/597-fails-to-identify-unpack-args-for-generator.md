```yaml
number: 597
title: "Fails to identify & unpack args for generator"
type: issue
state: closed
author: Ryang20718
labels: []
assignees: []
created_at: 2025-06-07T00:09:25Z
updated_at: 2025-06-07T01:30:54Z
url: https://github.com/astral-sh/ty/issues/597
synced_at: 2026-01-10T02:34:10Z
```

# Fails to identify & unpack args for generator

---

_Issue opened by @Ryang20718 on 2025-06-07 00:09_

### Summary

```
import random
def _generate_color_wheel(h_range: int = 5, h_step: int = 35):
    keys = ["a", "b"]
    curr_h = 0
    for k in keys:
        yield (k, (curr_h, curr_h + h_range))
        curr_h += h_range + h_step

H_RANGES = dict(_generate_color_wheel())
h_range = H_RANGES["a"]
h = random.randint(*h_range)

```

### Version

0.0.1a8

---

_Closed by @carljm on 2025-06-07 01:30_

---
