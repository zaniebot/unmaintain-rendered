```yaml
number: 16483
title: "Use std `home_dir` instead of `home` crate"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/use-std-home
created_at: 2025-10-28T12:49:40Z
updated_at: 2025-10-29T17:49:10Z
url: https://github.com/astral-sh/uv/pull/16483
synced_at: 2026-01-12T16:12:17Z
```

# Use std `home_dir` instead of `home` crate

---

_@konstin_

The `home_dir` function in std was deprecated for some years for reading `HOME` on Windows. It has recently been fixed and undeprecated: https://github.com/rust-lang/rust/pull/132515

Conversely, the Cargo maintainers want us to move away from the home crate (https://github.com/rust-lang/cargo/tree/master/crates/home):

> Note: This has been fixed in Rust 1.85 to no longer use the HOME environment variable on Windows. If you are still using this crate for the purpose of getting a home directory, you are strongly encouraged to switch to using the standard library's home_dir instead. It is planned to have the deprecation notice removed in 1.87.
>
> This crate further provides two functions, cargo_home and rustup_home, which are the canonical way to determine the location that Cargo and rustup store their data.
>
> See rust-lang/rust#43321.
>
> > This crate is maintained by the Cargo team, primarily for use by Cargo and Rustup and not intended for external use. This crate may make major changes to its APIs or be deprecated without warning.

When https://github.com/lunacookies/etcetera/pull/36 merges, we can remove the home crate from our dependency tree.




---

_Review requested from @geofft by @konstin on 2025-10-28 12:49_

---

_Label `internal` added by @konstin on 2025-10-28 12:49_

---

_Review requested from @BurntSushi by @konstin on 2025-10-28 12:49_

---

_@BurntSushi approved on 2025-10-28 13:09_

w00t!

---

_Comment by @utkarshgupta137 on 2025-10-28 13:30_

Sorry for the delay. I've released v0.11 with the home crate removed.

---

_Comment by @konstin on 2025-10-28 13:31_

Thank you for the quick response and the release!

---

_@charliermarsh approved on 2025-10-29 16:06_

---

_Merged by @konstin on 2025-10-29 17:49_

---

_Closed by @konstin on 2025-10-29 17:49_

---

_Branch deleted on 2025-10-29 17:49_

---
