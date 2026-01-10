```yaml
number: 8628
title: Fork when minimum Python version increases
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - resolver
assignees: []
merged: true
base: main
head: charlie/fork
created_at: 2024-10-28T01:38:33Z
updated_at: 2024-10-28T13:48:07Z
url: https://github.com/astral-sh/uv/pull/8628
synced_at: 2026-01-10T12:54:14Z
```

# Fork when minimum Python version increases

---

_Pull request opened by @charliermarsh on 2024-10-28 01:38_

## Summary

This is a re-implementation of https://github.com/astral-sh/uv/pull/4712, though is now seemingly much simpler. This issue keeps coming up, and users have a workaround with `tool.uv.environments`, but it's really a bug in the resolver.

Closes https://github.com/astral-sh/uv/issues/4668.


---

_Review requested from @konstin by @charliermarsh on 2024-10-28 01:48_

---

_Label `bug` added by @charliermarsh on 2024-10-28 01:48_

---

_@charliermarsh reviewed on 2024-10-28 01:53_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:2842 on 2024-10-28 01:53_

This is just indented. The only change is the extra condition.

---

_@konstin approved on 2024-10-28 09:04_

---

_Label `resolver` added by @konstin on 2024-10-28 09:04_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-10-28 12:25_

---

_Review comment by @BurntSushi on `crates/uv/tests/it/pip_compile.rs`:7995 on 2024-10-28 13:29_

Nice!

---

_@BurntSushi approved on 2024-10-28 13:29_

Clever!

---

_@zanieb approved on 2024-10-28 13:40_

---

_Merged by @charliermarsh on 2024-10-28 13:48_

---

_Closed by @charliermarsh on 2024-10-28 13:48_

---

_Branch deleted on 2024-10-28 13:48_

---
