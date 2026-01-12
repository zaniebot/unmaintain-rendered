```yaml
number: 1917
title: Unable to build ripgrep against musl
type: issue
state: closed
author: beaglesnuf
labels:
  - duplicate
assignees: []
created_at: 2021-06-30T00:10:36Z
updated_at: 2021-06-30T00:27:43Z
url: https://github.com/BurntSushi/ripgrep/issues/1917
synced_at: 2026-01-12T16:13:24Z
```

# Unable to build ripgrep against musl

---

_@beaglesnuf_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
-SIMD -AVX (runtime)

rustc 1.53.0 (53cb7b09b 2021-06-17)

Current install is built against glibc and compiles and runs as expected.

#### How did you install ripgrep?

cargo build --release --target x86_64-unknown-linux-musl

#### What operating system are you using ripgrep on?

Debian sid

#### Describe your bug.

Unable to build ripgrep against musl

#### What are the steps to reproduce the behavior?

rustup override set stable && rustup update stable
rustup target add x86_64-unknown-linux-musl
cargo build --release --target x86_64-unknown-linux-musl --verbose

#### What is the actual behavior?

```
" "/opt/rustup/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/libcompiler_builtins-f3dd73e3317f9c91.rlib" "-Wl,-Bdynamic" "-lpthread" "/opt/rustup/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/self-contained/crtendS.o" "/opt/rustup/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-musl/lib/self-contained/crtn.o"
  = note: /usr/bin/ld: /tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps/rg-412a32416e02af75.rg.6ie25vzd-cgu.3.rcgu.o: in function `<jemallocator::Jemalloc as core::alloc::global::GlobalAlloc>::alloc':
          /opt/rustup/.cargo/bin/registry/src/github.com-1ecc6299db9ec823/jemallocator-0.3.2/src/lib.rs:81: undefined reference to `_rjem_mallocx'
          /usr/bin/ld: /tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps/rg-412a32416e02af75.rg.6ie25vzd-cgu.3.rcgu.o: in function `<jemallocator::Jemalloc as core::alloc::global::GlobalAlloc>::dealloc':
          /opt/rustup/.cargo/bin/registry/src/github.com-1ecc6299db9ec823/jemallocator-0.3.2/src/lib.rs:99: undefined reference to `_rjem_sdallocx'
          /usr/bin/ld: /tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps/rg-412a32416e02af75.rg.6ie25vzd-cgu.3.rcgu.o: in function `<jemallocator::Jemalloc as core::alloc::global::GlobalAlloc>::realloc':
          /opt/rustup/.cargo/bin/registry/src/github.com-1ecc6299db9ec823/jemallocator-0.3.2/src/lib.rs:105: undefined reference to `_rjem_rallocx'
          /usr/bin/ld: /tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps/rg-412a32416e02af75.rg.6ie25vzd-cgu.3.rcgu.o: in function `<jemallocator::Jemalloc as core::alloc::global::GlobalAlloc>::alloc_zeroed':
          /opt/rustup/.cargo/bin/registry/src/github.com-1ecc6299db9ec823/jemallocator-0.3.2/src/lib.rs:88: undefined reference to `_rjem_calloc'
          /usr/bin/ld: /opt/rustup/.cargo/bin/registry/src/github.com-1ecc6299db9ec823/jemallocator-0.3.2/src/lib.rs:91: undefined reference to `_rjem_mallocx'
          collect2: error: ld returned 1 exit status


error: aborting due to previous error

error: could not compile `ripgrep`

Caused by:
  process didn't exit successfully: `rustc --crate-name rg --edition=2018 crates/core/main.rs --error-format=json --json=diagnostic-rendered-ansi --crate-type bin --emit=dep-info,link -C opt-level=3 -C embed-bitcode=no -C debuginfo=1 -C metadata=412a32416e02af75 -C extra-filename=-412a32416e02af75 --out-dir /tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps --target x86_64-unknown-linux-musl -L dependency=/tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps -L dependency=/tmp/build/ripgrep-13.0.0/target/release/deps --extern bstr=/tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps/libbstr-026193b63d713816.rlib --extern clap=/tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps/libclap-2ae0af467c6d6118.rlib --extern grep=/tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps/libgrep-d8178d51d81486b8.rlib --extern ignore=/tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps/libignore-3a64cb0dddc44998.rlib --extern jemallocator=/tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps/libjemallocator-320f796225b77535.rlib --extern lazy_static=/tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps/liblazy_static-89714ca3ed105801.rlib --extern log=/tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps/liblog-caa4e1ddc941ebfc.rlib --extern num_cpus=/tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps/libnum_cpus-7d73ad5632a9aabc.rlib --extern regex=/tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps/libregex-d5ab58f8123935ba.rlib --extern serde_json=/tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps/libserde_json-f799113d95ebef27.rlib --extern termcolor=/tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/deps/libtermcolor-e67652ba2be6bf92.rlib -L native=/tmp/build/ripgrep-13.0.0/target/x86_64-unknown-linux-musl/release/build/jemalloc-sys-5d0a2e66c852cdff/out/build/lib` (exit status: 1)

```

#### What is the expected behavior?

Successful compilation against musl libc.


---

_Comment by @BurntSushi on 2021-06-30 00:27_

You might need to install `musl-tools`.

While the error here is not exactly the same, I suspect this is a duplicate of #1542.

---

_Closed by @BurntSushi on 2021-06-30 00:27_

---

_Label `duplicate` added by @BurntSushi on 2021-06-30 00:27_

---
