```yaml
number: 390
title: Tuple Unpacking Not Working For Types
type: issue
state: closed
author: andrew-cho-architect
labels: []
assignees: []
created_at: 2025-05-14T17:11:32Z
updated_at: 2025-05-14T17:23:51Z
url: https://github.com/astral-sh/ty/issues/390
synced_at: 2026-01-10T02:34:09Z
```

# Tuple Unpacking Not Working For Types

---

_Issue opened by @andrew-cho-architect on 2025-05-14 17:11_

### Summary

For example:
`set.intersection(*sets)`

```
No argument provided for required parameter `self` of function `intersection`
```

That's not true because the first element of sets should be the `self` argument

### Version

_No response_

---

_Closed by @AlexWaygood on 2025-05-14 17:23_

---
