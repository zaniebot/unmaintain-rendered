---
number: 6309
title: space efficiency
type: issue
state: closed
author: max397574
labels:
  - question
  - cache
assignees: []
created_at: 2024-08-21T07:51:54Z
updated_at: 2024-08-21T13:21:13Z
url: https://github.com/astral-sh/uv/issues/6309
synced_at: 2026-01-10T01:23:58Z
---

# space efficiency

---

_Issue opened by @max397574 on 2024-08-21 07:51_

Regarding https://docs.astral.sh/uv/concepts/projects/#project-environments I'm a bit worried about the space this will required.
Did you consider using links like e.g. https://github.com/pnpm/pnpm does it to be more space efficient than npm?


---

_Comment by @mitsuhiko on 2024-08-21 11:50_

IMO the better approach would be `clonefile` or similar options.

---

_Comment by @charliermarsh on 2024-08-21 13:21_

uv installs packages into a central cache and then links them into each relevant environment using Copy-on-Write (if available) or hardlinks -- so we're already doing something similar to pnpm in that regard, the space efficiency should be the same.

---

_Label `question` added by @charliermarsh on 2024-08-21 13:21_

---

_Label `cache` added by @charliermarsh on 2024-08-21 13:21_

---

_Closed by @charliermarsh on 2024-08-21 13:21_

---
