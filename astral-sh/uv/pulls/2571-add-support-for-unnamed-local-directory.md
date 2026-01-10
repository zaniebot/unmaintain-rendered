```yaml
number: 2571
title: Add support for unnamed local directory requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/bare-iii
created_at: 2024-03-20T19:39:08Z
updated_at: 2024-03-21T03:46:12Z
url: https://github.com/astral-sh/uv/pull/2571
synced_at: 2026-01-10T14:49:08Z
```

# Add support for unnamed local directory requirements

---

_Pull request opened by @charliermarsh on 2024-03-20 19:39_

## Summary

For example: `cargo run pip install .`

The strategy taken here is to attempt to extract the package name from the distribution without executing the PEP 517 build steps. We could choose to do that in the future if this proves lacking, but it adds complexity.

Part of: https://github.com/astral-sh/uv/issues/313.


---

_@charliermarsh reviewed on 2024-03-20 20:17_

---

_Review comment by @charliermarsh on `crates/uv/src/requirements.rs`:688 on 2024-03-20 20:17_

\cc @BurntSushi - Here I want to match `setup(name="foo")`-like calls.

---

_Review requested from @konstin by @charliermarsh on 2024-03-20 23:39_

---

_Label `enhancement` added by @charliermarsh on 2024-03-20 23:39_

---

_Marked ready for review by @charliermarsh on 2024-03-20 23:39_

---

_@gotmax23 reviewed on 2024-03-21 00:15_

---

_Review comment by @gotmax23 on `crates/uv/src/requirements.rs`:688 on 2024-03-21 00:15_

I do not think frontends are supposed to use build backend–specific heuristics to retrieve metadata.

---

_@charliermarsh reviewed on 2024-03-21 00:23_

---

_Review comment by @charliermarsh on `crates/uv/src/requirements.rs`:688 on 2024-03-21 00:23_

I'm fine with it. These are just heuristics to support non-spec compliant requirement definitions.

---

_Review comment by @gotmax23 on `crates/uv/src/requirements.rs`:688 on 2024-03-21 00:31_

Why not run `prepare_metadata_for_build_wheel()` and use the metadata from there like pip does?

---

_@gotmax23 reviewed on 2024-03-21 00:31_

---

_@charliermarsh reviewed on 2024-03-21 00:33_

---

_Review comment by @charliermarsh on `crates/uv/src/requirements.rs`:688 on 2024-03-21 00:33_

I don't want to pay that cost here when all we need is the project _name_ (which is far less to ask than the project _metadata_). I'll consider it though.

---

_@charliermarsh reviewed on 2024-03-21 00:37_

---

_Review comment by @charliermarsh on `crates/uv/src/requirements.rs`:688 on 2024-03-21 00:37_

I'll look into how challenging it would be.

---

_@gotmax23 reviewed on 2024-03-21 00:43_

---

_Review comment by @gotmax23 on `crates/uv/src/requirements.rs`:688 on 2024-03-21 00:43_

The project name is part of the Core metadata spec just like `Requires-Dist` or anything else, but I understand. Thanks for looking into it.

---

_@gotmax23 reviewed on 2024-03-21 00:49_

---

_Review comment by @gotmax23 on `crates/uv/src/requirements.rs`:688 on 2024-03-21 00:49_

Also, it's _possible_ for a build backend to exist that isn't setuptools or poetry and doesn't use PEP 621 metadata.

---

_@zanieb approved on 2024-03-21 02:01_

---

_Merged by @charliermarsh on 2024-03-21 03:46_

---

_Closed by @charliermarsh on 2024-03-21 03:46_

---

_Branch deleted on 2024-03-21 03:46_

---
