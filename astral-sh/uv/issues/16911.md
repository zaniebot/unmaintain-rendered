```yaml
number: 16911
title: Drop xz-in-ZIP support
type: issue
state: open
author: woodruffw
labels:
  - breaking
assignees: []
created_at: 2025-12-01T17:05:52Z
updated_at: 2025-12-03T08:45:51Z
url: https://github.com/astral-sh/uv/issues/16911
synced_at: 2026-01-10T03:23:55Z
```

# Drop xz-in-ZIP support

---

_Issue opened by @woodruffw on 2025-12-01 17:05_

It'd be nice to reduce the footprint of our decompression stack to only the decompression schemes Python packaging (widely) uses. One that we could potentially eliminate is xz/lzma, since only a tiny minority of Python wheel distributions (should) be using it.

This would technically be a breaking change. However, it should be a marginal one, since PyPI itself doesn't allow any ZIP compression schemes other than DEFLATE and STORE (and eventually zstd):

https://github.com/pypi/warehouse/blob/6ba4eb2cb1482403a64805fc0d1839e4f36d1bb3/warehouse/forklift/legacy.py#L313

---

_Label `breaking` added by @woodruffw on 2025-12-01 17:05_

---

_Comment by @woodruffw on 2025-12-01 17:35_

We could also _probably_ drop `.tar.xz` support, since the sdist spec only allows sdists named `.tar.gz`. That one might cause some real-world breakage due to incidentally supported formats, e.g. https://github.com/astral-sh/uv/issues/2187

---

_Added to milestone `v0.10.0` by @woodruffw on 2025-12-01 17:36_

---

_Comment by @zanieb on 2025-12-01 17:36_

cc @lengau 

---

_Comment by @lengau on 2025-12-01 19:47_

Thanks @zanieb for the link. I don't know of any issues about this on the wheel side, but I'll comment about this on the sdist side in the other bug.

---

_Comment by @konstin on 2025-12-03 08:45_

I would drop xz/lzma support for both zips and tar archives so we can remove the dependency. PEP 625 requires `.tar.gz` for source distributions, and with warehouse enforcing DEFLATE or stored for wheels, other cases should be rare. We should start with a deprecation message to give users a chance to report user cases and/or migrate.

---
