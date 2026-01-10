```yaml
number: 5955
title: "cargo: straighten out LTO configuration"
type: pull_request
state: merged
author: BurntSushi
labels:
  - releases
assignees: []
merged: true
base: main
head: ag/undo-lto-for-profiling
created_at: 2024-08-09T13:28:14Z
updated_at: 2024-08-09T13:34:54Z
url: https://github.com/astral-sh/uv/pull/5955
synced_at: 2026-01-10T13:31:54Z
```

# cargo: straighten out LTO configuration

---

_Pull request opened by @BurntSushi on 2024-08-09 13:28_

This PR tweaks the change made in #5904 so that the `profiling` Cargo
profile does _not_ have LTO enabled. With LTO enabled, compile times
even after just doing a `touch crates/uv/src/bin/uv.rs` are devastating:

    $ cargo b --profile profiling -p uv
       Compiling uv-cli v0.0.1 (/home/andrew/astral/uv/crates/uv-cli)
       Compiling uv v0.2.34 (/home/andrew/astral/uv/crates/uv)
        Finished `profiling` profile [optimized + debuginfo] target(s) in 3m 47s

Even with `lto = "thin"`, compile times are not great, but an
improvement:

    $ cargo b --profile profiling -p uv
       Compiling uv v0.2.34 (/home/andrew/astral/uv/crates/uv)
        Finished `profiling` profile [optimized + debuginfo] target(s) in 53.98s

But our original configuration for `profiling`, prior to #5904, was with
LTO completely disabled:

    $ cargo b --profile profiling -p uv
       Compiling uv v0.2.34 (/home/andrew/astral/uv/crates/uv)
        Finished `profiling` profile [optimized + debuginfo] target(s) in 30.09s

This gives reasonable-ish compile times, although I still want them to
be better.

This setup does risk that we are measuring something in benchmarks that
we are shipping, but in order to make those two the same, we'd either
need to make compile times way worse for development, or take a hit
to binary size and a slight hit to runtime performance in our release
builds. I would weakly prefer that we accept the hit to runtime
performance and binary size in order to bring our measurements in line
with what we ship, but I _strongly_ feel that we should not have compile
times exceeding minutes for development. When doing performance testing,
long compile times, for me anyway, break "flow" state.

A confounding factor here was that #5904 enabled LTO for the `release`
profile, but the `dist` profile (used by `cargo dist`) was still setting
it to `lto = "thin"`. However, because of shenanigans in our release
pipeline, we we actually using the `release` profile for binaries we
ship. This PR does not make any changes here other than to remove `lto =
"thin"` from the `dist` profile to make the fact that they are the same
a bit clearer.

cc @davfsa


---

_@konstin approved on 2024-08-09 13:30_

I mainly want lto for the 15% - 20% compressed size reduction, i'm fine accepting a minor divergence between profiling and release for that

---

_Review comment by @BurntSushi on `Cargo.toml`:208 on 2024-08-09 13:30_

[`"fat"` and `true` are the same](https://doc.rust-lang.org/cargo/reference/profiles.html#lto), but `fat` is more descriptive. The boolean value is a holdover from the early days when `lto` was a strict binary option.

---

_@charliermarsh approved on 2024-08-09 13:31_

---

_Comment by @charliermarsh on 2024-08-09 13:31_

Thanks Andrew!

---

_Review comment by @BurntSushi on `Cargo.toml`:249 on 2024-08-09 13:31_

[`"full"` is equivalent to `true`](https://doc.rust-lang.org/cargo/reference/profiles.html#debug), but `full` is more descriptive.

---

_@BurntSushi reviewed on 2024-08-09 13:32_

---

_Label `release` added by @charliermarsh on 2024-08-09 13:33_

---

_Merged by @BurntSushi on 2024-08-09 13:34_

---

_Closed by @BurntSushi on 2024-08-09 13:34_

---

_Branch deleted on 2024-08-09 13:34_

---
