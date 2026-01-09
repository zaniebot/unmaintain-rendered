---
number: 10367
title: Drop UV_STACK_SIZE in favour of RUST_MIN_STACK
type: issue
state: closed
author: Gankra
labels:
  - internal
assignees: []
created_at: 2025-01-07T14:52:45Z
updated_at: 2025-01-15T03:35:18Z
url: https://github.com/astral-sh/uv/issues/10367
synced_at: 2026-01-07T13:12:18-06:00
---

# Drop UV_STACK_SIZE in favour of RUST_MIN_STACK

---

_Issue opened by @Gankra on 2025-01-07 14:52_

[RUST_MIN_STACK](https://doc.rust-lang.org/std/thread/#stack-size) is fairly obscure and barely documented but it does exist specifically to do exactly what UV_STACK_SIZE does (it would affect *all* non-main threads and not just tokio threads, but the fact that we only resize tokio threads is not really intentional so much as Convenient).

Alternatively: it's really unclear why we need to set the stack size specifically on Windows. All information I can find suggests Rust already forces Windows, macOS, and Linux to all use 2MiB stacks for non-main-threads by default (and UV_STACK_SIZE doesn't affect the main thread). 

Unfortunately there's a ton of possibilities:
* Newer versions of Rust fixed this and we don't actually need it anymore?
* Windows wastes more of your stack space with OS-internal datastructures (like the TEB)?
* Some platform-specific types are weirdly bigger on Windows?
* Windows' ABI/codegen forces more stackspills bloating the stack way more?

---

_Label `internal` added by @zanieb on 2025-01-07 15:38_

---

_Comment by @konstin on 2025-01-08 09:50_

Another consideration is that usually the main thread overflowed, which afaik gets built by the OS instead of rust.

---

_Referenced in [astral-sh/uv#10479](../../astral-sh/uv/pulls/10479.md) on 2025-01-10 20:11_

---

_Closed by @Gankra on 2025-01-15 03:35_

---

_Closed by @Gankra on 2025-01-15 03:35_

---
