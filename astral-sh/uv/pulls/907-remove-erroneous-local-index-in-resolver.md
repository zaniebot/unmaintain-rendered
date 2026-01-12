```yaml
number: 907
title: "Remove erroneous local `Index` in resolver"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/local-index
created_at: 2024-01-13T03:53:01Z
updated_at: 2024-01-13T20:19:01Z
url: https://github.com/astral-sh/uv/pull/907
synced_at: 2026-01-12T16:04:17Z
```

# Remove erroneous local `Index` in resolver

---

_@charliermarsh_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2024-01-13 03:53_

---

_@charliermarsh reviewed on 2024-01-13 03:53_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:238 on 2024-01-13 03:53_

This seems like an oversight from a refactor? Confused. We should definitely be using `self.index` instead of this.

---

_@charliermarsh reviewed on 2024-01-13 03:54_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:380 on 2024-01-13 03:54_

This seems wrong... Because the request it kicks off eventually calls `self.index.distributions.done(package_id)`. So here we have a `register` without a `done`.

---

_Review requested from @konstin by @charliermarsh on 2024-01-13 03:54_

---

_@zanieb approved on 2024-01-13 04:41_

LGTM but I'm not a good reviewer for this.

---

_Merged by @charliermarsh on 2024-01-13 20:19_

---

_Closed by @charliermarsh on 2024-01-13 20:19_

---

_Branch deleted on 2024-01-13 20:19_

---
