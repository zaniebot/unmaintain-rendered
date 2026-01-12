```yaml
number: 15198
title: "Fix: update smoke-test get command failed count method"
type: pull_request
state: closed
author: zhangbo2012
labels: []
assignees: []
base: main
head: main
created_at: 2025-08-10T10:15:17Z
updated_at: 2025-08-11T02:31:57Z
url: https://github.com/astral-sh/uv/pull/15198
synced_at: 2026-01-12T16:11:37Z
```

# Fix: update smoke-test get command failed count method

---

_@zhangbo2012_

## Summary

in `scripts/smoke-test/__main__.py.main()`

In get failed count, We use sum to calc all return code
```python
failed = sum(result.returncode != 0 for result in results)
```
However, If retuncode is greater than 1, or a negative number, Sum will return a wrong result.

So update to len(), We can get the exact number of failures


## Test Plan

I test this update on MacOS, and get result

```
SUCCESS - 8/8 commands succeeded
```


---

_Comment by @zanieb on 2025-08-10 16:24_

I don't think this is true? We're summing the booleans.

---

_Comment by @zhangbo2012 on 2025-08-11 02:31_

You are right， I will close this PR

---

_Closed by @zhangbo2012 on 2025-08-11 02:31_

---
