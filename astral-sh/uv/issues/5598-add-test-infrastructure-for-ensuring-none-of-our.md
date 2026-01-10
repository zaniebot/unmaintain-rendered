```yaml
number: 5598
title: add test infrastructure for ensuring none of our universal locks can produce a dependency tree with multiple versions of the same package for the same marker environment
type: issue
state: closed
author: BurntSushi
labels:
  - internal
  - lock
assignees: []
created_at: 2024-07-30T12:51:34Z
updated_at: 2024-09-24T14:55:25Z
url: https://github.com/astral-sh/uv/issues/5598
synced_at: 2026-01-10T04:45:09Z
```

# add test infrastructure for ensuring none of our universal locks can produce a dependency tree with multiple versions of the same package for the same marker environment

---

_Issue opened by @BurntSushi on 2024-07-30 12:51_

As witnessed in #5597, we have actual test outputs where it's possible to try to install two different versions of the same package for the same marker environment. We should add some kind of test or test helper that asserts that the locks we produce in tests do not suffer from this flaw.

Another interesting idea is to prevent these locks by construction. i.e., We could do this check after generating the lock but before writing it to disk. If it turns out we produced a resolution with more than one version of the same package, we could raise an error. This would be kind of a bummer though in general, because it would effectively be us trying to detect our own bugs. But it might lead to a better user experience.

---

_Label `internal` added by @BurntSushi on 2024-07-30 12:51_

---

_Label `lock` added by @BurntSushi on 2024-07-30 12:51_

---

_Assigned to @BurntSushi by @BurntSushi on 2024-07-30 13:24_

---

_Closed by @BurntSushi on 2024-09-24 14:55_

---

_Closed by @BurntSushi on 2024-09-24 14:55_

---
