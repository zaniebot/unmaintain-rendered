```yaml
number: 13782
title: Update async-compression to 0.4.23
type: pull_request
state: closed
author: konstin
labels:
  - internal
assignees: []
draft: true
base: main
head: konsti/update-async-compression
created_at: 2025-06-02T11:28:00Z
updated_at: 2025-10-09T13:48:49Z
url: https://github.com/astral-sh/uv/pull/13782
synced_at: 2026-01-12T16:10:52Z
```

# Update async-compression to 0.4.23

---

_@konstin_

Our manual usage of the `xz2/static` blocks updates of async-compression as they switched from the unmaintained lzma-sys to liblzma-sys.


---

_Label `internal` added by @konstin on 2025-06-02 11:28_

---

_Comment by @nilskch on 2025-06-13 13:37_

Hey @konstin, sorry to jump in here, but do you think this PR will get over the line soon'ish? We want to consume UV as a lib, but we are running into a dependency conflict with [rattler](https://github.com/conda/rattler/blob/e159a4a9f49834383399b8da9a46e183cfe99908/Cargo.toml#L29).

---

_Comment by @konstin on 2025-06-13 16:28_

Mainly that liblzma is a rust library that pulls in a C submodule, which makes it hard for us to audit, especially for xz2/lzma specifically, while the old lzma-sys had a well-known maintainer. Ideally, we'd have the option to use lzma-rs, which is an lzma decoder written in safe rust, that we can audit well.

---

_Comment by @nilskch on 2025-06-14 14:04_

> Ideally, we'd have the option to use lzma-rs, which is an lzma decoder written in safe rust, that we can audit well.

Do you need help with that?

---

_Comment by @konstin on 2025-06-16 19:22_

That would be greatly appreciated! I filed an upstream issue here: https://github.com/Nullus157/async-compression/issues/345

---

_Comment by @nilskch on 2025-06-16 20:16_

Awesome, I will take a stab at it later this week! 

---

_Comment by @konstin on 2025-07-16 07:47_

@nilskch Did you get a chance to work on that?

---

_Closed by @konstin on 2025-10-09 13:48_

---
