```yaml
number: 507
title: Fix version in playground
type: issue
state: closed
author: MichaReiser
labels:
  - playground
assignees: []
created_at: 2025-05-25T11:30:14Z
updated_at: 2025-05-26T11:55:13Z
url: https://github.com/astral-sh/ty/issues/507
synced_at: 2026-01-10T02:34:10Z
```

# Fix version in playground

---

_Issue opened by @MichaReiser on 2025-05-25 11:30_

### Summary

The playground always shows `0.0.0`. 

It's not entirely clear to me how we would fix this because we only set the version in the ty repository but the playground is deployed from the ruff repository. 

For now, the easiest would be to remove the version.

### Version

_No response_

---

_Label `playground` added by @MichaReiser on 2025-05-25 11:30_

---

_Comment by @jelle-openai on 2025-05-25 13:31_

Maybe just add "last deployed N hours ago / at May 25, 2025 1:2:3 PM" so it's easy to verify that it's not out of date with your main branch.

---

_Comment by @MichaReiser on 2025-05-26 05:50_

That's a great idea. Or the commit hash. We probably want something else in the future (once we have more stable releases)

---

_Closed by @MichaReiser on 2025-05-26 11:55_

---
