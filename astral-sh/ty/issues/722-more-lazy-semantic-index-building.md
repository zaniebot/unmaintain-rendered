```yaml
number: 722
title: More lazy semantic index building
type: issue
state: open
author: MichaReiser
labels:
  - performance
  - memory
assignees: []
created_at: 2025-06-28T20:35:31Z
updated_at: 2025-06-30T19:55:49Z
url: https://github.com/astral-sh/ty/issues/722
synced_at: 2026-01-10T02:07:36Z
```

# More lazy semantic index building

---

_Issue opened by @MichaReiser on 2025-06-28 20:35_

https://github.com/astral-sh/ruff/pull/19019 shows that we spent a decent amount of time in semantic indexing. I wonder if it would help performance if we make the semantic index more lazy by building it scope by scope instead of all in a single pass. 

This could also help for third party modules where it may not be necessary to index the entire file to answer the necessary type requests. 

---

_Label `performance` added by @MichaReiser on 2025-06-28 20:35_

---

_Label `memory` added by @MichaReiser on 2025-06-28 20:35_

---

_Comment by @carljm on 2025-06-30 19:55_

Yes, I think we had this as a potential area of exploration on the roadmap for a long time now. I think the only real downside I can think of is more queries, but I think proportionally the increase shouldn't be too great, definitions (for example) greatly outnumber scopes.

---
