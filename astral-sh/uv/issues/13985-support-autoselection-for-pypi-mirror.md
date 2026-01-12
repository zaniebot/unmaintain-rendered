```yaml
number: 13985
title: Support autoselection for PyPI mirror
type: issue
state: open
author: shell-nlp
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-06-12T09:25:41Z
updated_at: 2025-06-12T14:03:58Z
url: https://github.com/astral-sh/uv/issues/13985
synced_at: 2026-01-12T16:01:41Z
```

# Support autoselection for PyPI mirror

---

_@shell-nlp_

### Summary

Automatically select the best index-url based on network speed.

### Example

_No response_

---

_Label `enhancement` added by @shell-nlp on 2025-06-12 09:25_

---

_Comment by @konstin on 2025-06-12 09:28_

Can you extend what you mean? pypi uses a CDN, which already selects a server close to your location.

---

_Comment by @shell-nlp on 2025-06-12 10:36_

If multiple index-urls are configured (e.g., https://pypi.tuna.tsinghua.edu.cn/simple and https://pypi.org/simple/), test their network speeds and prioritize the fastest one.
@konstin 

---

_Comment by @shell-nlp on 2025-06-12 10:37_

> Can you extend what you mean? pypi uses a CDN, which already selects a server close to your location.

Sorry, I'm in English very bad

---

_Renamed from "Automatically select the optimal source according to the network speed of different pipy sources" to "Automatically select the best index-url based on network speed." by @shell-nlp on 2025-06-12 10:40_

---

_Comment by @zanieb on 2025-06-12 13:40_

I don't think we'll do this as described. We _could_ add support for arbitrary index _mirrors_ and select one of those based on speed, but it'd need to be a new concept.

---

_Renamed from "Automatically select the best index-url based on network speed." to "Support autoselection for PyPI mirror" by @konstin on 2025-06-12 14:03_

---

_Label `wish` added by @konstin on 2025-06-12 14:03_

---
