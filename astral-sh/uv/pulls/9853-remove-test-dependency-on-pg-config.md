```yaml
number: 9853
title: "Remove test dependency on `pg_config`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/pg_config
created_at: 2024-12-12T21:54:45Z
updated_at: 2024-12-13T12:42:29Z
url: https://github.com/astral-sh/uv/pull/9853
synced_at: 2026-01-10T12:00:01Z
```

# Remove test dependency on `pg_config`

---

_Pull request opened by @konstin on 2024-12-12 21:54_

By mocking the metadata of `psycopg-c`, we avoid a test dependency on `pg_config` for the warehouse ecosystem test.

---

_Label `internal` added by @konstin on 2024-12-12 21:54_

---

_Review requested from @BurntSushi by @konstin on 2024-12-12 21:54_

---

_@zanieb approved on 2024-12-12 22:01_

uh yes

---

_Merged by @konstin on 2024-12-13 11:45_

---

_Closed by @konstin on 2024-12-13 11:45_

---

_Branch deleted on 2024-12-13 11:45_

---

_@BurntSushi reviewed on 2024-12-13 12:42_

---

_Review comment by @BurntSushi on `ecosystem/warehouse/pyproject.toml`:160 on 2024-12-13 12:42_

This is really neat.

---
