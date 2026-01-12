```yaml
number: 17097
title: Enforce minumum version of uv in a project
type: issue
state: closed
author: jaseemabid
labels:
  - question
assignees: []
created_at: 2025-12-12T11:03:27Z
updated_at: 2025-12-12T14:16:42Z
url: https://github.com/astral-sh/uv/issues/17097
synced_at: 2026-01-12T16:02:43Z
```

# Enforce minumum version of uv in a project

---

_@jaseemabid_

### Summary

Is there an easy way to enforce a minimum version of uv in a project? 

I want to make sure uv is new enough to know about the specific version of python pinned, so that it won't complain about a very recent python release. 

### Example

Avoids an error like this for other users of my project. 

```
error: No interpreter found for Python 3.13.11 in managed installations or search path
```

---

_Label `enhancement` added by @jaseemabid on 2025-12-12 11:03_

---

_Comment by @zanieb on 2025-12-12 13:43_

Yes

```
[tool.uv]
required-version = ">= 0.9.1"
````

---

_Label `enhancement` removed by @zanieb on 2025-12-12 13:43_

---

_Label `question` added by @zanieb on 2025-12-12 13:43_

---

_Comment by @jaseemabid on 2025-12-12 14:10_

Oh thank you! I couldn't find it in the docs at all and Claude gave up!

---

_Closed by @jaseemabid on 2025-12-12 14:16_

---
