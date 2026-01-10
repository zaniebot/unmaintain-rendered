```yaml
number: 1574
title: Add graceful fallback for Artifactory indexes
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/slice
created_at: 2024-02-17T04:39:02Z
updated_at: 2024-02-17T14:37:07Z
url: https://github.com/astral-sh/uv/pull/1574
synced_at: 2026-01-10T15:33:24Z
```

# Add graceful fallback for Artifactory indexes

---

_Pull request opened by @charliermarsh on 2024-02-17 04:39_

## Summary

There are more details in https://github.com/astral-sh/uv/issues/1370, but it looks like Artifactory servers have incorrect behavior when it comes to HTTP range requests, in that they return `Accept-Ranges: bytes`, but then incorrectly return 200 requests when you actually ask for a given range.

This PR ensures that we fallback gracefully in this case. It's built on https://github.com/prefix-dev/async_http_range_reader/pull/5. Assuming that gets merged upstream, we can then remove the Git dependency.

Closes https://github.com/astral-sh/uv/issues/1370.

## Test Plan

`cargo run pip install requests -i https://killjoyuvbug.jfrog.io/artifactory/api/pypi/pypi/simple --verbose`

---

_Review comment by @charliermarsh on `Cargo.toml`:24 on 2024-02-17 04:39_

See: https://github.com/prefix-dev/async_http_range_reader/pull/5

---

_@charliermarsh reviewed on 2024-02-17 04:39_

---

_Label `bug` added by @charliermarsh on 2024-02-17 04:39_

---

_Review requested from @zanieb by @charliermarsh on 2024-02-17 04:39_

---

_Review requested from @konstin by @charliermarsh on 2024-02-17 04:39_

---

_@zanieb reviewed on 2024-02-17 05:17_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:461 on 2024-02-17 05:17_

Should we trace this at a warn or something?

---

_@zanieb approved on 2024-02-17 05:17_

---

_@charliermarsh reviewed on 2024-02-17 12:56_

---

_Review comment by @charliermarsh on `Cargo.toml`:24 on 2024-02-17 12:56_

Merged and released with many thanks to @baszalmstra!

---

_Merged by @charliermarsh on 2024-02-17 14:37_

---

_Closed by @charliermarsh on 2024-02-17 14:37_

---

_Branch deleted on 2024-02-17 14:37_

---
