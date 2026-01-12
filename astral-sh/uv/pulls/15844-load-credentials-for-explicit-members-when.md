```yaml
number: 15844
title: Load credentials for explicit members when lowering
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/load
created_at: 2025-09-15T00:11:08Z
updated_at: 2025-09-15T13:54:39Z
url: https://github.com/astral-sh/uv/pull/15844
synced_at: 2026-01-12T16:11:58Z
```

# Load credentials for explicit members when lowering

---

_@charliermarsh_

## Summary

If the target for `uv pip compile` is a `pyproject.toml` in a subdirectory, we won't have loaded the credentials when we go to lower (since it won't be loaded as part of "configuration discovery"). We now add those indexes just-in-time.

Closes https://github.com/astral-sh/uv/issues/15362.


---

_Review requested from @zanieb by @charliermarsh on 2025-09-15 00:11_

---

_Review requested from @konstin by @charliermarsh on 2025-09-15 00:11_

---

_Label `bug` added by @charliermarsh on 2025-09-15 00:11_

---

_Marked ready for review by @charliermarsh on 2025-09-15 00:11_

---

_Review comment by @konstin on `crates/uv-distribution/src/metadata/lowering.rs`:226 on 2025-09-15 07:40_

nit: We can move this outside the let-Some to avoid stateful action in the `inspect`

---

_@konstin approved on 2025-09-15 07:41_

---

_@charliermarsh reviewed on 2025-09-15 13:20_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/lowering.rs`:226 on 2025-09-15 13:20_

It's kind of a pain because the thing we return in the `map` isn't the same as this `index`.

---

_Review comment by @konstin on `crates/uv-distribution/src/metadata/lowering.rs`:226 on 2025-09-15 13:23_

I thought like this:

```rust
                            // Identify the named index from either the project indexes or the workspace indexes,
                            // in that order.
                            let Some(index) = locations
                                .indexes()
                                .filter(|index| matches!(index.origin, Some(Origin::Cli)))
                                .chain(project_indexes.iter())
                                .chain(workspace.indexes().iter())
                                .find(|Index { name, .. }| {
                                    name.as_ref().is_some_and(|name| *name == index)
                                })
                            else {
                                return Err(LoweringError::MissingIndex(
                                    requirement.name.clone(),
                                    index,
                                ));
                            };
                            if let Some(credentials) = index.credentials() {
                                let credentials = Arc::new(credentials);
                                uv_auth::store_credentials(index.raw_url(), credentials);
                            }
                            let Index {
                                url, format: kind, ..
                            } = index;
                            let index = IndexMetadata {
                                url: url.clone(),
                                format: *kind,
                            };
```

or directly with:

```rust
let index = IndexMetadata {
    url: index.url.clone(),
    format: index.format,
};
```

---

_@konstin reviewed on 2025-09-15 13:23_

---

_@charliermarsh reviewed on 2025-09-15 13:36_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/metadata/lowering.rs`:226 on 2025-09-15 13:36_

Yeah I changed it (I just don't like it as much -- you're probably right though).

---

_Merged by @charliermarsh on 2025-09-15 13:54_

---

_Closed by @charliermarsh on 2025-09-15 13:54_

---

_Branch deleted on 2025-09-15 13:54_

---
