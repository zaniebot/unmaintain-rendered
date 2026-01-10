```yaml
number: 10072
title: Add support for direct archive dependencies in Git
type: pull_request
state: open
author: charliermarsh
labels:
  - enhancement
assignees: []
base: main
head: charlie/add-git-path-deps
created_at: 2024-12-21T04:10:46Z
updated_at: 2025-10-14T06:13:08Z
url: https://github.com/astral-sh/uv/pull/10072
synced_at: 2026-01-10T06:36:15Z
```

# Add support for direct archive dependencies in Git

---

_Pull request opened by @charliermarsh on 2024-12-21 04:10_

## Summary

This PR adds support for sources that reference pre-built archives (wheels or source distributions) within Git repositories.


---

_Converted to draft by @charliermarsh on 2024-12-21 04:13_

---

_Marked ready for review by @charliermarsh on 2024-12-21 17:53_

---

_Label `enhancement` added by @charliermarsh on 2024-12-21 17:53_

---

_@charliermarsh reviewed on 2024-12-21 18:10_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:5532 on 2024-12-21 18:10_

This is the real motivating example: you have a project that you push to GitHub that includes a `path` source that points to a wheel, like:

```toml
[project]
name = "archive-in-git-test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = ["iniconfig"]

[tool.uv.sources]
iniconfig = { path = "archives/iniconfig-2.0.0-py3-none-any.whl" }
```

We have to translate that path source to a Git source so that we avoid writing cache paths to the lockfile.


---

_@charliermarsh reviewed on 2024-12-21 18:10_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/sync.rs`:5614 on 2024-12-21 18:10_

Sick

---

_Review requested from @konstin by @charliermarsh on 2025-01-11 14:51_

---

_Review comment by @konstin on `crates/uv-distribution/src/source/mod.rs`:1361 on 2025-01-13 10:08_

typo: create unzip

---

_@konstin approved on 2025-01-13 10:16_

---

_Comment by @xcorp on 2025-10-14 06:13_

Any plans on having a look at this again, I really would like a fix for https://github.com/astral-sh/uv/issues/15417

Thanks

---
