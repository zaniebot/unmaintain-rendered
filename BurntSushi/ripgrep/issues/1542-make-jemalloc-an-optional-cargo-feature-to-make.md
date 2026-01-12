```yaml
number: 1542
title: make jemalloc an optional Cargo feature to make musl builds easier
type: issue
state: closed
author: moderation
labels:
  - enhancement
  - wontfix
assignees: []
created_at: 2020-04-05T18:54:39Z
updated_at: 2021-06-16T13:22:32Z
url: https://github.com/BurntSushi/ripgrep/issues/1542
synced_at: 2026-01-12T16:13:23Z
```

# make jemalloc an optional Cargo feature to make musl builds easier

---

_@moderation_

#### What version of ripgrep are you using?

non-musl version built from master

```
rg --version
ripgrep 12.0.1 (rev 49de7b119c)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Compile from source 

#### What operating system are you using ripgrep on?

`Linux moderation-x1 5.3.0-40-generic #32-Ubuntu SMP Fri Jan 31 20:24:34 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux`

```
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=19.10
DISTRIB_CODENAME=eoan
DISTRIB_DESCRIPTION="Ubuntu 19.10"
```

#### Describe your question, feature request, or bug.

Trying to build a musl binary from source following instructions at https://github.com/BurntSushi/ripgrep#building

#### If this is a bug, what are the steps to reproduce the behavior?

`cargo build --release --verbose --target x86_64-unknown-linux-musl`

I _think_ have the required Rust targets an OS packages

```
rustup target list | rg installed
x86_64-unknown-linux-gnu (installed)
x86_64-unknown-linux-musl (installed)
```

```
dpkg --get-selections | rg musl
musl:amd64                                      install
musl-dev:amd64                                  install
musl-tools                                      install
```

```
rustc --version
rustc 1.42.0 (b8cedc004 2020-03-09)
```

#### If this is a bug, what is the actual behavior?

```
Running `rustc --crate-name rg --edition=2018 crates/core/main.rs --error-format=json --json=diagnostic-rendered-ansi --crate-type bin --emit=dep-info,link -C opt-level=3 -C debuginfo=1 -C metadata=ec4ab00b248f1489 -C extra-filename=-ec4ab00b248f1489 --out-dir ~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps --target x86_64-unknown-linux-musl -L dependency=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps -L dependency=~/Library/rust/ripgrep/target/release/deps --extern bstr=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libbstr-0662598af7cca08b.rlib --extern clap=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libclap-6011a3ecf35a177d.rlib --extern grep=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libgrep-6fc14b1a78706d86.rlib --extern ignore=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libignore-bce5d9bdf93d81fa.rlib --extern jemallocator=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libjemallocator-5e79b37a74845db7.rlib --extern lazy_static=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/liblazy_static-9617dd9b9616e9e7.rlib --extern log=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/liblog-6d2d031510ac25b2.rlib --extern num_cpus=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libnum_cpus-d5c9f8c1bb381449.rlib --extern regex=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libregex-b070a2602945e877.rlib --extern serde_json=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libserde_json-1ee383f3f2df211c.rlib --extern termcolor=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libtermcolor-4ebca4a0b76073be.rlib -C target-cpu=native -L native=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/build/jemalloc-sys-4a0f093f66a534eb/out/build/lib`
error: linking with `cc` failed: exit code: 1
  |
  = note: "cc" "-Wl,--as-needed" "-Wl,-z,noexecstack" "-Wl,--eh-frame-hdr" "-m64" "-nostdlib" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/crt1.o" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/crti.o" "-L" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.0.rcgu.o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.1.rcgu.o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.10.rcgu.o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.11.rcgu.o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.12.rcgu.o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.13.rcgu.o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.14.rcgu.o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.15.rcgu.o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.2.rcgu.o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.3.rcgu.o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.4.rcgu.o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.5.rcgu.o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.6.rcgu.o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.7.rcgu.o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.8.rcgu.o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.rg.72arghw2-cgu.9.rcgu.o" "-o" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/rg-ec4ab00b248f1489.546abslvap0scla1.rcgu.o" "-Wl,--gc-sections" "-no-pie" "-Wl,-zrelro" "-Wl,-znow" "-Wl,-O1" "-nodefaultlibs" "-L" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps" "-L" "~/Library/rust/ripgrep/target/release/deps" "-L" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/build/jemalloc-sys-4a0f093f66a534eb/out/build/lib" "-L" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib" "-Wl,-Bstatic" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libjemallocator-5e79b37a74845db7.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libjemalloc_sys-a17cb992ff1a396c.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libnum_cpus-d5c9f8c1bb381449.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libgrep-6fc14b1a78706d86.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libgrep_regex-aceb264ce4fcd271.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libgrep_printer-1e6e4810bb3d5757.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libserde_json-1ee383f3f2df211c.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libryu-721a91e20b40f1de.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libitoa-0f8ddd5cb693e84b.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libserde-0ebbbce1e004f975.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libgrep_searcher-89a1b57ad055b188.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libmemmap-52d20d9580a2a750.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libencoding_rs_io-fa28242b62d919f0.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libencoding_rs-6bebacc26e931a43.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libbytecount-93501284051bdcec.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libbase64-ba0f1c7d783482dc.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libgrep_matcher-1831c5036703d182.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libgrep_cli-1187989060083956.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libtermcolor-4ebca4a0b76073be.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libatty-e74ea94d290f9004.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/liblibc-3353131a251f4c31.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libclap-6011a3ecf35a177d.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libtextwrap-06aef39074f37e08.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libunicode_width-83e1d761e5ad8450.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libstrsim-2c95ea1b6a5e68b8.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libbitflags-f4fd3bf3f15654b2.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libignore-bce5d9bdf93d81fa.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libwalkdir-44002ba0c9cc519d.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libsame_file-05917483918d6478.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libglobset-d52ed1563518bc33.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libregex-b070a2602945e877.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libthread_local-0695194ea504768a.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libregex_syntax-c1ec04075d6937d0.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/liblog-6d2d031510ac25b2.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libfnv-2c65e6419b1a00d5.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libbstr-0662598af7cca08b.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libregex_automata-abcc5a82cb510d19.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libbyteorder-c48440e9d78dd40c.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libaho_corasick-5afa16b5a565a989.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libmemchr-65a2717f68fab6b9.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libcrossbeam_channel-cb15e054f38dab2a.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libmaybe_uninit-8de56a1c1827bedc.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libcrossbeam_utils-dcd9c9cfc3405c53.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/liblazy_static-9617dd9b9616e9e7.rlib" "~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libcfg_if-35887fa27db474e8.rlib" "-Wl,--start-group" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/libstd-e5539170812218e5.rlib" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/libpanic_unwind-b1343649071ea3b8.rlib" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/libhashbrown-6739199638468e73.rlib" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/librustc_std_workspace_alloc-4c8c973d9a6b5ae6.rlib" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/libbacktrace-a44399c11ae501b4.rlib" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/libbacktrace_sys-9a97763b2b5e5e63.rlib" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/librustc_demangle-f623dd3eb180c80c.rlib" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/libunwind-984beb1d09763686.rlib" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/libcfg_if-6ff9ed4a29146a3a.rlib" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/liblibc-6c4492b949101b15.rlib" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/liballoc-14f40be876bd9d71.rlib" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/librustc_std_workspace_core-9fb39f7b7913489a.rlib" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/libcore-1357ee741b52e86d.rlib" "-Wl,--end-group" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/libcompiler_builtins-c9fca8d265033410.rlib" "-Wl,-Bdynamic" "-lpthread" "-static" "~/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/crtn.o"
  = note: /usr/bin/ld: ~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libjemalloc_sys-a17cb992ff1a396c.rlib(jemalloc.pic.o): in function `jemalloc_secure_getenv':
          ~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/build/jemalloc-sys-4a0f093f66a534eb/out/build/../jemalloc/src/jemalloc.c:700: undefined reference to `secure_getenv'
          collect2: error: ld returned 1 exit status


error: aborting due to previous error

error: could not compile `ripgrep`.

Caused by:
  process didn't exit successfully: `rustc --crate-name rg --edition=2018 crates/core/main.rs --error-format=json --json=diagnostic-rendered-ansi --crate-type bin --emit=dep-info,link -C opt-level=3 -C debuginfo=1 -C metadata=ec4ab00b248f1489 -C extra-filename=-ec4ab00b248f1489 --out-dir ~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps --target x86_64-unknown-linux-musl -L dependency=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps -L dependency=~/Library/rust/ripgrep/target/release/deps --extern bstr=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libbstr-0662598af7cca08b.rlib --extern clap=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libclap-6011a3ecf35a177d.rlib --extern grep=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libgrep-6fc14b1a78706d86.rlib --extern ignore=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libignore-bce5d9bdf93d81fa.rlib --extern jemallocator=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libjemallocator-5e79b37a74845db7.rlib --extern lazy_static=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/liblazy_static-9617dd9b9616e9e7.rlib --extern log=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/liblog-6d2d031510ac25b2.rlib --extern num_cpus=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libnum_cpus-d5c9f8c1bb381449.rlib --extern regex=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libregex-b070a2602945e877.rlib --extern serde_json=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libserde_json-1ee383f3f2df211c.rlib --extern termcolor=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/deps/libtermcolor-4ebca4a0b76073be.rlib -C target-cpu=native -L native=~/Library/rust/ripgrep/target/x86_64-unknown-linux-musl/release/build/jemalloc-sys-4a0f093f66a534eb/out/build/lib` (exit code: 1)
```

#### If this is a bug, what is the expected behavior?

Successful build


---

_Comment by @BurntSushi on 2020-04-05 19:07_

Not sure what the problem is, sorry. I can indeed reproduce it on my system. I've had problems compiling with musl and jemalloc in the past: https://github.com/gnzlbg/jemallocator/issues/124

I think you have two immediate paths in front of you:

* Use [`cross`](https://github.com/rust-embedded/cross/).
* Patch ripgrep to stop using jemalloc and always use the system allocator (musl's allocator in this case). You can do that by removing https://github.com/BurntSushi/ripgrep/blob/49de7b119c95eacfdc2b15aaee3c846710776b2c/crates/core/main.rs#L42-L44 and https://github.com/BurntSushi/ripgrep/blob/49de7b119c95eacfdc2b15aaee3c846710776b2c/Cargo.toml#L64-L65.

Using `cross` is very easy:

```
$ cargo install cross
$ cross build --release --target x86_64-unknown-linux-musl
```

It does require Docker though. The advantage is that it correctly sets up your cross compilation toolchain, which I guess jemalloc is pretty picky about. You could instead look at what is being installed in Cross's docker image and then re-create that on your box. That would probably be the most "correct" solution in that you wouldn't need Docker and you wouldn't sacrifice performance. (ripgrep is slower when using musl's allocator.)

It might be a good idea to make jemalloc a Cargo feature so that it's easier to disable for cases like these.

---

_Label `enhancement` added by @BurntSushi on 2020-04-05 19:07_

---

_Renamed from "musl build failing" to "make jemalloc an optional Cargo feature to make musl builds easier" by @BurntSushi on 2020-04-05 19:08_

---

_Comment by @moderation on 2020-04-05 20:18_

Thanks for the detailed response. Will definitely take a look at `cross`.

---

_Comment by @BurntSushi on 2020-05-09 02:34_

So @jonstites actually put in the work to do this in #1569, but echoing [my comment on that PR](https://github.com/BurntSushi/ripgrep/pull/1569#issuecomment-626091627), I think there's probably not a good way to do this right now with Cargo's existing feature set. I think if folks really want to build ripgrep with musl without jemalloc, then they are advised to just patch ripgrep for now.

---

_Closed by @BurntSushi on 2020-05-09 02:34_

---

_Label `wontfix` added by @BurntSushi on 2020-05-09 02:34_

---

_Comment by @emilheunecke on 2021-06-16 13:22_

for reference, I succeeded building a musl target on ubuntu 20.04.2 by installing musl-tools first

---
