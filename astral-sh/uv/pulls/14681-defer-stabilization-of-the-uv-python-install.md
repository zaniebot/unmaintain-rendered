```yaml
number: 14681
title: "Defer stabilization of the `uv python install --default` behavior"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: zb/stable-bin
head: zb/no-stable-default
created_at: 2025-07-17T13:06:24Z
updated_at: 2025-07-17T13:29:07Z
url: https://github.com/astral-sh/uv/pull/14681
synced_at: 2026-01-10T06:53:02Z
```

# Defer stabilization of the `uv python install --default` behavior

---

_Pull request opened by @zanieb on 2025-07-17 13:06_

I'm a little worried about the semantics of this flag and the "default install". We can make it more accessible while stabilizing the rest of the behaviors, while deferring final stabilization of this to a future release. I left some TODOs in the code covering open questions for the feature.

This is stacked on #14626, which initially stabilized this feature.

---

_@zanieb reviewed on 2025-07-17 13:14_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:745 on 2025-07-17 13:14_

We _could_ stabilize the `is_default_install` behavior (i.e., `uv python install` gets you `python` / `python3` / `python3.13` while `uv python install 3.13` only gets you `python3.13`) separately from the `--default` flag. The default install behavior is really helpful for beginners, who expect `uv python install` to result in an available `python` executable, but I'm not sure how common it is for people to run it without a version request.

---

_Marked ready for review by @zanieb on 2025-07-17 13:15_

---

_@charliermarsh approved on 2025-07-17 13:21_

---

_@charliermarsh reviewed on 2025-07-17 13:21_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:175 on 2025-07-17 13:21_

I feel like this is supposed to end in a period since it's multiple sentences, but not sure if we do that consistently for these preview warnings.

---

_@zanieb reviewed on 2025-07-17 13:26_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:175 on 2025-07-17 13:26_

I don't think we do. I can do a pass for that everywhere separately.

---

_Merged by @zanieb on 2025-07-17 13:29_

---

_Closed by @zanieb on 2025-07-17 13:29_

---

_Branch deleted on 2025-07-17 13:29_

---
