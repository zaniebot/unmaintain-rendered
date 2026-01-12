```yaml
number: 4160
title: Add support for musl Python toolchain fetches
type: pull_request
state: closed
author: zanieb
labels:
  - preview
assignees: []
draft: true
base: main
head: zb/toolchain-vii
created_at: 2024-06-08T14:06:30Z
updated_at: 2024-06-11T16:26:32Z
url: https://github.com/astral-sh/uv/pull/4160
synced_at: 2026-01-12T16:06:04Z
```

# Add support for musl Python toolchain fetches

---

_@zanieb_

Previously, we always assumed GNU.

I considered restoring some of the `platform-host` crate which was removed in #2381. I'm not sure we actually need to do the whole ELF inspection thing though since we don't need version information and `target-lexicon` seems to have what we need. I presume since `target-lexicon::HOST` is a const, that the information is based on the uv's target host not the runtime host. This seems okay for toolchain download assumptions? If not, we can go back to the ELF inspection technique but I'm not fully comfortable modifying that code.

---

_Label `preview` added by @zanieb on 2024-06-08 14:06_

---

_Review requested from @konstin by @zanieb on 2024-06-08 14:10_

---

_Comment by @charliermarsh on 2024-06-08 20:47_

> This seems okay for toolchain download assumptions?

I'm not sure... Wouldn't this _always_ download the musl Pythons on our Linux builds now, since we only ship musl? My guess is we _do_ need to determine this dynamically.

---

_Comment by @zanieb on 2024-06-08 21:34_

Eek that might be true. Okay, back to the goblin elf approach. I'll need to chat with Konsti about that code.

---

_Comment by @charliermarsh on 2024-06-09 17:23_

I'm actually a bit worried about the use of `std::env::consts::ARCH` too -- is it granular enough to map to the set of Pythons that we ship? That's what #3857 was about.

---

_Comment by @zanieb on 2024-06-09 21:47_

Oh interesting thanks for the link I didn't see #3857 or https://github.com/astral-sh/uv/pull/3855. I'll need to look into that further. These were not intended to be fully "production ready" when written. I certainly wouldn't be surprised if we need to do something more robust or restore more of the implementation we were using before we moved platform information queries to rely on a Python implementation.

---

_Comment by @konstin on 2024-06-10 09:37_

tl;dr: We need to go back to the libc detection from https://github.com/astral-sh/uv/pull/2381.

There are three different libc configurations supported by rust: glibc dynamically linked (x86_64-unknown-linux-gnu), statically linked with musl (x86_64-unknown-linux-musl), musl dynamically linked (`-crt-static`, https://github.com/rust-lang/compiler-team/issues/422). To load a dynamic library such as native python modules, the libc configuration of the loading binary must match the one of the loaded library. Python encodes this as `manylinux` and `musllinux` tags. Statically linked binaries can't load dynamic libraries (by the mechanism we need).

The libc that uv itself uses doesn't matter since we don't load libraries ourself, but the python we download and start needs to match the libc the user expects. Finding the platform default is non-trivial since you can install glibc on alpine (https://wiki.alpinelinux.org/wiki/Running_glibc_programs) and musl on ubuntu (`apt install musl`). As proxy, we check `/bin/ls` or `/bin/sh` for its libc and use it platform default. Alternatively, we could switch to shipping dynamically linked glibc and musl builds of uv and enforce using the right one in the installer (removing the glibc-too-old to musl fallback).

On that note, python-build-standalone is currently statically linked for musl and fails to loads any compiled native module (https://github.com/astral-sh/uv/pull/2382). This is a blocker for deploying python-build-standalone on alpine targets.

---

_Converted to draft by @zanieb on 2024-06-10 14:12_

---

_Comment by @zanieb on 2024-06-10 14:12_

Moving this back to draft as it will require significant revisions.


---

_Comment by @zanieb on 2024-06-11 16:26_

I'll open an issue for proper musl detection.

This pull request is superseded by #4236 

---

_Closed by @zanieb on 2024-06-11 16:26_

---
