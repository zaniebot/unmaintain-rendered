```yaml
number: 15644
title: Allow registries to pre-provide core metadata
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/pyx-simple
created_at: 2025-09-02T21:58:33Z
updated_at: 2025-09-03T02:12:48Z
url: https://github.com/astral-sh/uv/pull/15644
synced_at: 2026-01-12T16:11:52Z
```

# Allow registries to pre-provide core metadata

---

_@charliermarsh_

## Summary

This PR adds support for the `application/vnd.pyx.simple.v1` content type, similar to `application/vnd.pypi.simple.v1` with the exception that it can also include core metadata for package-versions directly. This enables pyx to send down metadata for package-versions in advance, while continuing to use the expected format for all other clients. Since the media type is namespaced (`pyx`) and version (`v1`), it also means that if we're able to upstream these changes into a PEP in the future (and therefore evolve the format), we can seamlessly add support in pyx and while continuing to power existing clients.


---

_@charliermarsh reviewed on 2025-09-02 21:59_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:576 on 2025-09-02 21:59_

In theory, we should be able to pass `MediaType::all()` to all registries, and registries that don't support it should just ignore the new media types. But I'm worried that misconfigured registries could reject them and error.

---

_@charliermarsh reviewed on 2025-09-02 22:00_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:232 on 2025-09-02 22:00_

The store is only needed for `is_known_url`, so maybe there's a more lightweight pattern we can use.

---

_Review requested from @zanieb by @charliermarsh on 2025-09-02 22:00_

---

_Review requested from @konstin by @charliermarsh on 2025-09-02 22:00_

---

_Label `enhancement` added by @charliermarsh on 2025-09-02 22:00_

---

_Marked ready for review by @charliermarsh on 2025-09-02 22:00_

---

_@zanieb reviewed on 2025-09-02 22:18_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:232 on 2025-09-02 22:18_

Seems worth a TODO but not worth blocking.

---

_@zanieb reviewed on 2025-09-02 22:18_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:576 on 2025-09-02 22:18_

Might be worth encoding in a comment here or in `MediaType::pypi`

---

_@zanieb reviewed on 2025-09-02 22:20_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:1221 on 2025-09-02 22:20_

Assuming this matches an existing pattern but it'd be nice to say why this is skipped in the future.

---

_@zanieb reviewed on 2025-09-02 22:20_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:1227 on 2025-09-02 22:20_

Don't we have `filename.version()`?

---

_@zanieb reviewed on 2025-09-02 22:23_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:2384 on 2025-09-02 22:23_

Nit: It'd be nice to `continue` here since this is so nested already

---

_@zanieb approved on 2025-09-02 22:23_

---

_Merged by @charliermarsh on 2025-09-03 00:56_

---

_Closed by @charliermarsh on 2025-09-03 00:56_

---

_Branch deleted on 2025-09-03 00:56_

---
