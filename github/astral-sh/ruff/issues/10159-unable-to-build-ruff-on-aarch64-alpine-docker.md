---
number: 10159
title: Unable to build ruff on aarch64 alpine docker
type: issue
state: closed
author: matejsp
labels:
  - question
assignees: []
created_at: 2024-02-29T06:03:14Z
updated_at: 2024-04-12T01:47:14Z
url: https://github.com/astral-sh/ruff/issues/10159
synced_at: 2026-01-07T13:12:15-06:00
---

# Unable to build ruff on aarch64 alpine docker

---

_Issue opened by @matejsp on 2024-02-29 06:03_

I am getting undefined reference to `getauxval' using ruff 0.2.2 and rust 1.75.0

I saw mentioning this issue very often and in https://github.com/rust-lang/rust/issues/89626 there is potencial workaround: 
CFLAGS=-mno-outline-atomics but I don't know if same solution applies to ruff and where to put it.


```
d5eeb851975c9e9d.yansi_term.b1b943cb7c038b2f-cgu.0.rcgu.o.rcgu.o" "/tmp/pip-req-build-g44dfv8y/target/release/deps/ruff-40440a78eac19877.yansi_term-d5eeb851975c9e9d.yansi_term.b1b943cb7c038b2f-cgu.1.rcgu.o.rcgu.o" "-Wl,--as-needed" "-L" "/tmp/pip-req-build-g44dfv8y/target/release/deps" "-L" "/tmp/pip-req-build-g44dfv8y/target/release/build/tikv-jemalloc-sys-c62787d9c96f094e/out/build/lib" "-L" "/usr/local/lib/rustlib/aarch64-unknown-linux-musl/lib" "-Wl,-Bstatic" "/tmp/rustc4x3SZs/libtikv_jemalloc_sys-a7374e6d0308fde8.rlib" "-lunwind" "-lc" "/usr/local/lib/rustlib/aarch64-unknown-linux-musl/lib/libcompiler_builtins-38b80b5b9028b8db.rlib" "-Wl,-Bdynamic" "-Wl,--eh-frame-hdr" "-Wl,-z,noexecstack" "-nostartfiles" "-L" "/usr/local/lib/rustlib/aarch64-unknown-linux-musl/lib" "-L" "/usr/local/lib/rustlib/aarch64-unknown-linux-musl/lib/self-contained" "-o" "/tmp/pip-req-build-g44dfv8y/target/release/deps/ruff-40440a78eac19877" "-Wl,--gc-sections" "-static" "-no-pie" "-Wl,-z,relro,-z,now" "-Wl,-O1" "-nodefaultlibs" "-s" "/usr/local/lib/rustlib/aarch64-unknown-linux-musl/lib/self-contained/crtend.o" "/usr/local/lib/rustlib/aarch64-unknown-linux-musl/lib/self-contained/crtn.o"
        = note: /usr/lib/gcc/aarch64-alpine-linux-musl/12.2.1/../../../../aarch64-alpine-linux-musl/bin/ld: /usr/local/lib/rustlib/aarch64-unknown-linux-musl/lib/libcompiler_builtins-38b80b5b9028b8db.rlib(45c91108d938afe8-cpu_model.o): in function `init_have_lse_atomics':
                /cargo/registry/src/index.crates.io-6f17d22bba15001f/compiler_builtins-0.1.103/./lib/builtins/cpu_model.c:1075: undefined reference to `getauxval'
                /usr/lib/gcc/aarch64-alpine-linux-musl/12.2.1/../../../../aarch64-alpine-linux-musl/bin/ld: /usr/local/lib/rustlib/aarch64-unknown-linux-musl/lib/libcompiler_builtins-38b80b5b9028b8db.rlib(45c91108d938afe8-cpu_model.o): in function `init_cpu_features':
                /cargo/registry/src/index.crates.io-6f17d22bba15001f/compiler_builtins-0.1.103/./lib/builtins/cpu_model.c:1373: undefined reference to `getauxval'
                /usr/lib/gcc/aarch64-alpine-linux-musl/12.2.1/../../../../aarch64-alpine-linux-musl/bin/ld: /cargo/registry/src/index.crates.io-6f17d22bba15001f/compiler_builtins-0.1.103/./lib/builtins/cpu_model.c:1374: undefined reference to `getauxval'
                collect2: error: ld returned 1 exit status
        = note: some `extern` functions couldn't be found; some native libraries may need to be installed or have their path specified
        = note: use the `-l` flag to specify native libraries to link
        = note: use the `cargo:rustc-link-lib` directive to specify the native libraries to link with Cargo (see https://doc.rust-lang.org/cargo/reference/build-scripts.html#cargorustc-link-libkindname)
      error: aborting due to previous error
      Error: command ['maturin', 'pep517', 'build-wheel', '-i', '/source/python311/bin/python3.11', '--compatibility', 'off'] returned non-zero exit status 1
      [end of output]
```




---

_Label `question` added by @MichaReiser on 2024-02-29 08:46_

---

_Comment by @MichaReiser on 2024-02-29 08:48_

That's interesting. It's not clear to me why you run into this. We do create musl builds in CI and that works fine, strange. 

Could you share some more details about your setup? That would help us to reproduce the issue.
 What's your docker image? What commands are you running? 

---

_Comment by @matejsp on 2024-02-29 09:25_

I made it working by adding: export CFLAGS=-mno-outline-atomics before calling wheel

will try to reproduce it at home after work because our corpo firewall is messing with ssl certificate ...
```
docker run -it alpine bash
apk add python3-dev py3-pip bash maturin
wget https://files.pythonhosted.org/packages/70/06/b2e9ee5f17dab59476fcb6cc6fdd268e8340d95b7dfc760ed93f4243f16f/ruff-0.2.2.tar.gz
wget --no-check-certificate https://static.rust-lang.org/dist/rust-1.75.0-$(uname -m)-unknown-linux-musl.tar.gz
tar -xzf rust-1.75.0-$(uname -m)-unknown-linux-musl.tar.gz

export CFLAGS=-mno-outline-atomics
pip install wheel --break-system-packages
pip wheel ruff-0.2.2.tar.gz
```

---

_Comment by @zanieb on 2024-03-11 17:35_

@matejsp did you figure more out here / is there anything actionable for us to do?

---

_Comment by @charliermarsh on 2024-04-12 01:47_

Closing for now...

---

_Closed by @charliermarsh on 2024-04-12 01:47_

---

_Referenced in [dgtlmoon/changedetection.io#3188](../../dgtlmoon/changedetection.io/issues/3188.md) on 2025-05-10 08:54_

---
