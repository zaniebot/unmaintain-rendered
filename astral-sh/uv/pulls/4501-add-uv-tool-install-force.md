```yaml
number: 4501
title: "Add `uv tool install --force`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/tool-install-force
created_at: 2024-06-25T00:37:40Z
updated_at: 2024-06-26T20:03:04Z
url: https://github.com/astral-sh/uv/pull/4501
synced_at: 2026-01-10T13:48:28Z
```

# Add `uv tool install --force`

---

_Pull request opened by @zanieb on 2024-06-25 00:37_

Adds detection of existing entry points, avoiding clobbering entry points that were installed by another tool. If we see any existing entry point collisions, we'll stop instead of overwriting them. The `--force` flag can be used to opt-in to overwriting the files; we can't use `-f` because it's taken by `--find-links` which is silly.  The `--force` flag also implies replacing a tool previously installed by uv (the environment is rebuilt).

Similarly, #4504 adds support for reinstalls that _will not_ clobber entry points managed by other tools.

---

_Label `preview` added by @zanieb on 2024-06-25 00:37_

---

_Marked ready for review by @zanieb on 2024-06-26 16:01_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-26 16:53_

---

_Review comment by @charliermarsh on `crates/uv-tool/src/lib.rs`:115 on 2024-06-26 18:40_

Nit: I don't think these typically end in a period

---

_@charliermarsh reviewed on 2024-06-26 18:44_

Given this, how would I upgrade a tool?

---

_Comment by @zanieb on 2024-06-26 18:55_

> Given this, how would I upgrade a tool?

We don't prefer anything about the existing environment right now. So `uv tool install <tool> --force` will upgrade, as will `uv tool install <tool> --reinstall`.

---

_@charliermarsh reviewed on 2024-06-26 19:05_

---

_Review comment by @charliermarsh on `crates/uv-tool/src/lib.rs`:111 on 2024-06-26 19:05_

I have a sense that we might need to lock more aggressively, like for the duration of the command (and on a per-tool basis or something).

---

_@charliermarsh reviewed on 2024-06-26 19:06_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/install.rs`:157 on 2024-06-26 19:06_

What happens if environment removal fails part-way through? The environment will be present but we won't have copied the entrypoints.

---

_@zanieb reviewed on 2024-06-26 19:10_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:111 on 2024-06-26 19:10_

Why do we want a longer lock?

#4560 adds locking per tool though.

---

_@zanieb reviewed on 2024-06-26 19:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/install.rs`:157 on 2024-06-26 19:13_

This is during failure due to existing entry points — we'd just fail to display a nice error message.

There's a todo during environment creation though that covers your concern a little more holistically:

```
    // TODO(zanieb): Build the environment in the cache directory then copy into the tool directory
    // This lets us confirm the environment is valid before removing an existing install
    let environment = installed_tools.create_environment(&name, interpreter)?;
```

---

_@zanieb reviewed on 2024-06-26 19:25_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:111 on 2024-06-26 19:25_

I guess if we lock over in the CLI then we can avoid modifying the executable directory concurrently.

---

_@zanieb reviewed on 2024-06-26 19:25_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:111 on 2024-06-26 19:25_

(I'd like to address this separately)

---

_@charliermarsh reviewed on 2024-06-26 19:36_

---

_Review comment by @charliermarsh on `crates/uv-tool/src/lib.rs`:111 on 2024-06-26 19:36_

There might be operations that happen after calling `remove_environment` that assume the tool no longer exists. Probably more common with `create_environment`: you probably assume later in the command that the environment exists. But if you're only locking when creating the environment, it could be removed immediately after. We might need to lock for the duration of the command to make it truly safe to run concurrently.

---

_@zanieb reviewed on 2024-06-26 19:37_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:111 on 2024-06-26 19:37_

Makes sense, thanks for explaining.

---

_@charliermarsh approved on 2024-06-26 19:37_

---

_Merged by @zanieb on 2024-06-26 20:03_

---

_Closed by @zanieb on 2024-06-26 20:03_

---

_Branch deleted on 2024-06-26 20:03_

---
