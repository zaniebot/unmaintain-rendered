```yaml
number: 12955
title: "Add `uv export` support for PEP 751"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/pep-751
created_at: 2025-04-18T02:30:32Z
updated_at: 2025-04-21T21:21:19Z
url: https://github.com/astral-sh/uv/pull/12955
synced_at: 2026-01-12T16:10:28Z
```

# Add `uv export` support for PEP 751

---

_@charliermarsh_

## Summary

This PR adds `uv export` support for [PEP 751](https://peps.python.org/pep-0751). We don't yet expose a way to consume the generated lockfile, but it's a first step.

The logic to go from `uv.lock` to "flat set of packages to include, with markers telling us when to include them" is all shared with the `requirements.txt` export (and extracted in https://github.com/astral-sh/uv/pull/12956). So most of the code is just converting from our internal types to the PEP 751 schema.


---

_Review requested from @Gankra by @charliermarsh on 2025-04-18 03:10_

---

_Review requested from @konstin by @charliermarsh on 2025-04-18 03:10_

---

_Label `enhancement` added by @charliermarsh on 2025-04-18 03:10_

---

_Marked ready for review by @charliermarsh on 2025-04-18 03:10_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:111 on 2025-04-21 15:51_

In the PEP, this is a toml datetime, but in our export, it is a toml string - probably because serde's type system doesn't have datatimes?

---

_Comment by @konstin on 2025-04-21 16:02_

What do you think about inferring the output format depending on the filename, so that `uv export -o pylock.toml` writes a PEP 751?

---

_Comment by @charliermarsh on 2025-04-21 16:03_

@konstin -- That's implemented in https://github.com/astral-sh/uv/pull/12958.

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:71 on 2025-04-21 16:04_

This is intentionally a stub, right?

---

_@konstin approved on 2025-04-21 16:35_

nice work!

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:71 on 2025-04-21 18:40_

Yeah. The PEP actually doesn't define a schema for this at all... You can put "whatever" you want in here.

---

_@charliermarsh reviewed on 2025-04-21 18:40_

---

_@charliermarsh reviewed on 2025-04-21 18:40_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:111 on 2025-04-21 18:40_

Oof, you're right, good catch. I'll... figure out how to model this.

---

_@charliermarsh reviewed on 2025-04-21 21:16_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:111 on 2025-04-21 21:16_

Fixed.

---

_Merged by @charliermarsh on 2025-04-21 21:21_

---

_Closed by @charliermarsh on 2025-04-21 21:21_

---

_Branch deleted on 2025-04-21 21:21_

---
