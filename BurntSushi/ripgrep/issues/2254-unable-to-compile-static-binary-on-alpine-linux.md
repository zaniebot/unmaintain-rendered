```yaml
number: 2254
title: Unable to compile static binary on Alpine Linux
type: issue
state: closed
author: MrDrMcCoy
labels:
  - question
assignees: []
created_at: 2022-07-07T00:03:25Z
updated_at: 2023-11-24T19:21:01Z
url: https://github.com/BurntSushi/ripgrep/issues/2254
synced_at: 2026-01-12T16:13:24Z
```

# Unable to compile static binary on Alpine Linux

---

_@MrDrMcCoy_

#### What version of ripgrep are you using?

Git master (2022/06/06), unsuccessful build.

#### How did you install ripgrep?

In an `alpine:edge` Docker container, I run:

```
apk --no-cache add build-base git rustup
rustup-init -vy --target x86_64-unknown-linux-musl
git clone --depth=1 https://github.com/BurntSushi/ripgrep.git /build/ripgrep
cd /build/ripgrep
export CFLAGS="-static -fPIC -Os"
export CXXFLAGS="${CFLAGS} -static-libstdc++"
export LDFLAGS='-static -pthread'
export PATH="${PATH}:${HOME}/.cargo/bin"
export RUSTFLAGS='-C target-feature=+crt-static'
export RUST_BACKTRACE="full"
cargo build --release --target 'x86_64-unknown-linux-musl' --features 'pcre2'
```

#### What operating system are you using ripgrep on?

Alpine Linux edge, x86_64.

#### Describe your bug.

When attempting to compile ripgrep statically using the above commands, the build fails with the following message in the stack trace:

```
  /usr/lib/gcc/x86_64-alpine-linux-musl/11.2.1/../../../../x86_64-alpine-linux-musl/bin/ld: /usr/lib/gcc/x86_64-alpine-linux-musl/11.2.1/crtbeginT.o: relocation R_X86_64_32 against hidden symbol `__TMC_END__' can not be used when making a shared object
  /usr/lib/gcc/x86_64-alpine-linux-musl/11.2.1/../../../../x86_64-alpine-linux-musl/bin/ld: failed to set dynamic section sizes: bad value
  collect2: error: ld returned 1 exit status
  make: *** [Makefile:387: lib/libjemalloc.so.2] Error 1
  make: *** Waiting for unfinished jobs....
  thread 'main' panicked at 'command did not execute successfully: "make" "srcroot=../jemalloc/" "-j" "12"
  expected success, got: exit status: 2', /root/.cargo/registry/src/github.com-1ecc6299db9ec823/jemalloc-sys-0.3.2/build.rs:392:9
```

If the above steps are taken without the `RUSTFLAGS='-C target-feature=+crt-static'` variable, cargo builds a dynamic binary linked against musl.

#### What is the expected behavior?

Build should render static binary. Are there any tricks I can use to get this to compile statically on Alpine?


---

_Comment by @BurntSushi on 2022-07-07 00:19_

I don't know anything about Alpine, sorry. Have you tried building a static binary in the same way that CI does it?

Also, your error is in jemalloc, so this doesn't look like a ripgrep issue. You might consider patching out jemalloc from ripgrep and try building that.

---

_Comment by @KEINOS on 2023-10-26 04:40_

@MrDrMcCoy 

How about specifying larger page size as "16" (2**16 = 64k) and set `PCRE2_SYS_STATIC` as "0" to use system `libpcre2`?

```diff
  apk --no-cache add build-base git rustup
  rustup-init -vy --target x86_64-unknown-linux-musl
  git clone --depth=1 https://github.com/BurntSushi/ripgrep.git /build/ripgrep
  cd /build/ripgrep
+ export JEMALLOC_SYS_WITH_LG_PAGE=16
+ export PCRE2_SYS_STATIC=0
  export CFLAGS="-static -fPIC -Os"
  export CXXFLAGS="${CFLAGS} -static-libstdc++"
  export LDFLAGS='-static -pthread'
  export PATH="${PATH}:${HOME}/.cargo/bin"
  export RUSTFLAGS='-C target-feature=+crt-static'
  export RUST_BACKTRACE="full"
  cargo build --release --target 'x86_64-unknown-linux-musl' --features 'pcre2'
```

Here is my Dockerfile that successfully runs `ripgrep` using multistage build. Which the built binary must be static.

```Dockerfile
# -----------------------------------------------------------------------------
#  First stage (build)
# -----------------------------------------------------------------------------
FROM rust:alpine AS rg-build

WORKDIR /root

# Install dependencies
RUN apk add --no-cache \
    alpine-sdk \
    build-base \
    asciidoc \
    cargo \
    cargo-audit \
    pcre2-dev \
    xz

# Export environment variables
ENV \
    # use system libpcre2
    PCRE2_SYS_STATIC=0 \
    # 2**16 = 64k
    JEMALLOC_SYS_WITH_LG_PAGE=16 \
    # Static flags
    CFLAGS="-static -fPIC -Os" \
    CXXFLAGS="${CFLAGS} -static-libstdc++" \
    LDFLAGS='-static -pthread' \
    PATH="${PATH}:${HOME}/.cargo/bin" \
    RUSTFLAGS='-C target-feature=+crt-static' \
    RUST_BACKTRACE="full"

# Build
RUN \
    git clone --depth=1 https://github.com/BurntSushi/ripgrep.git && \
    cd ripgrep && \
    cargo build --release \
        --target 'x86_64-unknown-linux-musl' --features 'pcre2' && \
    # Smoke test
    /root/ripgrep/target/x86_64-unknown-linux-musl/release/rg --version

# -----------------------------------------------------------------------------
#  Second stage (copy)
# -----------------------------------------------------------------------------
FROM alpine AS ripgrep

# Copy the ripgrep binary under /usr/local/bin
COPY --from=rg-build \
    /root/ripgrep/target/x86_64-unknown-linux-musl/release/rg \
    /usr/local/bin

# Smoke test
RUN rg --version
```

- Reference:
  - [APKBUILD](https://git.alpinelinux.org/aports/tree/community/ripgrep/APKBUILD) | ripgrep | aports @ git.alpinelinux.org
- Tested env:
  - Docker version 24.0.6, build ed223bc
    - Guest OS: Alpine Linux 3.18.4
  - Host OS: macOS 12.7, build 21G816

---

_Comment by @BurntSushi on 2023-11-24 19:20_

I'm going to close this because I don't think there is anything actionable here on my end.

---

_Closed by @BurntSushi on 2023-11-24 19:20_

---

_Label `question` added by @BurntSushi on 2023-11-24 19:21_

---
