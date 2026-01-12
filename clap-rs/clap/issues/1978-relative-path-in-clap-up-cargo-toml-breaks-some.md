```yaml
number: 1978
title: "Relative path in `clap_up/Cargo.toml` breaks some tools"
type: issue
state: closed
author: fatemender
labels:
  - C-bug
assignees: []
created_at: 2020-06-19T12:10:21Z
updated_at: 2020-09-26T07:06:43Z
url: https://github.com/clap-rs/clap/issues/1978
synced_at: 2026-01-12T16:14:12Z
```

# Relative path in `clap_up/Cargo.toml` breaks some tools

---

_@fatemender_

In `clap_up/Cargo.toml` there is a dependency on `cargo-up` expected to be locally available.

https://github.com/clap-rs/clap/blob/81457178fa7e055775867ca659b37798b5ae9584/clap_up/Cargo.toml#L31 

While this is not a problem for most workflows because this crate is not part of the workspace, some tools are affected.  E.g. `cargo-outdated` fails for anything that depends on `clap` now when doing its recursive sweep of manifests.  While this may be also considered a bug in `cargo-outdated`, I still don't believe having relative paths that go above the repository root is a great idea.

---

_Label `T: bug` added by @fatemender on 2020-06-19 12:10_

---

_Comment by @pksunkara on 2020-06-19 12:11_

It is still in development. Will be fixed before release.

---

_Added to milestone `3.0` by @pksunkara on 2020-06-19 12:11_

---

_Comment by @pksunkara on 2020-06-19 12:12_

`cargo-outdated` should definitely not be doing recursive sweeps. It should be relying on `cargo_metadata`.

---

_Label `P1: urgent` added by @CreepySkeleton on 2020-06-25 22:24_

---

_Comment by @pksunkara on 2020-09-26 07:06_

This has been fixed by #2114 

---

_Closed by @pksunkara on 2020-09-26 07:06_

---
