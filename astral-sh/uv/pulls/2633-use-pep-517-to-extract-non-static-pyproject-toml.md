```yaml
number: 2633
title: "Use PEP 517 to extract non-static `pyproject.toml` metadata"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/setup-ii
created_at: 2024-03-23T02:04:13Z
updated_at: 2025-04-01T02:23:45Z
url: https://github.com/astral-sh/uv/pull/2633
synced_at: 2026-01-10T11:10:33Z
```

# Use PEP 517 to extract non-static `pyproject.toml` metadata

---

_Pull request opened by @charliermarsh on 2024-03-23 02:04_

## Summary

When a user passes a `pyproject.toml` to `pip compile` (e.g., `uv pip compile pyproject.toml`), we extract the requirements from the `pyproject.toml` directly. However... that isn't always possible (as seen in the linked issues). When it's _not_, we instead need to run the PEP 517 build hooks to identify the metadata.

Closes https://github.com/astral-sh/uv/issues/1624.

Closes https://github.com/astral-sh/uv/issues/1644.

## Test Plan

`cargo test`


---

_Marked ready for review by @charliermarsh on 2024-03-23 02:04_

---

_Label `enhancement` added by @charliermarsh on 2024-03-23 02:04_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-23 02:40_

---

_Comment by @zanieb on 2024-03-24 23:34_

Sorry I'm a little lost in all the changes lately, just want to confirm does this address https://github.com/astral-sh/uv/issues/2130 as well? or was that addressed previously?

---

_Comment by @charliermarsh on 2024-03-24 23:43_

I don't think so, that issue surprises me but I'll look into it separately...

---

_@zanieb reviewed on 2024-03-25 19:31_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker.rs`:1046 on 2024-03-25 19:31_

It's a little confusing that the outcome in these comments is the same?

---

_@zanieb reviewed on 2024-03-25 19:33_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker.rs`:1037 on 2024-03-25 19:33_

This comment is bearing a lot of weight, am I understanding this correctly: because `simplified` removes true and `num_expressions` was the original length if there are less in the simplified version the whole thing is `None` because one of them was true?

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker.rs`:970 on 2024-03-25 19:34_

Maybe @BurntSushi should review this marker code.

---

_@zanieb reviewed on 2024-03-25 19:34_

---

_@charliermarsh reviewed on 2024-03-25 19:37_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker.rs`:1037 on 2024-03-25 19:37_

Yes, that's a correct understanding of the code.

---

_@zanieb reviewed on 2024-03-25 19:37_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:443 on 2024-03-25 19:37_

Oh nice!

---

_@charliermarsh reviewed on 2024-03-25 19:37_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker.rs`:1046 on 2024-03-25 19:37_

The previous branch could be "return the remaining expression".

---

_@zanieb reviewed on 2024-03-25 19:37_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_compile.rs`:123 on 2024-03-25 19:37_

Just wondering, why only when empty?

---

_@charliermarsh reviewed on 2024-03-25 19:37_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker.rs`:970 on 2024-03-25 19:37_

Sure, feel free @BurntSushi.

---

_@zanieb approved on 2024-03-25 19:39_

---

_@charliermarsh reviewed on 2024-03-25 19:40_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip_compile.rs`:123 on 2024-03-25 19:40_

It's possible that an extra _will be_ used for one of the source trees, but we can't know that until much later, since we don't have any of the metadata for the source trees yet.

---

_Comment by @zanieb on 2024-03-25 19:41_

Also addresses or supersedes:

- https://github.com/astral-sh/uv/pull/1650
- https://github.com/astral-sh/uv/issues/1619
- https://github.com/astral-sh/uv/issues/1597
- https://github.com/astral-sh/uv/issues/1634

---

_@charliermarsh reviewed on 2024-03-25 19:44_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker.rs`:1037 on 2024-03-25 19:44_

(I shall expand.)

---

_@charliermarsh reviewed on 2024-03-25 19:45_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:443 on 2024-03-25 19:45_

Yeah let's go...

---

_@zanieb reviewed on 2024-03-25 19:46_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip_compile.rs`:123 on 2024-03-25 19:46_

Ah okay 👍 a comment might be nice

---

_@charliermarsh reviewed on 2024-03-25 19:52_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker.rs`:970 on 2024-03-25 19:52_

Messaged with Andrew, I want to build on this branch (but not related to the markers) so I'm going to merge and address any review comments post-merge.

---

_Merged by @charliermarsh on 2024-03-25 20:27_

---

_Closed by @charliermarsh on 2024-03-25 20:27_

---

_Branch deleted on 2024-03-25 20:27_

---

_@BurntSushi reviewed on 2024-03-25 20:57_

LGTM. :-) Nice marker tests!

---

_Comment by @mahimairaja on 2025-04-01 02:23_

So how can we install using `uv` from a poetry based `project.toml`?

---
