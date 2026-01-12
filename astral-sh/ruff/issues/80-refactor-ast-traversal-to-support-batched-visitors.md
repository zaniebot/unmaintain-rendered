```yaml
number: 80
title: Refactor AST traversal to support batched visitors
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2022-09-01T17:15:21Z
updated_at: 2022-10-11T17:54:07Z
url: https://github.com/astral-sh/ruff/issues/80
synced_at: 2026-01-12T15:54:40Z
```

# Refactor AST traversal to support batched visitors

---

_@charliermarsh_

Right now, we're able to get away with a single visitor implementation that implements all rules. We'll need to evolve this to something closer to Fixit's model: multiple visitors that deal with isolated concerns, and can be batched and run in a single traversal.

---

_Label `internal` added by @charliermarsh on 2022-09-01 17:19_

---

_Comment by @charliermarsh on 2022-09-04 16:00_

I want to tackle this sooner rather than later, the single visitor is getting too big.

---

_Comment by @charliermarsh on 2022-10-11 17:54_

Eh, we're more moving towards a plugin architecture, where plugins are called by the single visitor.

---

_Closed by @charliermarsh on 2022-10-11 17:54_

---
