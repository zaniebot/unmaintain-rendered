```yaml
number: 2516
title: Compile error with cargo +nightly and features
type: issue
state: closed
author: gcflymoto
labels: []
assignees: []
created_at: 2023-05-13T20:59:10Z
updated_at: 2023-10-29T01:42:37Z
url: https://github.com/BurntSushi/ripgrep/issues/2516
synced_at: 2026-01-12T16:13:24Z
```

# Compile error with cargo +nightly and features

---

_@gcflymoto_

This is an FYI since this issue is with the grep crate.

#### What version of ripgrep are you using?
13.0.0
Replace this text with the output of `rg --version`.
Cannot compile it
#### How did you install ripgrep?
cargo +nightly install ripgrep --features 'pcre2 simd-accel' --git https://github.com/BurntSushi/ripgrep

#### What operating system are you using ripgrep on?
SUSE12 linux

#### Describe your bug.
cannot compile

#### What are the steps to reproduce the behavior?
cargo +nightly install ripgrep --features 'pcre2 simd-accel' --git https://github.com/BurntSushi/ripgrep

#### What is the actual behavior?
compile error

If the output is small, put it in code fences:
```
error: missing documentation for an extern crate
  --> crates/grep/src/lib.rs:17:1
   |
17 | pub extern crate grep_cli as cli;
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
note: the lint level is defined here
  --> crates/grep/src/lib.rs:15:9
   |
15 | #![deny(missing_docs)]
   |         ^^^^^^^^^^^^

error: missing documentation for an extern crate
  --> crates/grep/src/lib.rs:18:1
   |
18 | pub extern crate grep_matcher as matcher;
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

error: missing documentation for an extern crate
  --> crates/grep/src/lib.rs:20:1
   |
20 | pub extern crate grep_pcre2 as pcre2;
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

error: missing documentation for an extern crate
  --> crates/grep/src/lib.rs:21:1
   |
21 | pub extern crate grep_printer as printer;
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

error: missing documentation for an extern crate
  --> crates/grep/src/lib.rs:22:1
   |
22 | pub extern crate grep_regex as regex;
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

error: missing documentation for an extern crate
  --> crates/grep/src/lib.rs:23:1
   |
23 | pub extern crate grep_searcher as searcher;
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

error: could not compile `grep` (lib) due to 6 previous errors
warning: build failed, waiting for other jobs to finish...
error: failed to compile `ripgrep v13.0.0 (https://github.com/BurntSushi/ripgrep#04154485)`, intermediate artifacts can be found at `/tmp/cargo-installQNU2vs`
```

#### What is the expected behavior?
compile
What do you think ripgrep should have done?
suggesting github actions to test compile

---

_Closed by @BurntSushi on 2023-05-16 17:16_

---

_Comment by @BurntSushi on 2023-05-16 17:17_

Interesting, thanks for the report! It looks like a newer version of Rust does a better job of enforcing the `missing_docs` lint. In this case, I just decided to remove the lint, since `grep` is just a facade that re-exports other crates.

I've published `grep 0.2.12` to crates.io and also updated the lock file.

---

_Comment by @gcflymoto on 2023-05-16 18:35_

Thanks it's fixed!

---

_Comment by @gcflymoto on 2023-10-29 01:42_

BTW. With recent builds, pcre2-sys may not compile:

warning: pcre2-sys@0.2.6: upstream/src/pcre2_compile.c: In function 342200230compile_branch342200231:
warning: pcre2-sys@0.2.6: upstream/src/pcre2_compile.c:5859:13: error: 342200230for342200231 loop initial declarations are only allowed in C99 mode
warning: pcre2-sys@0.2.6:              for (int i = 0; i < 32; i++) pbits[i] |= cbits[(int)i + taboffset];
warning: pcre2-sys@0.2.6:              ^
..
error: failed to run custom build command for `pcre2-sys v0.2.6`

I forced rust to use clang 16.x version for 'cc' and that resolved it.

---
