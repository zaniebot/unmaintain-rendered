```yaml
number: 4111
title: Avoid showing dev hints for Python requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - error messages
assignees: []
merged: true
base: main
head: charlie/dev-hint
created_at: 2024-06-06T19:46:49Z
updated_at: 2024-06-06T19:58:31Z
url: https://github.com/astral-sh/uv/pull/4111
synced_at: 2026-01-12T16:06:02Z
```

# Avoid showing dev hints for Python requirements

---

_@charliermarsh_

Closes https://github.com/astral-sh/uv/issues/4096.

---

_Label `bug` added by @charliermarsh on 2024-06-06 19:46_

---

_Label `error messages` added by @charliermarsh on 2024-06-06 19:46_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:415 on 2024-06-06 19:51_

The whole body is now just indented under `if let PubGrubPackageInner::Package { name, .. } = &**package`. Unfortunately we can't do this in the `match` itself because of the inner struct.

---

_@charliermarsh reviewed on 2024-06-06 19:51_

---

_@charliermarsh reviewed on 2024-06-06 19:51_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/report.rs`:416 on 2024-06-06 19:51_

Not really necessary anymore, we just inline `selector.prerelease_strategy().allows(name)` because we have the name already.

---

_Merged by @charliermarsh on 2024-06-06 19:58_

---

_Closed by @charliermarsh on 2024-06-06 19:58_

---

_Branch deleted on 2024-06-06 19:58_

---
