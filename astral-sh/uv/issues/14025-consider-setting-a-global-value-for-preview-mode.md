---
number: 14025
title: Consider setting a global value for preview mode
type: issue
state: open
author: jtfmumm
labels:
  - internal
  - needs-design
assignees: []
created_at: 2025-06-13T14:00:19Z
updated_at: 2025-06-13T14:00:19Z
url: https://github.com/astral-sh/uv/issues/14025
synced_at: 2026-01-10T01:25:41Z
---

# Consider setting a global value for preview mode

---

_Issue opened by @jtfmumm on 2025-06-13 14:00_

In order to put new features behind the `--preview` flag, we currently need to thread `PreviewMode` through numerous places in the codebase. The more complex the change, the more sprawling those changes can be, which could help discourage preview releases. 

Should we consider setting a global for preview mode? This would make putting new features into preview (and taking them out of preview) much easier. The downside is that in general it's better to avoid setting and relying on global values.

---

_Label `internal` added by @jtfmumm on 2025-06-13 14:00_

---

_Label `needs-design` added by @jtfmumm on 2025-06-13 14:00_

---
