```yaml
number: 8839
title: "Respect fork markers in `--resolution-mode=lowest-direct`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/fork
created_at: 2024-11-05T19:23:53Z
updated_at: 2024-11-06T13:46:56Z
url: https://github.com/astral-sh/uv/pull/8839
synced_at: 2026-01-10T12:00:00Z
```

# Respect fork markers in `--resolution-mode=lowest-direct`

---

_Pull request opened by @charliermarsh on 2024-11-05 19:23_

## Summary

Previously, given:

```toml
dependencies = [
    "pycountry >= 22.1.10",
    "setuptools >= 50.0.0; python_version>='3.12'"
]
```

We'd solve for the lowest version of setuptools (with _no_ lower-bound constraint) in the `python_version < '3.12'` complement.

Closes https://github.com/astral-sh/uv/issues/8819.


---

_Marked ready for review by @charliermarsh on 2024-11-05 19:23_

---

_Label `bug` added by @charliermarsh on 2024-11-05 19:23_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-11-05 19:24_

---

_@charliermarsh reviewed on 2024-11-05 19:27_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/pip_compile.rs`:12712 on 2024-11-05 19:27_

Oops, hold on, this should be 55.0.0.

---

_Merged by @charliermarsh on 2024-11-05 21:09_

---

_Closed by @charliermarsh on 2024-11-05 21:09_

---

_Branch deleted on 2024-11-05 21:09_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:382 on 2024-11-06 13:46_

Makes sense to me!

---

_@BurntSushi reviewed on 2024-11-06 13:46_

LGTM!

---
