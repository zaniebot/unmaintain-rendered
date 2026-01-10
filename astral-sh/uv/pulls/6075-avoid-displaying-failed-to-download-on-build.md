```yaml
number: 6075
title: "Avoid displaying \"failed to download\" on build failures for local source distributions"
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/build-err-download
created_at: 2024-08-14T00:04:55Z
updated_at: 2024-08-14T22:27:56Z
url: https://github.com/astral-sh/uv/pull/6075
synced_at: 2026-01-10T13:09:50Z
```

# Avoid displaying "failed to download" on build failures for local source distributions

---

_Pull request opened by @zanieb on 2024-08-14 00:04_

Especially with workspace members (e.g., [this new test case](https://github.com/astral-sh/uv/pull/6073/files#diff-273076013b4f5a8139defd5dcd24f5d1eb91c0266dceb4448fdeddceb79f7738R1377-R1379)), I find it very confusing that we say we failed to download these distributions.

---

_Label `error messages` added by @zanieb on 2024-08-14 00:04_

---

_@zanieb reviewed on 2024-08-14 00:05_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:179 on 2024-08-14 00:05_

Unfortunately, we have a duplicate "Failed to build" here because of `uv-distribution::Error::Build` — I'm exploring a way to avoid that.

---

_@zanieb reviewed on 2024-08-14 00:22_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:179 on 2024-08-14 00:22_

Removed in https://github.com/astral-sh/uv/pull/6075/commits/6cf1a949806c079dde6fdd94374eb7f503810ffb

It becomes a little ambiguous where we failed for some errors, but I think it's a net improvement.

---

_Marked ready for review by @zanieb on 2024-08-14 00:23_

---

_@zanieb reviewed on 2024-08-14 00:28_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:3485 on 2024-08-14 00:28_

This is arguably more ambiguous now, but I think dropping the duplicate package URL is nice. I'm not sure how to improve this in the general case? I think we'd need to implement display for `Error::FetchAndBuild` to include a bit more logic which seems wrong.

---

_@charliermarsh reviewed on 2024-08-14 22:26_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install.rs`:3485 on 2024-08-14 22:26_

Don't we have the URL in the line just above? It looks like it was repeated

---

_@charliermarsh approved on 2024-08-14 22:26_

Significantly better.

---

_@zanieb reviewed on 2024-08-14 22:27_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:3485 on 2024-08-14 22:27_

Yeah, it's an improvement not to repeat the URL. The ambiguity here is that it's not clear if the following error, e.g., "Build backend failed to determine metadata", is a _build_ or _download_ error.

---

_Merged by @zanieb on 2024-08-14 22:27_

---

_Closed by @zanieb on 2024-08-14 22:27_

---

_Branch deleted on 2024-08-14 22:27_

---
