```yaml
number: 1891
title: "refactor: Split CliSettings from Settings"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: cli-settings
created_at: 2023-01-15T10:11:39Z
updated_at: 2023-01-15T20:19:42Z
url: https://github.com/astral-sh/ruff/pull/1891
synced_at: 2026-01-12T05:36:32Z
```

# refactor: Split CliSettings from Settings

---

_Pull request opened by @not-my-profile on 2023-01-15 10:11_

_No description provided._

---

_@charliermarsh reviewed on 2023-01-15 18:55_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:15 on 2023-01-15 18:55_

Is this intended?

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:52 on 2023-01-15 18:56_

Should we impl `From<&Configuration>`?

---

_@charliermarsh reviewed on 2023-01-15 18:56_

---

_@charliermarsh reviewed on 2023-01-15 18:57_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:45 on 2023-01-15 18:57_

Idly wondering if this should be `LibSettings`, and `AllSettings` should be `Settings`.

---

_@charliermarsh reviewed on 2023-01-15 19:26_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:45 on 2023-01-15 19:26_

Eh, `AllSettings` or `CombinedSettings` is probably for the aggregate struct. Although I do wonder if `Settings` should be `LibSettings` or something.

---

_@not-my-profile reviewed on 2023-01-15 19:36_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:15 on 2023-01-15 19:36_

Yes the import is only used in the test-only `for_rule`(s) functions and omitting this attribute results in clippy complaining about an unused import. An alternative would be to move the import into the functions but I don't really see the benefit.

---

_@not-my-profile reviewed on 2023-01-15 19:39_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:52 on 2023-01-15 19:39_

We'd have to get rid of the additional `project_root` parameter ... I am not sure if that's desirable.

---

_@not-my-profile reviewed on 2023-01-15 19:41_

---

_Review comment by @not-my-profile on `src/settings/mod.rs`:45 on 2023-01-15 19:41_

I don't think `CliSettings` belongs in the `ruff` crate at all ... my plan is to move it to `ruff_cli` in the future. So I think it makes sense to keep it as `Settings` since these ought to be the only settings in the library crate.

If it turns out that we cannot move `CliSettings` I am alright with changing `Settings` to `LibSettings` but I'd like to try that factorisation first (at some point in the future after this PR has been merged).

---

_Merged by @charliermarsh on 2023-01-15 20:19_

---

_Closed by @charliermarsh on 2023-01-15 20:19_

---
