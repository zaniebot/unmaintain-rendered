```yaml
number: 11433
title: "Support `--active` for PEP 723 script environments"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - no-build
assignees: []
merged: true
base: main
head: charlie/sync-active
created_at: 2025-02-12T03:05:21Z
updated_at: 2025-02-13T19:40:23Z
url: https://github.com/astral-sh/uv/pull/11433
synced_at: 2026-01-10T11:10:38Z
```

# Support `--active` for PEP 723 script environments

---

_Pull request opened by @charliermarsh on 2025-02-12 03:05_

## Summary

See: https://github.com/astral-sh/uv/pull/11361#discussion_r1948851085


---

_Label `enhancement` added by @charliermarsh on 2025-02-12 03:05_

---

_Marked ready for review by @charliermarsh on 2025-02-12 03:05_

---

_Renamed from "Charlie/sync active" to "Support `--active` for PEP 723 script environments" by @charliermarsh on 2025-02-12 03:05_

---

_@zanieb reviewed on 2025-02-12 14:32_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3047 on 2025-02-12 14:32_

I might rephrase now? Like "Sync dependencies to the active virtual environment."

Then follow with "Instead of creating or updating the usual environment for the project or script, active virtual environment will be read from `VIRTUAL_ENV` and preferred."

---

_@zanieb reviewed on 2025-02-12 14:33_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:34 on 2025-02-12 14:33_

This is... interesting? Maybe this should be noted in the doc? Like "Unlike `is_same_file`, this will..."

I'm not sure I understand why we should do this extra check for directories but not files.

---

_@zanieb reviewed on 2025-02-12 14:34_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:34 on 2025-02-12 14:34_

It's more like.. `is_same_file_allow_missing`?

---

_@zanieb reviewed on 2025-02-12 14:35_

---

_Review comment by @zanieb on `crates/uv-fs/src/lib.rs`:34 on 2025-02-12 14:35_

(Shit I wrote this code)

If you're going to move it out like this, I think it's important to clarify what's going on. It was narrowly scoped before because it was only intended for that one use case.

---

_@zanieb reviewed on 2025-02-12 14:37_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/mod.rs`:604 on 2025-02-12 14:37_

Is this going to show on `uv run` too? I think that'd be surprising, for a script. Though it does seem useful for `sync`.

---

_@zanieb reviewed on 2025-02-12 14:38_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:3528 on 2025-02-12 14:38_

Can you add test coverage for `run` too?

---

_@charliermarsh reviewed on 2025-02-13 15:27_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:34 on 2025-02-13 15:27_

Sure.

---

_@charliermarsh reviewed on 2025-02-13 15:28_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/mod.rs`:604 on 2025-02-13 15:28_

Okay, I can scope it to `sync`. I'm a little uncomfortable with the inconsistency... but I get it.

---

_Label `no-build` added by @charliermarsh on 2025-02-13 17:25_

---

_@zanieb approved on 2025-02-13 19:40_

Thanks for addressing the feedback

---

_Merged by @zanieb on 2025-02-13 19:40_

---

_Closed by @zanieb on 2025-02-13 19:40_

---

_Branch deleted on 2025-02-13 19:40_

---
