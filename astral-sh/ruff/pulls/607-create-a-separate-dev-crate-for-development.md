```yaml
number: 607
title: Create a separate dev crate for development scripts
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/dev
created_at: 2022-11-05T19:59:06Z
updated_at: 2022-11-05T19:59:26Z
url: https://github.com/astral-sh/ruff/pull/607
synced_at: 2026-01-12T15:55:05Z
```

# Create a separate dev crate for development scripts

---

_@charliermarsh_

Now, the usage looks like (from `./ruff_dev`):

```
cargo run generate-rules-table
cargo run generate-check-code-prefix
...
```

Resolves: #532.


---

_Merged by @charliermarsh on 2022-11-05 19:59_

---

_Closed by @charliermarsh on 2022-11-05 19:59_

---

_Branch deleted on 2022-11-05 19:59_

---

_Comment by @charliermarsh on 2022-11-05 19:59_

\cc @harupy - workflow changing a bit here.

---
