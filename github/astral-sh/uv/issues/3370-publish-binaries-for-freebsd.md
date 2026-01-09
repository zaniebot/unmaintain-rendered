---
number: 3370
title: Publish binaries for FreeBSD
type: issue
state: open
author: zanieb
labels:
  - releases
assignees: []
created_at: 2024-05-04T06:07:27Z
updated_at: 2025-12-09T05:52:03Z
url: https://github.com/astral-sh/uv/issues/3370
synced_at: 2026-01-07T13:12:17-06:00
---

# Publish binaries for FreeBSD

---

_Issue opened by @zanieb on 2024-05-04 06:07_

e.g. the installer currently fails with

```
# curl -LsSf https://astral.sh/uv/install.sh | sh
ERROR: there isn't a package for x86_64-unknown-freebsd
```

I'm not sure how feasible this is, but here's a tracking issue.

Related #2442 

---

_Referenced in [astral-sh/uv#3369](../../astral-sh/uv/issues/3369.md) on 2024-05-04 06:08_

---

_Label `release` added by @charliermarsh on 2024-05-04 12:04_

---

_Comment by @charliermarsh on 2024-05-04 12:04_

For what it's worth, you can't upload FreeBSD wheels to PyPI, so users will always need to build any dependencies from source (IIUC).

---

_Comment by @zanieb on 2024-05-04 15:11_

I'm a little confused since I thought we didn't have Python dependencies but that's good to know.

---

_Comment by @charliermarsh on 2024-05-04 15:31_

Ah, to clarify, we don’t have any Python deps, I just mean: for any dependencies they install with pip or uv, IIUC they will end up building from source.

---

_Comment by @konstin on 2024-05-06 07:39_

Note that freebsd is not supported by github actions, we either need a custom cross compiling solution or use cirrus ci

---

_Comment by @dbohdan on 2024-08-25 21:11_

https://github.com/cross-platform-actions/ lets you use Free/Net/OpenBSD in GitHub Actions. I had a good experience with it in a C project. While I haven't used it, there is also https://github.com/houseabsolute/actions-rust-cross that cross-compiles Rust.

---

_Comment by @zanieb on 2024-08-26 17:07_

(We're currently at zero 👍 for this issue, so in addition to this being technically challenging to maintain this is pretty low priority since there's not a lot of clear interest)

---

_Comment by @donnex on 2024-09-28 11:21_

I ran into this issue today. The `uv` package in FreeBSD ports is really old and outdated.

Has anyone been able to successfully compile the latest release of `uv` in FreeBSD? I had no luck with manual buld by cloning the repo.   

---

_Comment by @konstin on 2024-09-28 13:40_

Can you share a log of what failed in the manual build?

---

_Comment by @donnex on 2024-09-29 09:40_

Yeah of course. I'm not sure if it's anything related to `uv` directly.

```
error: failed to run custom build command for `tikv-jemalloc-sys v0.6.0+5.3.0-1-ge13ca993e8ccb9ba9847cc330696e02839f328f7`
```
[uv-cargo-build.log](https://github.com/user-attachments/files/17178728/uv-cargo-build.log)

---

_Referenced in [astral-sh/uv#7780](../../astral-sh/uv/pulls/7780.md) on 2024-09-29 13:28_

---

_Comment by @konstin on 2024-09-29 13:28_

Could you try https://github.com/astral-sh/uv/pull/7780?

---

_Comment by @donnex on 2024-09-29 14:51_

@konstin works prefectly! :partying_face: :pray: 

```
target/release/uv version
uv 0.4.17 (c2f19dfd6 2024-09-29)
```

---

_Referenced in [astral-sh/uv#8269](../../astral-sh/uv/pulls/8269.md) on 2024-10-16 19:35_

---
