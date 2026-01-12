```yaml
number: 6990
title: "Add `--package` support to `uv build`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/build-workspace
created_at: 2024-09-04T01:18:50Z
updated_at: 2024-09-04T15:52:23Z
url: https://github.com/astral-sh/uv/pull/6990
synced_at: 2026-01-12T16:07:37Z
```

# Add `--package` support to `uv build`

---

_@charliermarsh_

## Summary

This PR adds `--package` support to `uv build`, such that you can use `--package` from anywhere in a workspace to build any member.

If a source directory is provided, we use that as the workspace root.

If a file is provided, we error.

For now, `uv build` only builds the current package, making it semantically identical to `uv sync`.

---

_Label `enhancement` added by @charliermarsh on 2024-09-04 01:18_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-04 01:19_

---

_Review requested from @konstin by @charliermarsh on 2024-09-04 01:19_

---

_@charliermarsh reviewed on 2024-09-04 01:19_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:1965 on 2024-09-04 01:19_

I'm not a huge fan of the interaction between `src` and `package` because I feel like it shows some warts between an API that's designed for projects vs. a general-purpose API for building an arbitrary directory.

---

_@konstin approved on 2024-09-04 10:00_

---

_@zanieb reviewed on 2024-09-04 14:30_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1965 on 2024-09-04 14:30_

I don't mind the interaction as described.

---

_@zanieb approved on 2024-09-04 14:31_

---

_Merged by @charliermarsh on 2024-09-04 15:52_

---

_Closed by @charliermarsh on 2024-09-04 15:52_

---

_Branch deleted on 2024-09-04 15:52_

---
