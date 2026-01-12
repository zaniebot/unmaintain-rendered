```yaml
number: 4295
title: Load configuration options from workspace root
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
  - preview
assignees: []
merged: true
base: main
head: charlie/configuration-ii
created_at: 2024-06-13T02:01:44Z
updated_at: 2024-06-14T01:26:22Z
url: https://github.com/astral-sh/uv/pull/4295
synced_at: 2026-01-12T16:06:08Z
```

# Load configuration options from workspace root

---

_@charliermarsh_

## Summary

In a workspace, we now read configuration from the workspace root. Previously, we read configuration from the first `pyproject.toml` or `uv.toml` file in path -- but in a workspace, that would often be the _project_ rather than the workspace configuration.

We need to read configuration from the workspace root, rather than its members, because we lock the workspace globally, so all configuration applies to the workspace globally.

As part of this change, the `uv-workspace` crate has been renamed to `uv-settings` and its purpose has been narrowed significantly (it no longer discovers a workspace; instead, it just reads the settings from a directory).

If a user has a `uv.toml` in their directory or in a parent directory but is _not_ in a workspace, we will still respect that use-case as before.

Closes #4249.


---

_@charliermarsh reviewed on 2024-06-13 02:03_

---

_Review comment by @charliermarsh on `crates/uv/src/main.rs`:126 on 2024-06-13 02:03_

Rules are explained here, in more detail.

---

_Comment by @charliermarsh on 2024-06-13 02:05_

It's a bit confusing that configuration in a workspace member will effectively be ignored. Something to revisit in the future, I think.

---

_Review requested from @zanieb by @charliermarsh on 2024-06-13 02:05_

---

_Review requested from @konstin by @charliermarsh on 2024-06-13 02:05_

---

_Label `configuration` added by @charliermarsh on 2024-06-13 02:06_

---

_Label `preview` added by @charliermarsh on 2024-06-13 02:06_

---

_Comment by @zanieb on 2024-06-13 13:51_

> It's a bit confusing that configuration in a workspace member will effectively be ignored. Something to revisit in the future, I think.

That seems pretty problematic :D

---

_@zanieb reviewed on 2024-06-13 13:52_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:3209 on 2024-06-13 13:52_

Is this behavior change intentional?

---

_@zanieb approved on 2024-06-13 13:52_

---

_@charliermarsh reviewed on 2024-06-13 13:53_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:3209 on 2024-06-13 13:53_

Yes, if you have an error in your `override-dependencies` we should fail. The previous behavior was wrong IMO.

---

_Comment by @charliermarsh on 2024-06-13 13:54_

I know it's a hard question, but what do you think the semantics should be? None of the options are enforceable on a per-package basis.

---

_@zanieb reviewed on 2024-06-13 15:34_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:3209 on 2024-06-13 15:34_

Makes sense to me

---

_Comment by @zanieb on 2024-06-13 15:38_

I'm not sure yet. I think I need to play with a real workspace setup first. I think we'd need to be able to map a tree of dependencies to their source project in the workspace to be able to enforce things, right? It seems like it would require a lot of changes.

---

_Merged by @charliermarsh on 2024-06-14 01:26_

---

_Closed by @charliermarsh on 2024-06-14 01:26_

---

_Branch deleted on 2024-06-14 01:26_

---
