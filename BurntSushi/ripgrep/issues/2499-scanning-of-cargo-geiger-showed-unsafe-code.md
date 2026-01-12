```yaml
number: 2499
title: "Scanning of \"cargo-geiger\" showed unsafe code "
type: issue
state: closed
author: vidhyasasi
labels:
  - invalid
assignees: []
created_at: 2023-04-27T07:56:47Z
updated_at: 2023-04-27T12:09:41Z
url: https://github.com/BurntSushi/ripgrep/issues/2499
synced_at: 2026-01-12T16:13:24Z
```

# Scanning of "cargo-geiger" showed unsafe code 

---

_@vidhyasasi_

#### What version of ripgrep are you using?

ripgrep 13.0.0 (rev a7ae9e4043)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

using source binary files

#### What operating system are you using ripgrep on?

Debian 11

#### Describe your bug.

I scanned the tool "cargo-geiger"(https://github.com/rust-secure-code/cargo-geiger) on ripgrep to find the usage of unsafe code in the package. It showed the statistics that a lot of code in the package has unsafe code. The screenshot of result is given below:
<img width="580" alt="Screenshot 2023-04-27 at 09 44 09" src="https://user-images.githubusercontent.com/64778227/234796849-5d59078e-2066-4562-a0d3-3719a3f44b45.png">


#### What are the steps to reproduce the behavior?

Install cargo-geiger using cargo install cargo-geiger

1.Navigate to the directory same as the Cargo.toml 
2. run cargo geiger


#### What is the expected behavior?

Expected behavior is showing all the files used by ripgrep are safe!


---

_Comment by @ltrzesniewski on 2023-04-27 10:01_

I may be assuming a bit much here, but you don't seem to have any Rust repository, so maybe you're unaware that [`unsafe` is a legitimate Rust keyword](https://doc.rust-lang.org/std/keyword.unsafe.html). It doesn't *automatically* make the code insecure, it only enables a few additional features which prevent the Rust compiler from automatically proving that the code is 100% safe. It indicates a region of code which needs careful manual review.

The Rust standard library contains unsafe code *everywhere*, so requiring ripgrep to use only safe code is a bit unrealisitc. 🙂


---

_Comment by @BurntSushi on 2023-04-27 12:09_

> Expected behavior is showing all the files used by ripgrep are safe!

I can't tell whether this is a joke, you're under-informed or if you're informed and seriously hold this position.

My guess is that you're under-informed. If so, I don't really know where your confusion lies, so I'll link you to the [Rustonomicon](https://doc.rust-lang.org/nomicon/).

---

_Closed by @BurntSushi on 2023-04-27 12:09_

---

_Label `invalid` added by @BurntSushi on 2023-04-27 12:09_

---
