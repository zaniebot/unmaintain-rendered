```yaml
number: 524
title: "narrow within `sys.version_info` conditional"
type: issue
state: closed
author: metaist
labels: []
assignees: []
created_at: 2025-05-27T15:57:44Z
updated_at: 2025-05-27T16:02:00Z
url: https://github.com/astral-sh/ty/issues/524
synced_at: 2026-01-12T15:54:23Z
```

# narrow within `sys.version_info` conditional

---

_@metaist_

### Summary

Most python type checkers narrow the features they check for in conditional blocks that check `version_info`. For example, even if the `ty` python version is set to 3.9, it should not error on this block because the code is explicitly set to only work in versions of python >= 3.10:

https://play.ty.dev/8fc43541-b948-4150-a154-56d433aa1442
```python
import sys
if sys.version_info >= (3, 10):
    x = str | int
```

This is a fairly common pattern for supporting new language features as they get introduced without clobbering support for older python versions.

### Version

8d5655a7b

---

_Renamed from "narrow within sys.version_info conditional" to "narrow within `sys.version_info` conditional" by @metaist on 2025-05-27 15:58_

---

_Comment by @AlexWaygood on 2025-05-27 16:01_

Thank you! Yeah, we know we need to change this :-)

---

_Closed by @AlexWaygood on 2025-05-27 16:01_

---
