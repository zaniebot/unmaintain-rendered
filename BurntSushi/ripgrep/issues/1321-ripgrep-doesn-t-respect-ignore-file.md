```yaml
number: 1321
title: "ripgrep doesn't respect .ignore file"
type: issue
state: closed
author: tesuji
labels:
  - duplicate
assignees: []
created_at: 2019-07-11T04:33:51Z
updated_at: 2019-07-11T11:27:49Z
url: https://github.com/BurntSushi/ripgrep/issues/1321
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep doesn't respect .ignore file

---

_@tesuji_

#### What version of ripgrep are you using?

ripgrep 11.0.1 (rev 8ebc113847)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

Build from source

#### What operating system are you using ripgrep on?

Debian GNU/Linux 9.9 (stretch) 4.9.0-9-amd64 #1 SMP Debian 4.9.168-1+deb9u2 (2019-05-13) x86_64 GNU/Linux

#### Describe your question, feature request, or bug.

Ripgrep doesn't respect `.ignore` file. My `.ignore` file:
```
src/test
src/rust-installer
src/doc/nomicon
src/tools/cargo
src/doc/reference
src/doc/book
src/tools/rls
src/tools/clippy
src/tools/rustfmt
src/tools/miri
src/doc/rust-by-example
src/llvm-emscripten
src/stdsimd
src/doc/rustc-guide
src/doc/edition-guide
src/llvm-project
src/doc/embedded-book
```

Project I grepped: https://github.com/rust-lang/rust
In the project root, I used `rg -trust '^\s*extern\s+crate\s+[a-z][_a-zA-Z0-9]*;' src`.
But ripgrep also included search result from `src/test`:
```grep
...
src/tools/rls/rls-analysis/src/test/mod.rs
17:extern crate env_logger;

src/test/run-make-fulldeps/issue-26006/in/time/lib.rs
2:extern crate libc;

src/test/ui/issues/issue-38875/issue-38875.rs
4:extern crate issue_38875_b;

src/test/run-make/thumb-none-qemu/example/src/main.rs
5:extern crate cortex_m;
9:extern crate panic_halt;
...
```

#### If this is a bug, what are the steps to reproduce the behavior?

See above.

#### If this is a bug, what is the actual behavior?

The output is really large, upto 18MiB with only stderr. I don't know if it is 
appropriate to put it in gist.

If you actually need it, I will upload it somewhere later.

#### If this is a bug, what is the expected behavior?

I think ripgrep should ignore `src/test` path.


---

_Comment by @tesuji on 2019-07-11 04:37_

On a second thoughts, I upload ripgrep output with `--debug` option here. Note that this
file only includes stderr from ripgrep.

[rgoutput-stderr-only.zip](https://github.com/BurntSushi/ripgrep/files/3380252/rgoutput-stderr-only.zip)


---

_Comment by @BurntSushi on 2019-07-11 11:27_

Dupe of #829.

---

_Closed by @BurntSushi on 2019-07-11 11:27_

---

_Label `duplicate` added by @BurntSushi on 2019-07-11 11:27_

---
