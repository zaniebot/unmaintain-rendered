```yaml
number: 580
title: Add tests for repeated installs with source distributions
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/tests
created_at: 2023-12-06T19:58:07Z
updated_at: 2023-12-06T20:02:33Z
url: https://github.com/astral-sh/uv/pull/580
synced_at: 2026-01-12T16:04:02Z
```

# Add tests for repeated installs with source distributions

---

_@charliermarsh_

Adds a few more tests for re-installs with various kinds of source distributions, and changes the tests to use packages that we can safely import (via `check_command`) for extra validation.

Once we properly respect cached built wheels, we should expect these snapshots to change, since we'll no longer download and re-build unnecessarily.

---

_Label `internal` added by @charliermarsh on 2023-12-06 19:58_

---

_Merged by @charliermarsh on 2023-12-06 20:02_

---

_Closed by @charliermarsh on 2023-12-06 20:02_

---

_Branch deleted on 2023-12-06 20:02_

---
