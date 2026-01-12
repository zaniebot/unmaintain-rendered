```yaml
number: 17238
title: Sharing cache across different laptops
type: issue
state: open
author: lanbaoshen
labels:
  - question
assignees: []
created_at: 2025-12-26T06:29:48Z
updated_at: 2026-01-06T18:38:24Z
url: https://github.com/astral-sh/uv/issues/17238
synced_at: 2026-01-12T16:02:47Z
```

# Sharing cache across different laptops

---

_@lanbaoshen_

### Question

In my scenario, I have multiple Jenkins sub nodes running tests, and I currently use `uv` to install the Python environment on these child nodes.

Is there a solution to allow these sub nodes to share a cache?





### Platform

Ubuntu24.04

### Version

_No response_

---

_Label `question` added by @lanbaoshen on 2025-12-26 06:29_

---

_Comment by @linrl3 on 2025-12-27 13:43_

Probably no because uv serves as a cli tool in the host.

---

_Comment by @konstin on 2026-01-06 18:38_

The same consideration as in https://docs.astral.sh/uv/concepts/cache/#caching-in-continuous-integration should apply to jenkins too. Is there a specific caching problem beyond that guide?

---
