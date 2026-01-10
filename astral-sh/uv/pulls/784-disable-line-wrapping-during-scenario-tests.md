```yaml
number: 784
title: Disable line wrapping during scenario tests
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/disable-line-wrap
created_at: 2024-01-04T16:55:41Z
updated_at: 2024-01-04T19:07:18Z
url: https://github.com/astral-sh/uv/pull/784
synced_at: 2026-01-10T15:44:44Z
```

# Disable line wrapping during scenario tests

---

_Pull request opened by @zanieb on 2024-01-04 16:55_

Adds support for a `PUFFIN_NO_WRAP` environment variable which disables line wrapping in `miette` output.

We set this variable in the scenario tests to improve the readability of snapshots.

I contributed the ability to disable line wrapping upstream at https://github.com/zkat/miette/pull/328


---

_Review comment by @zanieb on `Cargo.toml`:50 on 2024-01-04 17:01_

See https://github.com/zkat/miette/pull/328

---

_@zanieb reviewed on 2024-01-04 17:01_

---

_Marked ready for review by @zanieb on 2024-01-04 17:19_

---

_@charliermarsh reviewed on 2024-01-04 18:17_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_install_scenarios.rs`:98 on 2024-01-04 18:17_

I know it sounds difficult, but... is there any way we could instead strip the prefix when we generate the error message, so that it's wrapped appropriately to start? Right now, we it feels like we're applying one fixup on top of another rather than solving the problem at the source.

---

_@zanieb reviewed on 2024-01-04 18:52_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:98 on 2024-01-04 18:52_

I don't think that's worth the complexity. It'd definitely be much more of a hack.

I think disabling the line wrapping during tests is desirable separately from the long package names — the line wrapping is generally confusing at 80 characters when reading our error messages in snapshots. Perhaps in the long run, we should focus on improving that formatting such that wrapped messages are easier to read. I think this is the simplest path forward for now and I think I'd want to be able to disable line wrapping in output as a user anyway.

---

_Comment by @zanieb on 2024-01-04 18:54_

Note the pull request was already accepted / merged upstream.

---

_@charliermarsh approved on 2024-01-04 19:00_

---

_Merged by @zanieb on 2024-01-04 19:07_

---

_Closed by @zanieb on 2024-01-04 19:07_

---

_Branch deleted on 2024-01-04 19:07_

---
