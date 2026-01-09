---
number: 8974
title: "docker build hangs, can't build image for uv locally"
type: issue
state: closed
author: shaneikennedy
labels:
  - question
assignees: []
created_at: 2024-11-09T15:40:30Z
updated_at: 2024-11-09T20:21:37Z
url: https://github.com/astral-sh/uv/issues/8974
synced_at: 2026-01-07T13:12:18-06:00
---

# docker build hangs, can't build image for uv locally

---

_Issue opened by @shaneikennedy on 2024-11-09 15:40_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

```
uv git:main*
❯ docker build . -t myuv --load
[+] Building 992.6s (16/18)                                    docker:desktop-linux
 => [internal] load build definition from Dockerfile                           0.0s
 => => transferring dockerfile: 1.54kB                                         0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest               1.8s
 => [internal] load .dockerignore                                              0.0s
 => => transferring context: 2B                                                0.0s
 => [ 1/14] FROM docker.io/library/ubuntu:latest@sha256:99c35190e22d294cdace2  0.0s
 => [internal] load build context                                              0.1s
 => => transferring context: 2.08MB                                            0.1s
 => CACHED [ 2/14] WORKDIR /root                                               0.0s
 => CACHED [ 3/14] RUN apt update   && apt install -y --no-install-recommends  0.0s
 => CACHED [ 4/14] RUN python3 -m venv /root/.venv                             0.0s
 => CACHED [ 5/14] RUN .venv/bin/pip install cargo-zigbuild                    0.0s
 => CACHED [ 6/14] RUN case "linux/arm64" in   "linux/arm64") echo "aarch64-u  0.0s
 => CACHED [ 7/14] COPY rust-toolchain.toml rust-toolchain.toml                0.0s
 => CACHED [ 8/14] RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup  0.0s
 => CACHED [ 9/14] RUN rustup target add $(cat rust_target.txt)                0.0s
 => [10/14] COPY crates crates                                                 0.2s
 => [11/14] COPY ./Cargo.toml Cargo.toml                                       0.0s
 => [12/14] COPY ./Cargo.lock Cargo.lock                                       0.0s
 => [13/14] RUN cargo zigbuild --bin uv --bin uvx --target $(cat rust_targe  990.3s
 => => #    Compiling uv-tool v0.0.1 (/root/crates/uv-tool)
 => => #    Compiling uv-scripts v0.0.1 (/root/crates/uv-scripts)
 => => #    Compiling tikv-jemallocator v0.6.0
 => => #    Compiling uv-performance-memory-allocator v0.1.0 (/root/crates/uv-perfo
 => => # rmance-memory-allocator)
 => => #    Compiling uv v0.5.1 (/root/crates/uv)
ERROR: failed to solve: Canceled: context canceled
uv git:main*  999s
❯ 
```

building the image locally hangs forever and never succeeds. This was working yesterday on possibly a very stale branch... don't rememeber how stale though unfortunately :/



---

_Comment by @zanieb on 2024-11-09 16:10_

We build these images for every release, so it should be working. Perhaps the problem has something to do with your platform?

---

_Label `question` added by @zanieb on 2024-11-09 16:10_

---

_Comment by @samypr100 on 2024-11-09 17:10_

Is this on linux?

---

_Referenced in [astral-sh/uv#8933](../../astral-sh/uv/pulls/8933.md) on 2024-11-09 17:36_

---

_Comment by @shaneikennedy on 2024-11-09 20:21_

This was on Mac,i tried on another laptop and the image builds, must be something on my work machine, sorry for the waste 👎 

---

_Closed by @shaneikennedy on 2024-11-09 20:21_

---
