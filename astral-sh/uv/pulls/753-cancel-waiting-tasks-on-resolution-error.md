```yaml
number: 753
title: Cancel waiting tasks on resolution error
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/cancel
created_at: 2024-01-03T19:23:31Z
updated_at: 2024-01-03T20:18:28Z
url: https://github.com/astral-sh/uv/pull/753
synced_at: 2026-01-10T15:44:44Z
```

# Cancel waiting tasks on resolution error

---

_Pull request opened by @charliermarsh on 2024-01-03 19:23_

## Summary

I don't understand why this works (because I don't understand why it's erroring) but it does. See: https://github.com/astral-sh/puffin/pull/746#issuecomment-1875722454.

## Test Plan

```
cargo run --bin puffin pip-install requires-transitive-incompatible-with-transitive-8329cfc0 --extra-index-url https://test.pypi.org/simple -n
```

---

_Review requested from @zanieb by @charliermarsh on 2024-01-03 19:23_

---

_Review requested from @konstin by @charliermarsh on 2024-01-03 19:23_

---

_Label `bug` added by @charliermarsh on 2024-01-03 19:23_

---

_@konstin approved on 2024-01-03 19:38_

We have some unwraps that depend on us never cancelling, so we must take care in the future that cancel only happens after that; I think we should add some warning comments.

---

_Comment by @charliermarsh on 2024-01-03 19:40_

Can you point me to one of those?

---

_@zanieb reviewed on 2024-01-03 20:01_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/resolver/mod.rs`:207 on 2024-01-03 20:01_

Might be worth a comment here

---

_Merged by @charliermarsh on 2024-01-03 20:18_

---

_Closed by @charliermarsh on 2024-01-03 20:18_

---

_Branch deleted on 2024-01-03 20:18_

---
