```yaml
number: 12407
title: "Support `--find-links`-style \"flat\" indexes in `[[tool.uv.index]]`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - no-build
assignees: []
merged: true
base: main
head: charlie/index-ii
created_at: 2025-03-24T01:56:15Z
updated_at: 2025-03-26T01:14:45Z
url: https://github.com/astral-sh/uv/pull/12407
synced_at: 2026-01-12T16:10:16Z
```

# Support `--find-links`-style "flat" indexes in `[[tool.uv.index]]`

---

_@charliermarsh_

## Summary

This PR extends `[[tool.uv.index]]` to support `--find-links`-style "flat" indexes, so that users can point to such indexes without using `--find-links` _and_ get access to the full functionality of `[[tool.uv.index]]` (e.g., they can now pin packages to `--find-links`-style indexes).

Note that, at present, `--find-links` indexes actually have some quirky behavior, in that we combine them into a single entity and then merge the discovered distributions into each Simple API-style index. The motivation here, IIRC, was to match pip's behavior quite closely. I'm interested in _removing_ that behavior, but it'd be breaking (and may also be inconvenient for some use-cases). So, the behavior for indexes passed in via `--find-links` remains completely unchanged. However, `[[tool.uv.index]]` entries with `format = "flat"` are now treated identically to those defined with `format = "simple"` (the default), in that we stop after we find the first-matching index, etc.

Closes https://github.com/astral-sh/uv/issues/11634.

---

_Label `enhancement` added by @charliermarsh on 2025-03-24 01:56_

---

_Label `no-build` added by @charliermarsh on 2025-03-24 02:04_

---

_Marked ready for review by @charliermarsh on 2025-03-24 02:11_

---

_Review requested from @zanieb by @charliermarsh on 2025-03-24 14:15_

---

_Review requested from @konstin by @charliermarsh on 2025-03-24 14:15_

---

_Review comment by @konstin on `crates/uv-resolver/src/flat_index.rs`:66 on 2025-03-24 19:41_

This field docs say:

    /// Whether any `--find-links` entries could not be resolved due to a lack of network
    /// connectivity.

which sounds different from this docstring.

---

_Review comment by @konstin on `crates/uv-distribution-types/src/index.rs`:73 on 2025-03-24 19:46_

Can't comment there, but the jsonschema description of `IndexUrl` needs to be updated 

---

_Review comment by @konstin on `crates/uv-client/src/registry_client.rs`:309 on 2025-03-24 19:48_

Is this still "approximately"?

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:9673 on 2025-03-24 19:50_

Can you add a description what this test is checking? It's not clear to me from the name

---

_Review comment by @konstin on `crates/uv/tests/it/lock.rs`:10124 on 2025-03-24 19:51_

nit (also below):

```suggestion
        Url::from_file_path(context.temp_dir.join("links/")).unwrap()
```

---

_@konstin approved on 2025-03-24 19:54_

---

_@zanieb approved on 2025-03-25 01:49_

I approve of the design choices here (but did not read the implementation)

---

_Merged by @charliermarsh on 2025-03-26 01:14_

---

_Closed by @charliermarsh on 2025-03-26 01:14_

---

_Branch deleted on 2025-03-26 01:14_

---
