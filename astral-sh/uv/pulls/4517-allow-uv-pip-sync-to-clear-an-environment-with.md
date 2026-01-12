```yaml
number: 4517
title: "Allow `uv pip sync` to clear an environment with opt-in"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/sync-allow-empty
created_at: 2024-06-25T14:36:44Z
updated_at: 2024-07-02T13:14:28Z
url: https://github.com/astral-sh/uv/pull/4517
synced_at: 2026-01-12T16:06:17Z
```

# Allow `uv pip sync` to clear an environment with opt-in

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/4516

Open to some deliberation about the opt-in strategy here.

---

_Label `cli` added by @zanieb on 2024-06-25 14:36_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-25 16:18_

---

_@charliermarsh reviewed on 2024-06-25 16:43_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:838 on 2024-06-25 16:43_

I would prefer `--allow-empty`, I don't think it's ambiguous.

I think this also needs to be added to the `PipOptions` and respected in persistent configuration, and it should _probably_ be accepted on `pip install` too? Though not sure what it would mean in that case so maybe not.

---

_@zanieb reviewed on 2024-06-25 17:07_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:838 on 2024-06-25 17:07_

If `pip install` exits with success when the requirements are empty I don't think it would mean anything there.

I can shorten it, though I find it a bit ambiguous.

---

_@zanieb reviewed on 2024-06-25 18:59_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:838 on 2024-06-25 18:59_

I guess it makes sense to include in the persistent configuration too? It only seems relevant for this command though.

---

_@charliermarsh reviewed on 2024-06-26 01:41_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:838 on 2024-06-26 01:41_

Yeah it should be in the persistent configuration -- that's the contract for the `pip` table (e.g., it includes things that only apply to `pip-compile`).

---

_@charliermarsh reviewed on 2024-06-26 01:42_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:838 on 2024-06-26 01:42_

I find `--allow-empty-requirements` to be more verbose than necessary but not a strong opinion, it's fine.

---

_Comment by @charliermarsh on 2024-07-02 02:20_

@zanieb - I'll add to config and merge.

---

_Comment by @zanieb on 2024-07-02 02:27_

@charliermarsh thanks!

---

_Closed by @zanieb on 2024-07-02 02:27_

---

_Reopened by @zanieb on 2024-07-02 02:28_

---

_Comment by @zanieb on 2024-07-02 02:28_

Sorry got too excited

---

_@charliermarsh approved on 2024-07-02 12:26_

---

_Merged by @charliermarsh on 2024-07-02 13:14_

---

_Closed by @charliermarsh on 2024-07-02 13:14_

---

_Branch deleted on 2024-07-02 13:14_

---
