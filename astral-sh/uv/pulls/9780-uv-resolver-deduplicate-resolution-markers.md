```yaml
number: 9780
title: "uv-resolver: deduplicate resolution markers"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
  - lock
assignees: []
merged: true
base: main
head: ag/dedupe-resolution-markers
created_at: 2024-12-10T18:13:40Z
updated_at: 2024-12-10T19:58:40Z
url: https://github.com/astral-sh/uv/pull/9780
synced_at: 2026-01-12T16:08:58Z
```

# uv-resolver: deduplicate resolution markers

---

_@BurntSushi_

Since we don't (currently) include conflict markers with our
`resolution-markers` in the lock file, it's possible that we end up
with duplicate markers. This happens when the resolver creates more
than one fork with the same PEP 508 markers but different conflict
markers, _and_ where those PEP 508 markers don't simplify to "always
true" after accounting for `requires-python`.

This change should be a strict improvement on the status quo. We aren't
removing any information. It is possible that we should be writing
conflict markers here (like we do for dependency edges), but I haven't
been able to come up with a case or think through a scenario where they
are necessary.

Fixes #9296


---

_Review requested from @charliermarsh by @BurntSushi on 2024-12-10 18:13_

---

_Review request for @charliermarsh removed by @BurntSushi on 2024-12-10 18:13_

---

_Review requested from @konstin by @BurntSushi on 2024-12-10 18:13_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-12-10 18:13_

---

_Label `bug` added by @BurntSushi on 2024-12-10 18:14_

---

_Label `lock` added by @BurntSushi on 2024-12-10 18:14_

---

_@charliermarsh approved on 2024-12-10 19:56_

---

_Merged by @BurntSushi on 2024-12-10 19:58_

---

_Closed by @BurntSushi on 2024-12-10 19:58_

---

_Branch deleted on 2024-12-10 19:58_

---
