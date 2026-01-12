```yaml
number: 14708
title: Use SHA256 from GitHub API for Python downloads
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/use-github-api-hashes
created_at: 2025-07-18T09:53:56Z
updated_at: 2025-07-18T12:03:56Z
url: https://github.com/astral-sh/uv/pull/14708
synced_at: 2026-01-12T16:11:21Z
```

# Use SHA256 from GitHub API for Python downloads

---

_@konstin_

We recently ran over the file limit and had to drop hash file from the releases page in favor of bulk SHA256SUMS files (https://github.com/astral-sh/python-build-standalone/pull/691). Conveniently, GitHub has recently started to add a SHA256 digest to the API. GitHub did not backfill the hashes for the old releases, so use the API hashes for newer assets, and eventually only download SHA256SUMS for older releases.

---

_Review requested from @geofft by @konstin on 2025-07-18 09:53_

---

_Review requested from @zanieb by @konstin on 2025-07-18 09:53_

---

_Label `internal` added by @konstin on 2025-07-18 09:53_

---

_@konstin reviewed on 2025-07-18 09:54_

---

_Review comment by @konstin on `crates/uv-python/fetch-download-metadata.py`:260 on 2025-07-18 09:54_

Assuming that GitHub commits to SHA256 for the forseeable future.

---

_@j178 reviewed on 2025-07-18 10:11_

---

_Review comment by @j178 on `crates/uv-python/fetch-download-metadata.py`:259 on 2025-07-18 10:11_

I think we only dropped the individual `.sha256` files, but the combined `SHA256SUMS` file is still around, and that’s the one the `fetch-python-metadata.py` script uses.

---

_@zanieb approved on 2025-07-18 11:38_

---

_Merged by @konstin on 2025-07-18 12:03_

---

_Closed by @konstin on 2025-07-18 12:03_

---

_Branch deleted on 2025-07-18 12:03_

---
