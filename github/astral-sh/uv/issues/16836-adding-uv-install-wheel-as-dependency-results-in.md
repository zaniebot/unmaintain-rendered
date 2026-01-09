---
number: 16836
title: Adding uv-install-wheel as dependency results in cargo build error on Windows
type: issue
state: closed
author: pavelzw
labels:
  - bug
  - help wanted
  - releases
assignees: []
created_at: 2025-11-24T16:49:06Z
updated_at: 2025-12-02T13:50:40Z
url: https://github.com/astral-sh/uv/issues/16836
synced_at: 2026-01-07T13:12:19-06:00
---

# Adding uv-install-wheel as dependency results in cargo build error on Windows

---

_Issue opened by @pavelzw on 2025-11-24 16:49_

```toml
# Cargo.toml
[package]
name = "uv-bug"
version = "0.1.0"
edition = "2024"

[dependencies]
uv-install-wheel = "0.0.1"
```

```rs
// main.rs
fn main() {
    println!("Hello, world!");
}
```

building this on Windows results in the following error:

```
   Compiling uv-trampoline-builder v0.0.1
error: couldn't read `C:\Users\runneradmin\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\uv-trampoline-builder-0.0.1\src\../../uv-trampoline/trampolines/uv-trampoline-x86_64-gui.exe`: The system cannot find the path specified. (os error 3)
  --> C:\Users\runneradmin\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\uv-trampoline-builder-0.0.1\src\lib.rs:18:5
   |
18 |     include_bytes!("../../uv-trampoline/trampolines/uv-trampoline-x86_64-gui.exe");
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

error: couldn't read `C:\Users\runneradmin\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\uv-trampoline-builder-0.0.1\src\../../uv-trampoline/trampolines/uv-trampoline-x86_64-console.exe`: The system cannot find the path specified. (os error 3)
  --> C:\Users\runneradmin\.cargo\registry\src\index.crates.io-1949cf8c6b5b557f\uv-trampoline-builder-0.0.1\src\lib.rs:22:5
   |
22 |     include_bytes!("../../uv-trampoline/trampolines/uv-trampoline-x86_64-console.exe");
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

error: could not compile `uv-trampoline-builder` (lib) due to 2 previous errors
warning: build failed, waiting for other jobs to finish...
```

this is because the binaries referenced in https://github.com/astral-sh/uv/blob/4b92f4fde442ee64c1ad2fc17182ef02cd32de49/crates/uv-trampoline-builder/src/lib.rs#L20-L22

are not part of the `uv-trampoline-builder` crate

@zanieb

---

_Referenced in [Quantco/pixi-pack#243](../../Quantco/pixi-pack/pulls/243.md) on 2025-11-24 16:49_

---

_Referenced in [conda/rattler#1866](../../conda/rattler/pulls/1866.md) on 2025-11-24 16:49_

---

_Renamed from "Adding uv-install-wheel/ results in cargo build error on Windows" to "Adding uv-install-wheel as dependency results in cargo build error on Windows" by @pavelzw on 2025-11-24 16:53_

---

_Comment by @zanieb on 2025-12-01 16:54_

I'm not sure what the best way to fix this is as I'm sort of a Rust packaging beginner :)

It's unfortunate that we need to include these binary blobs in the crate, but I don't think there's a feasible alternative.

cc @Gankra 

---

_Comment by @konstin on 2025-12-01 16:57_

We could copy the file to `uv-trampoline-builder` instead of `uv-trampoline`, and make sure the cargo include mention the build trampoline directory.

---

_Label `bug` added by @zanieb on 2025-12-01 16:59_

---

_Label `releases` added by @zanieb on 2025-12-01 16:59_

---

_Label `help wanted` added by @zanieb on 2025-12-01 16:59_

---

_Referenced in [astral-sh/uv#16922](../../astral-sh/uv/pulls/16922.md) on 2025-12-02 09:09_

---

_Closed by @zanieb on 2025-12-02 13:50_

---

_Referenced in [dzbarsky/rules_rs#6](../../dzbarsky/rules_rs/issues/6.md) on 2025-12-08 11:09_

---
