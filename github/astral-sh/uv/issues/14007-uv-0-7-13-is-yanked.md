---
number: 14007
title: uv 0.7.13 is yanked
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2025-06-12T22:03:22Z
updated_at: 2025-06-12T23:11:31Z
url: https://github.com/astral-sh/uv/issues/14007
synced_at: 2026-01-07T13:12:18-06:00
---

# uv 0.7.13 is yanked

---

_Issue opened by @zanieb on 2025-06-12 22:03_

👋 

We merged an external contribution for RISCV-64 in https://github.com/astral-sh/uv/pull/12688 (revert being discussed in #14006) which broke our 0.7.13 release process because the `manylinux_2_31_riscv64` platform tag was rejected by PyPI.

PyPI does not yet support waiting until all files for a release are uploaded before going "live" so only a subset of the wheels were published for 0.7.13. This provides a bad experience for users, where they either cannot use the new version or fallback to building from source.

Consequently, we've yanked the release. We will either complete the upload of files for 0.7.13, and un-yank it, or release a new version.

---

_Comment by @zanieb on 2025-06-12 22:39_

Alright, I've downloaded the artifacts from CI and manually published the relevant ones. The release is un-yanked now.

---

_Closed by @zanieb on 2025-06-12 22:39_

---

_Referenced in [astral-sh/uv#14010](../../astral-sh/uv/issues/14010.md) on 2025-06-12 22:41_

---

_Comment by @zanieb on 2025-06-12 22:44_

It'll take us a minute to "publish" the release to GitHub manually as well.

---

_Comment by @Gankra on 2025-06-12 23:11_

The github release is published and tagged and the shell installer seems to be working

---

_Referenced in [pypi/warehouse#18243](../../pypi/warehouse/issues/18243.md) on 2025-06-13 12:12_

---

_Referenced in [PyO3/maturin#2650](../../PyO3/maturin/pulls/2650.md) on 2025-06-13 17:51_

---
