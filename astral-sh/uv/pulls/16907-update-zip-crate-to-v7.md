```yaml
number: 16907
title: Update zip crate to v7
type: pull_request
state: open
author: konstin
labels:
  - internal
  - error messages
assignees: []
draft: true
base: main
head: konsti/update-zip-6
created_at: 2025-12-01T10:05:22Z
updated_at: 2026-01-05T14:43:26Z
url: https://github.com/astral-sh/uv/pull/16907
synced_at: 2026-01-12T16:12:31Z
```

# Update zip crate to v7

---

_@konstin_

We're showing a bad error message in https://github.com/astral-sh/uv/issues/16906 where we say duplicate filename but don't show the filename, upgrading the zip crate shows the filename in the error message.

Older versions of the crate are still supported.

This pulls in a new dependency: https://github.com/hasenbanck/lzma-rust2. No benchmarking, xz compressed archives are rare in Python. I've asked for a new release for the zip crate so we don't start with a previous version in https://github.com/zip-rs/zip2/issues/479

---

_Label `internal` added by @konstin on 2025-12-01 10:05_

---

_Label `error messages` added by @konstin on 2025-12-01 10:05_

---

_Comment by @konstin on 2025-12-01 10:07_

@musicinmybrain @mgorny @wetneb Would this cause problems for you with distro packaging? I remember we held back updating the zip crate due to concerns from you earlier.

---

_Review requested from @zanieb by @konstin on 2025-12-01 10:09_

---

_Review request for @zanieb removed by @konstin on 2025-12-01 10:09_

---

_Review requested from @charliermarsh by @konstin on 2025-12-01 10:09_

---

_Review requested from @zanieb by @konstin on 2025-12-01 10:09_

---

_Review requested from @woodruffw by @konstin on 2025-12-01 10:09_

---

_Comment by @wetneb on 2025-12-01 10:12_

Thanks for checking! As far as Debian is concerned, upgrading dependencies to the latest version rarely hurts. This case looks fine to me - it can be that Debian's packaging of uv initially downgrades zip to version 5, until version 6 is available.

---

_Comment by @musicinmybrain on 2025-12-01 10:54_

Thanks for checking! This should be just fine. We don’t have `rust-zip` updated to 6.0 yet in Fedora, but since several older versions are still allowed by the version range in `Cargo.toml` in this PR, that won’t be a problem. The big jump for us was from pre-1.0 “old maintainers” to post-1.0 “new maintainer who took over abruptly and then released tons of new code,” https://github.com/astral-sh/uv/issues/3642.

---

_Comment by @konstin on 2025-12-01 11:04_

I've opened https://github.com/zip-rs/zip2/issues/479 where I asked for new release, you might want to comment there if you have specific concerns about (breaking) changes.

---

_@woodruffw approved on 2025-12-01 14:19_

---

_Converted to draft by @konstin on 2025-12-16 12:13_

---

_Renamed from "Update zip crate to v6" to "Update zip crate to v7" by @konstin on 2026-01-05 14:43_

---
