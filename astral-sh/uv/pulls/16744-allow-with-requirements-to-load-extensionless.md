```yaml
number: 16744
title: "Allow `--with-requirements` to load extensionless inline-metadata scripts"
type: pull_request
state: merged
author: terror
labels:
  - enhancement
assignees: []
merged: true
base: main
head: with-req-inline-metadata
created_at: 2025-11-15T02:41:03Z
updated_at: 2025-11-21T04:11:11Z
url: https://github.com/astral-sh/uv/pull/16744
synced_at: 2026-01-10T05:58:11Z
```

# Allow `--with-requirements` to load extensionless inline-metadata scripts

---

_Pull request opened by @terror on 2025-11-15 02:41_

Resolves https://github.com/astral-sh/uv/issues/16732

This diff treats extensionless files that contain [PEP 723](https://peps.python.org/pep-0723/) metadata as scripts when resolving `--with-requirements`, so inline metadata works even when the script doesn’t end in `.py`.

---

_Renamed from "Allow `--with-requirements` to load extensionless inline-metadata scr…" to "Allow `--with-requirements` to load extensionless inline-metadata scripts" by @terror on 2025-11-15 02:41_

---

_@konstin reviewed on 2025-11-19 11:39_

---

_Review comment by @konstin on `crates/uv-requirements/src/specification.rs`:199 on 2025-11-19 11:39_

Haven't checked the rest properly yet, but the `if err.kind() == std::io::ErrorKind::NotFound` branches should be handled by fs_err already.

---

_@charliermarsh approved on 2025-11-21 02:22_

---

_Label `enhancement` added by @charliermarsh on 2025-11-21 02:24_

---

_Merged by @charliermarsh on 2025-11-21 02:33_

---

_Closed by @charliermarsh on 2025-11-21 02:33_

---

_Comment by @samypr100 on 2025-11-21 04:02_

Seems like more tests needed an update as there are legit test failures happening in main now

---

_Comment by @charliermarsh on 2025-11-21 04:11_

Broke them with a late commit, though I swear I saw the Linux tests pass before I merged. I'll revert and fix tomorrow (https://github.com/astral-sh/uv/pull/16802).

---
