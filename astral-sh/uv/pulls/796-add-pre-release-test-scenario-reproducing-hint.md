```yaml
number: 796
title: Add pre-release test scenario reproducing hint simplification bug
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/many-hint
created_at: 2024-01-05T16:38:46Z
updated_at: 2024-01-05T20:41:41Z
url: https://github.com/astral-sh/uv/pull/796
synced_at: 2026-01-10T15:44:44Z
```

# Add pre-release test scenario reproducing hint simplification bug

---

_Pull request opened by @zanieb on 2024-01-05 16:38_

A reproduction of #751 

Scenarios added in https://github.com/zanieb/packse/pull/68

---

_@zanieb reviewed on 2024-01-05 17:10_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:1079 on 2024-01-05 17:10_

I'm going to follow up with a pull request that hides these python requirements when not relevant

---

_Renamed from "Add "many" pre-release transitive test scenarios to verify hint simplification" to "Add pre-release test scenario reproducing hint simplification bug" by @zanieb on 2024-01-05 17:11_

---

_Marked ready for review by @zanieb on 2024-01-05 17:11_

---

_@zanieb reviewed on 2024-01-05 17:25_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:1270 on 2024-01-05 17:25_

Here we _do_ correctly simplify the exclusion of a5/a6/a7 into a5-a7 but the hole for b1 is not very nice

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_install_scenarios.rs`:1273 on 2024-01-05 17:25_

Despite the correct simplification of a5/a6/a7 above we do not do so here in the hint. Presumably, because this was the _actual_ request made.

---

_@zanieb reviewed on 2024-01-05 17:25_

---

_Comment by @zanieb on 2024-01-05 17:27_

Unfortunately, #751 will take some additional investigation because things look to be working roughly as intended here.

---

_Review requested from @charliermarsh by @zanieb on 2024-01-05 17:54_

---

_Comment by @zanieb on 2024-01-05 20:41_

Feel free to review, but I'm getting stacked up here.

---

_Merged by @zanieb on 2024-01-05 20:41_

---

_Closed by @zanieb on 2024-01-05 20:41_

---

_Branch deleted on 2024-01-05 20:41_

---
