```yaml
number: 5738
title: Remove double-proxy nodes in error reporting
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - error messages
assignees: []
merged: true
base: main
head: charlie/collapse
created_at: 2024-08-02T21:13:45Z
updated_at: 2024-08-05T14:03:55Z
url: https://github.com/astral-sh/uv/pull/5738
synced_at: 2026-01-12T16:06:59Z
```

# Remove double-proxy nodes in error reporting

---

_@charliermarsh_

## Summary

If _both_ nodes in the derivation tree are proxies, we need to remove the _entire_ node. So, the function now returns an `Option<Tree>` instead of taking `&mut Tree`.

Closes https://github.com/astral-sh/uv/issues/5618.


---

_Label `bug` added by @charliermarsh on 2024-08-02 21:13_

---

_Label `error messages` added by @charliermarsh on 2024-08-02 21:13_

---

_Marked ready for review by @charliermarsh on 2024-08-02 21:13_

---

_@charliermarsh reviewed on 2024-08-02 21:14_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:6806 on 2024-08-02 21:14_

This shouldn't be failing, but I think that's separate (@BurntSushi) -- the error message at least is much improved.

---

_Merged by @charliermarsh on 2024-08-02 21:27_

---

_Closed by @charliermarsh on 2024-08-02 21:27_

---

_Branch deleted on 2024-08-02 21:27_

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_compile.rs`:6806 on 2024-08-05 14:03_

Oh yes much better!

---

_@BurntSushi reviewed on 2024-08-05 14:03_

LGTM! Thanks!

---
