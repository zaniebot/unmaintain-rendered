---
number: 1917
title: "Use `std` for `uv-trampoline`"
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - windows
assignees: []
created_at: 2024-02-23T15:29:18Z
updated_at: 2024-07-06T20:38:47Z
url: https://github.com/astral-sh/uv/issues/1917
synced_at: 2026-01-10T01:23:09Z
---

# Use `std` for `uv-trampoline`

---

_Issue opened by @MichaReiser on 2024-02-23 15:29_

The `uv-trampoline` is used by `uv` to generate exe binaries for python entrypoint scripts (launcher scripts). 

Our current implementation is a fork of [posy's trampoline](https://github.com/njsmith/posy/tree/dda22e6f90f5fefa339b869dd2bbe107f5b48448/src/trampolines/windows-trampolines/posy-trampoline) and one of the author's primary goal is to generate binaries with a minimal size. They achieve that by not using Rust's `std` and only use `win32` APIs. Our current launcher has a size of a little less than 20KB.

We agree that it's important that the launcher is small because we include it twice into the `uv` binary and append it to every launcher script. However, not using `std` means that we must use unsafe `win32` API almost exclusively, which requires extra care and comes at the risk of UB and potential security vulnerabilities. 

Only using `non_std` is also no guarantee for a small binary. It requires extra care that no implicit panics "bloat" the binary size. 

That's why we think it's worth exploring using `std` for the trampoline. I created https://github.com/astral-sh/uv/pull/1864 and documented my initial findings. It seems that using `std` with `panic=abort` increases the binary size to roughly 90KB, which seems acceptable. It's also possible that the size can be reduced further. I haven't spent a lot of time trying. 

The goal is to explore using `std` for `uv-trampoline` while keeping the binary size minimal (by e.g. avoiding calling into `panic` handlers etc).

---

_Label `help wanted` added by @MichaReiser on 2024-02-23 15:29_

---

_Label `windows` added by @MichaReiser on 2024-02-23 15:29_

---

_Referenced in [astral-sh/uv#1864](../../astral-sh/uv/pulls/1864.md) on 2024-02-23 15:29_

---

_Referenced in [astral-sh/uv#1126](../../astral-sh/uv/issues/1126.md) on 2024-02-23 22:34_

---

_Referenced in [astral-sh/uv#4722](../../astral-sh/uv/pulls/4722.md) on 2024-07-02 04:44_

---

_Closed by @konstin on 2024-07-06 20:38_

---

_Closed by @konstin on 2024-07-06 20:38_

---
