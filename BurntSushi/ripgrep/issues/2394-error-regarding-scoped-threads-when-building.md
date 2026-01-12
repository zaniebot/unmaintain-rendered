```yaml
number: 2394
title: "Error regarding \"scoped_threads\" when building ignore 0.4.19"
type: issue
state: closed
author: luciodaou
labels:
  - invalid
assignees: []
created_at: 2023-01-08T02:51:22Z
updated_at: 2023-01-08T03:02:21Z
url: https://github.com/BurntSushi/ripgrep/issues/2394
synced_at: 2026-01-12T16:13:24Z
```

# Error regarding "scoped_threads" when building ignore 0.4.19

---

_@luciodaou_

#### What version of ripgrep are you using?

Not applicable.

#### How did you install ripgrep?

Trying to install through `cargo install rip-grep`

#### What operating system are you using ripgrep on?

aarch64-unknown-linux-gnu  (Android 13 termux-proot-distro Ubuntu 22.04.1 LTS with Rust 1.61 and LLVM 14.0.0)
aarch64-unknown-linux-gnu  (Android 13 termux-proot-distro ArchLinuxArm with Rust 1.66 and LLVM 14.0.6)
x86_64-unknown-linux-gnu (Windows11 WSL2 Ubuntu 22.04.1 LTS with Rust 1.61 and LLVM 14.0.0)

#### Describe your bug.

When trying to install `ripgrep` or `fd-find` using cargo, 3 errors comes up regarding "error[E0658]: use of unstable library feature 'scoped_threads' " :

![image](https://user-images.githubusercontent.com/53634214/211178216-91be8086-8778-495e-865f-33758bce1df1.png)

It doesn't happen on x86_64-pc-windows-msvc, Windows 11 with Rust 1.66 and LLVM 15.0.2.

#### What are the steps to reproduce the behavior?
`cargo install ripgrep`
`cargo install fd-find`
or any other crate that needs `ignore`.

#### What is the actual behavior?
Check image above.

#### What is the expected behavior?
Normal installation of `rip-grep`, `fd-find` and any other crate that depends on `ignore`.

---

_Comment by @BurntSushi on 2023-01-08 03:02_

`cargo install` counter-intuitively does not use the lock file, so it tried to use the latest dependencies of everything. Which is usually not a problem, but current master (and some recently published crates like `ignore`) require a newer Rust than Rust 1.61. In accordance with the build instructions: https://github.com/BurntSushi/ripgrep/#building

If you want `cargo install` to work, then either find a way to install a newer Rust (via `rustup`) or `cargo install --locked ripgrep` should work.

---

_Closed by @BurntSushi on 2023-01-08 03:02_

---

_Label `invalid` added by @BurntSushi on 2023-01-08 03:02_

---
