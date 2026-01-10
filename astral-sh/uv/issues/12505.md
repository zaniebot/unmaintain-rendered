```yaml
number: 12505
title: "Respect build constraints for `uv run --with` dependencies"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2025-03-27T02:24:45Z
updated_at: 2025-04-14T16:53:19Z
url: https://github.com/astral-sh/uv/issues/12505
synced_at: 2026-01-10T03:41:47Z
```

# Respect build constraints for `uv run --with` dependencies

---

_Issue opened by @charliermarsh on 2025-03-27 02:24_

Should these accept their own build constraints, or should they reuse the project's build constraints, or both?

---

_Label `enhancement` added by @charliermarsh on 2025-03-27 02:24_

---

_Comment by @zanieb on 2025-04-01 19:21_

I think they should use the project's? Adding a separate `--with-build-constraints` feels too complicated.

---

_Assigned to @Gankra by @Gankra on 2025-04-11 14:50_

---

_Closed by @charliermarsh on 2025-04-14 16:53_

---

_Closed by @charliermarsh on 2025-04-14 16:53_

---
