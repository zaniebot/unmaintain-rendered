```yaml
number: 15197
title: "Fix: Update smoke test failed cmd count method"
type: pull_request
state: closed
author: zhangbo2012
labels: []
assignees: []
draft: true
base: main
head: main
created_at: 2025-08-10T09:52:50Z
updated_at: 2025-08-10T10:09:37Z
url: https://github.com/astral-sh/uv/pull/15197
synced_at: 2026-01-10T06:44:33Z
```

# Fix: Update smoke test failed cmd count method

---

_Pull request opened by @zhangbo2012 on 2025-08-10 09:52_

## Summary

in `scripts/smoke-test/__main__.py.main()`

 In get failed count, We use sum to calc all return code 

```python
failed = sum(result.returncode != 0 for result in results)
```

However, If retuncode is greater than 1, or a negative number, Sum will return a wrong result.

So update to len(),  We can get the exact number of failures


## Test Plan

nothing

---

_Converted to draft by @zhangbo2012 on 2025-08-10 09:58_

---

_Closed by @zhangbo2012 on 2025-08-10 10:09_

---
