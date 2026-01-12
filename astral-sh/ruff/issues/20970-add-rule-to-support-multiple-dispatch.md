```yaml
number: 20970
title: Add rule to support multiple dispatch
type: issue
state: closed
author: ShalokShalom
labels: []
assignees: []
created_at: 2025-10-19T19:03:28Z
updated_at: 2025-10-20T13:47:45Z
url: https://github.com/astral-sh/ruff/issues/20970
synced_at: 2026-01-12T15:54:57Z
```

# Add rule to support multiple dispatch

---

_@ShalokShalom_

### Summary

The [plum-dispatch](https://github.com/beartype/plum#readme) library adds support for multiple dispatch. 

That means multiple definitions (methods) of a function, and dispatching on the arguments. 

This means, as of now, that every additional definition is flagged as a redefinition. 
That counts for both used as well as unused definitions. 

The @dispatch decorator could be used to differentiate. 

<img width="2880" height="1800" alt="Image" src="https://github.com/user-attachments/assets/48d59762-34a4-4e35-8c93-400f245b251d" />

---

_Comment by @ntBre on 2025-10-20 13:47_

Thanks for the report! This actually came up  previously in #11888, and I think the explanation there still stands. In short, this is tricky for static analysis tools to capture, and we prefer not to encode behavior specific to third-party packages into Ruff. So I think the best option would just be to use a `noqa` comment.

---

_Closed by @ntBre on 2025-10-20 13:47_

---
