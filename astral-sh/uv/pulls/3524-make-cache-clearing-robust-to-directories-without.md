```yaml
number: 3524
title: Make cache clearing robust to directories without read permissions
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/dir
created_at: 2024-05-11T16:52:21Z
updated_at: 2024-05-13T12:06:40Z
url: https://github.com/astral-sh/uv/pull/3524
synced_at: 2026-01-12T16:05:41Z
```

# Make cache clearing robust to directories without read permissions

---

_@charliermarsh_

## Summary

If you run the script included in the linked issue, then `uv cache clean`, we hit permissions errors on certain directories created by `setuptools`. The permissions on those directories look like:

```
❯ sudo ls -l /Users/crmarsh/Library/Caches/uv/built-wheels-v3/pypi/opentracing/2.4.0/M-fYsaHAaQQvedmPMUl9D/opentracing-2.4.0.tar.gz/build/bdist.macosx-14.2-arm64/wheel/opentracing
Password:
total 0
drwxr-xr-x  3 crmarsh  staff  96 May 11 12:51 harness
```

This PR adds logic to make those directories readable by the current user.

Closes https://github.com/astral-sh/uv/issues/3515.


---

_Label `bug` added by @charliermarsh on 2024-05-11 16:52_

---

_@charliermarsh reviewed on 2024-05-11 16:53_

---

_Review comment by @charliermarsh on `crates/uv-cache/src/removal.rs`:59 on 2024-05-11 16:53_

\cc @BurntSushi

---

_Merged by @charliermarsh on 2024-05-11 17:02_

---

_Closed by @charliermarsh on 2024-05-11 17:02_

---

_Branch deleted on 2024-05-11 17:02_

---

_Review comment by @BurntSushi on `crates/uv-cache/src/removal.rs`:59 on 2024-05-13 12:06_

This is interesting. As in like, the error will be reported again in the iterator?

---

_@BurntSushi reviewed on 2024-05-13 12:06_

---
