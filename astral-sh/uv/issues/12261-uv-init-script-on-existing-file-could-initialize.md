```yaml
number: 12261
title: "`uv init --script` on existing file could initialize required imports..."
type: issue
state: open
author: patniemeyer
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-03-18T04:11:14Z
updated_at: 2025-03-18T12:47:12Z
url: https://github.com/astral-sh/uv/issues/12261
synced_at: 2026-01-12T16:00:58Z
```

# `uv init --script` on existing file could initialize required imports...

---

_@patniemeyer_

### Summary

It would be very convenient if invoking `uv init --script foo.py` on an existing `foo.py` would not only add the script comment but populate the dependencies with the apparent file imports.  

Thanks! Very much enjoying not having to make a dozen venvs a day now :) 

### Example

_No response_

---

_Label `enhancement` added by @patniemeyer on 2025-03-18 04:11_

---

_Comment by @zanieb on 2025-03-18 12:47_

Yeah this would be cool, we don't have import -> dependency mapping support yet. See also https://github.com/astral-sh/uv/issues/11650

---

_Label `wish` added by @zanieb on 2025-03-18 12:47_

---
