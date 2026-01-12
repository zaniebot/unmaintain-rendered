```yaml
number: 1435
title: Support recursive extras
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/all
created_at: 2024-02-16T05:38:46Z
updated_at: 2024-02-16T16:42:05Z
url: https://github.com/astral-sh/uv/pull/1435
synced_at: 2026-01-12T16:04:37Z
```

# Support recursive extras

---

_@charliermarsh_

## Summary

We had a guard in the resolve to avoid "self-dependencies" (as in `gps3==0.33.3`), but this guard was _unintentionally_ filtering out recursive extras.

Closes https://github.com/astral-sh/uv/issues/1342.

## Test Plan

Taken from https://github.com/astral-sh/uv/pull/1352.


---

_Label `bug` added by @charliermarsh on 2024-02-16 05:38_

---

_Review requested from @zanieb by @charliermarsh on 2024-02-16 05:38_

---

_@charliermarsh reviewed on 2024-02-16 05:39_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/dependencies.rs`:25 on 2024-02-16 05:39_

I just renamed and re-ordered these (especially since `extra` got shadowed below).

---

_@zanieb approved on 2024-02-16 05:41_

Nice, that's the code I was suspicious of

---

_Comment by @zanieb on 2024-02-16 05:42_

Are these warnings only visible with `-v` — is that why we weren't seeing them?

---

_Comment by @charliermarsh on 2024-02-16 05:45_

Yeah

---

_@jankatins reviewed on 2024-02-16 10:49_

---

_Review comment by @jankatins on `crates/uv/tests/pip_install_scenarios.rs`:740 on 2024-02-16 10:49_

```suggestion
/// Multiple optional dependencies are requested for the package via an 'all' extra.
```

---

_@zanieb reviewed on 2024-02-16 14:28_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:740 on 2024-02-16 14:28_

Thanks! This file is actually generated. If you want to fix it, it's sourced from https://github.com/zanieb/packse/blob/main/scenarios/extras.json

---

_@jankatins reviewed on 2024-02-16 15:15_

---

_Review comment by @jankatins on `crates/uv/tests/pip_install_scenarios.rs`:740 on 2024-02-16 15:15_

https://github.com/zanieb/packse/pull/112


---

_Merged by @charliermarsh on 2024-02-16 16:42_

---

_Closed by @charliermarsh on 2024-02-16 16:42_

---

_Branch deleted on 2024-02-16 16:42_

---
