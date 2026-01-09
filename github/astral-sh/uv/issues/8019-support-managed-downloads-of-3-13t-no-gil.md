---
number: 8019
title: Support managed downloads of 3.13t (no-gil / freethreaded)
type: issue
state: closed
author: zanieb
labels:
  - duplicate
  - enhancement
assignees: []
created_at: 2024-10-08T20:41:23Z
updated_at: 2024-10-09T19:25:59Z
url: https://github.com/astral-sh/uv/issues/8019
synced_at: 2026-01-07T13:12:17-06:00
---

# Support managed downloads of 3.13t (no-gil / freethreaded)

---

_Issue opened by @zanieb on 2024-10-08 20:41_

Right now, we only support a single build per CPython version. We need to add support for other build options to allow installing the [freethreaded builds](https://github.com/indygreg/python-build-standalone/releases/tag/20241008). This will require changes to our download metadata fetch and template scripts. We should probably aim support debug builds too.



---

_Label `enhancement` added by @zanieb on 2024-10-08 20:41_

---

_Referenced in [astral-sh/uv#8015](../../astral-sh/uv/issues/8015.md) on 2024-10-08 22:05_

---

_Comment by @Crypto-Spartan on 2024-10-08 22:09_

In addition, I'd love it if `uv` were able to support other builds from [python-build-standalone](https://github.com/indygreg/python-build-standalone), such as `x86_64-unknown-linux-gnu-pgo+lto-full.tar.zst`, `x86_64_v3-unknown-linux-gnu-install_only_stripped.tar.gz`, etc.

I know we talked about this in the other issue, just wanted to add the request here specifically.

---

_Comment by @charliermarsh on 2024-10-08 22:11_

Can I ask why you need / want to use those other builds?

---

_Comment by @Crypto-Spartan on 2024-10-08 22:15_

Those other builds provide optimizations done by the compiler that aren't done in the base `-install_only_stripped.tar.gz` builds. I just chose those 2 as examples, but I believe `uv` should support any build from [python-build-standalone](https://github.com/indygreg/python-build-standalone).

---

_Comment by @charliermarsh on 2024-10-08 22:16_

The install-only-stripped builds include PGO and LTO.

---

_Comment by @Crypto-Spartan on 2024-10-08 22:21_

Ah, I didn't realize that. when I checked the docs at https://gregoryszorc.com/docs/python-build-standalone/main/running.html#obtaining-distributions, it didn't seem that way. I found https://gregoryszorc.com/docs/python-build-standalone/main/distributions.html#install-only-archive had it listed, all the way at the bottom of the page.

---

_Comment by @Crypto-Spartan on 2024-10-08 22:24_

What about the newer CPU architectures such as `x86_64_v3`?

---

_Comment by @zanieb on 2024-10-08 22:28_

> What about the newer CPU architectures such as `x86_64_v3`?

Yes, those also prefer the optimized builds.

---

_Comment by @Crypto-Spartan on 2024-10-08 22:32_

I figured that. I was trying to ask:
Is `uv` open to supporting installing builds with newer CPU architectures such as `x86_64_v3`? I didn't see any of those builds listed in https://github.com/astral-sh/uv/blob/f6fd849f2c63e3b319ab03c936beab866dc303ba/crates/uv-python/download-metadata.json

---

_Comment by @zanieb on 2024-10-08 23:25_

We should chat about that in another issue as it's totally unrelated to this thread — but, yes? If we can detect the architecture reliably.

---

_Referenced in [hynek/argon2-cffi-bindings#70](../../hynek/argon2-cffi-bindings/pulls/70.md) on 2024-10-09 12:08_

---

_Referenced in [astral-sh/uv#8037](../../astral-sh/uv/issues/8037.md) on 2024-10-09 13:41_

---

_Label `duplicate` added by @zanieb on 2024-10-09 19:25_

---

_Comment by @zanieb on 2024-10-09 19:25_

I actually had already opened an issue to track this at https://github.com/astral-sh/uv/issues/7193

---

_Closed by @zanieb on 2024-10-09 19:25_

---
