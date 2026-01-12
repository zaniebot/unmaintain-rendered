```yaml
number: 439
title: Implement N801 ~ N805
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: N801-805
created_at: 2022-10-16T09:43:06Z
updated_at: 2022-10-16T15:58:39Z
url: https://github.com/astral-sh/ruff/pull/439
synced_at: 2026-01-12T15:55:04Z
```

# Implement N801 ~ N805

---

_@harupy_

See #434.

---

_@harupy reviewed on 2022-10-16 10:02_

---

_Review comment by @harupy on `src/ast/checkers.rs`:1366 on 2022-10-16 10:02_

```suggestion
            CheckKind::InvalidArgumentName(name.to_string()),
```

---

_Review comment by @harupy on `src/snapshots/ruff__linter__tests__N803_N803.py.snap`:20 on 2022-10-16 10:04_

```suggestion
    row: 6
```

---

_@harupy reviewed on 2022-10-16 10:04_

---

_@harupy reviewed on 2022-10-16 10:05_

---

_Review comment by @harupy on `src/snapshots/ruff__linter__tests__N803_N803.py.snap`:17 on 2022-10-16 10:05_

```suggestion
    row: 6
```

---

_@harupy reviewed on 2022-10-16 10:05_

---

_Review comment by @harupy on `src/snapshots/ruff__linter__tests__N803_N803.py.snap`:6 on 2022-10-16 10:05_

```suggestion
    InvalidArgumentName: A
```

---

_@harupy reviewed on 2022-10-16 10:05_

---

_Review comment by @harupy on `src/snapshots/ruff__linter__tests__N803_N803.py.snap`:15 on 2022-10-16 10:05_

```suggestion
    InvalidArgumentName: A
```

---

_@harupy reviewed on 2022-10-16 10:06_

---

_Review comment by @harupy on `src/ast/checkers.rs`:1366 on 2022-10-16 10:06_

```suggestion
            CheckKind::InvalidArgumentName(name.to_string()),
```

---

_Comment by @charliermarsh on 2022-10-16 15:58_

Nice!

---

_Merged by @charliermarsh on 2022-10-16 15:58_

---

_Closed by @charliermarsh on 2022-10-16 15:58_

---
