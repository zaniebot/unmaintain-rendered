```yaml
number: 9618
title: Upgrade to Rust 1.83 is blocked by powerpc64le binary builds
type: issue
state: open
author: zanieb
labels:
  - releases
assignees: []
created_at: 2024-12-03T19:23:22Z
updated_at: 2025-01-08T13:33:32Z
url: https://github.com/astral-sh/uv/issues/9618
synced_at: 2026-01-12T15:59:54Z
```

# Upgrade to Rust 1.83 is blocked by powerpc64le binary builds

---

_@zanieb_

Maturin fails with `toolchain '1.83-x86_64-unknown-linux-gnu' does not support target 'powerpc64le-unknown-linux-musl'` during `rustup target add ${target}`. Alternatively, without specifying the a `1.83` nightly version the build fails because the image toolchain is still `1.82` which is no longer supported by our crates.

I presume this requires updated rust-musl-cross images?

I tried removing both removing `nightly` and using a newer version of nightly.

See 

- #9617
- https://github.com/astral-sh/uv/pull/9615 

---

_Label `releases` added by @zanieb on 2024-12-03 19:23_

---

_Comment by @messense on 2024-12-04 03:26_

Unfortunately rust-musl-cross [ppc64le build is broken](https://github.com/rust-cross/rust-musl-cross/actions/runs/12151152170/job/33885259180) at the moment due to https://github.com/japaric/xargo/issues/347, you could try use `-Zbuild-std` instead.

---

_Comment by @charliermarsh on 2024-12-04 03:35_

Thank you @messense!

---

_Comment by @charliermarsh on 2025-01-06 22:00_

As an update: we removed the ppc64le musl builds to get past this.

---

_Comment by @messense on 2025-01-08 08:20_

Update: I think it's available now in rust-musl-cross now that Rust nightly has it as a Tier 2 target: https://github.com/rust-cross/rust-musl-cross/actions/runs/12662111992/job/35286479909

---

_Comment by @charliermarsh on 2025-01-08 13:33_

Thanks, I’ll try re-adding.

---
