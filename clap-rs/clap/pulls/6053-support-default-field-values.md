---
number: 6053
title: Support default field values
type: pull_request
state: open
author: DJMcNab
labels: []
assignees: []
draft: true
base: master
head: default_field_values
created_at: 2025-06-30T16:34:37Z
updated_at: 2025-06-30T16:34:37Z
url: https://github.com/clap-rs/clap/pull/6053
synced_at: 2026-01-10T01:28:26Z
---

# Support default field values

---

_Pull request opened by @DJMcNab on 2025-06-30 16:34_

This uses my fork of syn defined at https://github.com/dtolnay/syn/pull/1870, and is intended to validate that. This PR is not necessarily intended to merged/reviewed, as things stand. In particular, if the syn PR is never merged, this won't be the right solution.

* RFC: https://github.com/rust-lang/rfcs/pull/3681
* Tracking issue: https://github.com/rust-lang/rust/issues/132162
* Feature gate: `#![feature(default_field_values)]`

```rust
#[derive(Parser)]
struct Opt {
    arg: i32 = 42,
    //       ^^^^
}
```

Some notes:
- Supporting default field values is frustratingly involved, because of how the support has to be hacked into syn 2.0. See the suggestion in https://github.com/dtolnay/syn/pull/1851#discussion_r1983947505 of the only way to move this forward. This will need to filter through the ecosystem if approved, which this PR is part of.
- Testing the use of this feature in CI is hard because it requires nightly
- There are some pretty nasty hacks in here; obviously those can be cleaned up before landing.

The test can be ran with:
```sh
RUSTFLAGS="--cfg=nightly" cargo +nightly test --test default_field_values --features derive --features help --features usage
```

---
