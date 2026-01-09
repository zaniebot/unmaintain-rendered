---
number: 12662
title: "`cargo test` for 0.5.6 fails on s390x, riscv5 and ppc64le"
type: issue
state: closed
author: WhyNotHugo
labels: []
assignees: []
created_at: 2024-08-04T14:33:45Z
updated_at: 2024-08-05T07:49:05Z
url: https://github.com/astral-sh/ruff/issues/12662
synced_at: 2026-01-07T13:12:15-06:00
---

# `cargo test` for 0.5.6 fails on s390x, riscv5 and ppc64le

---

_Issue opened by @WhyNotHugo on 2024-08-04 14:33_

```
   Compiling codspeed v2.6.0
error[E0432]: unresolved import `self::arch`
  --> /home/buildozer/.cargo/registry/src/index.crates.io-d11c229612889eed/codspeed-2.6.0/src/request/mod.rs:13:15
   |
13 | pub use self::arch::{send_client_request, Value};
   |               ^^^^ could not find `arch` in `self`
   |
note: found an item that was configured out
  --> /home/buildozer/.cargo/registry/src/index.crates.io-d11c229612889eed/codspeed-2.6.0/src/request/mod.rs:17:5
   |
17 | mod arch;
   |     ^^^^
   = note: the item is gated behind the `x86_64` feature
note: found an item that was configured out
  --> /home/buildozer/.cargo/registry/src/index.crates.io-d11c229612889eed/codspeed-2.6.0/src/request/mod.rs:21:5
   |
21 | mod arch;
   |     ^^^^
   = note: the item is gated behind the `x86` feature
note: found an item that was configured out
  --> /home/buildozer/.cargo/registry/src/index.crates.io-d11c229612889eed/codspeed-2.6.0/src/request/mod.rs:25:5
   |
25 | mod arch;
   |     ^^^^
   = note: the item is gated behind the `arm` feature
note: found an item that was configured out
  --> /home/buildozer/.cargo/registry/src/index.crates.io-d11c229612889eed/codspeed-2.6.0/src/request/mod.rs:29:5
   |
29 | mod arch;
   |     ^^^^
   = note: the item is gated behind the `aarch64` feature
For more information about this error, try `rustc --explain E0432`.
error: could not compile `codspeed` (lib) due to 1 previous error
warning: build failed, waiting for other jobs to finish...
```

Full build logs for [s390x](https://gitlab.alpinelinux.org/WhyNotHugo/aports/-/jobs/1472530), [riscv5](https://gitlab.alpinelinux.org/WhyNotHugo/aports/-/jobs/1472535) and [ppc64le](https://gitlab.alpinelinux.org/WhyNotHugo/aports/-/jobs/1472531).

At a glance, I get the impression that codspeed has architecture-specific code, so requires explicit support for these architectures.

---

_Comment by @MichaReiser on 2024-08-04 14:58_

Hmm interesting. The code is only used for benchmarking. You can build ruff by using `cargo build --bin ruff`

---

_Comment by @nekopsykose on 2024-08-04 15:25_

since this fails in `cargo test`, you can skip the bench tests specifically with `cargo test --workspace --exclude ruff_benchmark`. that skips building this specific dep since it's no longer in the graph

---

_Comment by @WhyNotHugo on 2024-08-04 16:53_

Yeah, we should honestly be skipping benchmarks when packaging anyway.

---

_Comment by @WhyNotHugo on 2024-08-04 16:54_

Isn't `cargo test` supposed to run only tests and `cargo bench` runs benchmarks?


---

_Comment by @nekopsykose on 2024-08-04 17:00_

afaict yes, but `cargo test` will still build the workspace to perhaps detect `#[test]` declarations- even if nothing in ruff_benchmark has any of those and only criterion `bench` thingies in it, the crate as a whole will be built (and before that, the deps of it)

i could be wrong but that's how it seems to function currently at the workspace level in any project

---

_Renamed from "0.5.6 fails to build on s390x, riscv5 and ppc64le" to "`cargo test` for 0.5.6 fails on s390x, riscv5 and ppc64le" by @WhyNotHugo on 2024-08-04 18:26_

---

_Comment by @MichaReiser on 2024-08-04 19:30_

Excluding the crate from tests is probably a good idea either way 

---

_Comment by @WhyNotHugo on 2024-08-04 20:22_

Yeah, I've excluded it when packaging/testing.

---

_Referenced in [astral-sh/ruff#12678](../../astral-sh/ruff/pulls/12678.md) on 2024-08-05 06:11_

---

_Referenced in [astral-sh/ruff#12680](../../astral-sh/ruff/pulls/12680.md) on 2024-08-05 06:53_

---

_Closed by @MichaReiser on 2024-08-05 07:49_

---

_Referenced in [CodSpeedHQ/codspeed-rust#54](../../CodSpeedHQ/codspeed-rust/pulls/54.md) on 2024-09-13 12:46_

---
