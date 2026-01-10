```yaml
number: 11101
title: uv fails sometimes (not reproducable) to deserialize cache entries
type: issue
state: closed
author: cwiede
labels:
  - bug
assignees: []
created_at: 2025-01-30T16:17:38Z
updated_at: 2025-01-30T19:37:44Z
url: https://github.com/astral-sh/uv/issues/11101
synced_at: 2026-01-10T03:50:31Z
```

# uv fails sometimes (not reproducable) to deserialize cache entries

---

_Issue opened by @cwiede on 2025-01-30 16:17_

### Summary

Unfortunately I cannot reproduce this issue consistently at the moment, but in the last days I got errors

> Failed to deserialize cache entry
> array had incorrect length, expected 5

when trying to `uv pip install` a directory in editable mode. I can perform a `uv cache clean` as a workaround, but I wonder, if there is any option to make these kind of errors warnings such that the corresponding cache entries can be ignored (maybe it should even be the default behaviour)?

I switch between the uv/python versions as noted below.

### Platform

Ubuntu 20.04

### Version

0.5.18, 0.5.25

### Python version

3.10.14, 3.10.16, 3.9.21

---

_Label `bug` added by @cwiede on 2025-01-30 16:17_

---

_Comment by @zanieb on 2025-01-30 16:20_

Thanks for the report. 

I think this is a duplicate of https://github.com/astral-sh/uv/issues/11043

Not sure what is going on here yet.

---

_Closed by @cwiede on 2025-01-30 16:35_

---

_Closed by @charliermarsh on 2025-01-30 17:48_

---

_Comment by @cwiede on 2025-01-30 19:37_

Thank a lot both for the outstanding tool and outstanding support!

---
