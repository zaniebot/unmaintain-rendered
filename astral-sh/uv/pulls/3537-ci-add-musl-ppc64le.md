```yaml
number: 3537
title: "ci: add musl ppc64le"
type: pull_request
state: merged
author: henryiii
labels:
  - releases
assignees: []
merged: true
base: main
head: henryiii/ci/othermusl
created_at: 2024-05-13T04:25:40Z
updated_at: 2024-05-15T03:39:41Z
url: https://github.com/astral-sh/uv/pull/3537
synced_at: 2026-01-10T14:37:54Z
```

# ci: add musl ppc64le

---

_Pull request opened by @henryiii on 2024-05-13 04:25_


<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Working on followup to #3523. I expect some changes will be required to match the glibc versions above.


<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @henryiii on 2024-05-13 05:02_

I'm not sure how to cross-compile for these targets. Binaries are not provided, AFAICT, so I think the target needs to be built. I vaguely have clues on how to do that, but not sure how to inject my ideas into maturin-action.

I don't think it's going to work, locally I tried:

```bash
docker run -v $PWD:/uv -w /uv --rm -it ubuntu
apt update && apt install curl build-essential cmake
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
. "$HOME/.cargo/env"
rustup install nightly
rustup +nightly component add rust-src
cargo +nightly build -Z build-std --target powerpc64le-unknown-linux-musl
```

It makes it pretty far, but crashes compiling OpenSSL.

---

_Comment by @charliermarsh on 2024-05-14 00:17_

@konstin or @messense may have ideas.

---

_Comment by @henryiii on 2024-05-14 01:51_

It looks like there's mention of the powerpc64le one in maturin-action, so I was rather thinking that might work. s309x is sounds like it's not possible currently. I'm checking to see if missing uv on these two (I would assume) rarely used images is acceptable in manylinux.

---

_Comment by @messense on 2024-05-14 01:59_

ppc64le musl requires Rust nightly, I have tried to [build a Docker image for s390x musl](https://github.com/rust-cross/rust-musl-cross/pull/62) in the past, but it seems to have some issues (see https://github.com/rust-cross/rust-musl-cross/issues/63) so I haven't added it to `maturin-action`.

---

_Renamed from "ci: add builds for other musl targets" to "ci: add musl ppc64le" by @henryiii on 2024-05-14 05:15_

---

_Comment by @henryiii on 2024-05-14 05:16_

Great! Only musl s390x for manylinux (and Windows ARM for cibuildwheel) left!

Is https://github.com/rust-cross/rust-musl-cross/issues/63 still an issue? Looks like it's been a while since it was attempted?

---

_Marked ready for review by @henryiii on 2024-05-14 12:20_

---

_Comment by @messense on 2024-05-15 02:29_

> Is [rust-cross/rust-musl-cross#63](https://github.com/rust-cross/rust-musl-cross/issues/63) still an issue? Looks like it's been a while since it was attempted?

Probably still an issue, but I think it's solvable given that Alpine Linux has the target working for years now, although it's a hard problem for people without deep knowledge in GCC.

---

_@charliermarsh approved on 2024-05-15 03:39_

---

_Merged by @charliermarsh on 2024-05-15 03:39_

---

_Closed by @charliermarsh on 2024-05-15 03:39_

---

_Label `release` added by @charliermarsh on 2024-05-15 03:39_

---
