```yaml
number: 15387
title: "feat(ci): build loongarch64 binaries in CI"
type: pull_request
state: merged
author: SkyBird233
labels: []
assignees: []
merged: true
base: main
head: feat/loongarch64-binary
created_at: 2025-08-20T07:46:53Z
updated_at: 2025-09-06T11:52:08Z
url: https://github.com/astral-sh/uv/pull/15387
synced_at: 2026-01-12T16:11:43Z
```

# feat(ci): build loongarch64 binaries in CI

---

_@SkyBird233_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR adds support for building loongarch64 binaries in CI. As uv itself runs perfectly well on loongarch64 and with the latter's userbase steadily growing, it would be a good idea to ship prebuilt binaries to help them out.

Please note that as Ubuntu is not yet available for loongarch64, I have elected to use a Debian Trixie container maintained by community members. In addition, as Debian's pip does not allow installing modules system-wide, the workflow for loongarch64 installs additional modules in a virtual environment.

## Test Plan

<!-- How was it tested? -->

Tests are included in CI and the loongarch64 artifacts built in [this workflow](https://github.com/SkyBird233/uv/actions/runs/17091637376/job/48466486690) has been smoke tested.

---

_Comment by @zanieb on 2025-08-21 00:52_

As with https://github.com/astral-sh/python-build-standalone/pull/653 I think we're blocked on clearing this with our lawyers first.

---

_Comment by @MingcongBai on 2025-08-21 01:45_

> As with [astral-sh/python-build-standalone#653](https://github.com/astral-sh/python-build-standalone/pull/653) I think we're blocked on clearing this with our lawyers first.

Interesting... Just out of curiosity, what legal challenges/processes are we facing with this?

---

_Comment by @shauneccles on 2025-08-21 01:49_

Sanctions is my guess - https://www.tomshardware.com/news/us-govt-blacklists-loongson-and-inspur



---

_Comment by @MingcongBai on 2025-08-21 02:21_

> Sanctions is my guess - https://www.tomshardware.com/news/us-govt-blacklists-loongson-and-inspur

Same guess as mine but was wondering if they could just say it outright.

---

_Comment by @zanieb on 2025-08-21 03:23_

Yeah, the sanctions.

---

_Comment by @xen0n on 2025-08-21 12:26_

My two cents... (Disclaimer: IANAL and I'm Chinese, unaffiliated with Loongson though, so take my words with a grain of salt. I'm just trying to bring some constructive points forward because I am a LoongArch user and community contributor myself.)

Technically the sanctions only affect the company entity, and LoongArch is just an abstract ISA, even though right now all readily available LoongArch CPU models are from Loongson. And the contributor is not a Loongson employee AFAIK. So maybe having anything to do with this PR doesn't imply any relationship with or endorsement of Loongson?

On the other hand, like it or not politics is not going anywhere, and not producing LoongArch binaries wouldn't stop the community from doing so downstream, so maybe the net effect is just making community packagers' and LoongArch daily-drivers' lives harder. Is it possible to both keep being "compliant" and somehow not regress downstream UX when they want to just install `uv`, like allowing people to specify download mirrors or anything equivalent?

---

_Comment by @andypost on 2025-08-25 11:06_

Would be great to add this kind of testing as starting with 0.8.13 building `uv` fails for Alpinelinux on `rustix`
see the full log https://gitlab.alpinelinux.org/alpine/aports/-/jobs/1985668

---

_Comment by @andypost on 2025-08-28 22:24_

0.8.14 fails too
```
error[E0659]: `STATX_TYPE` is ambiguous
   --> /home/buildozer/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rustix-1.0.8/src/fs/statx.rs:73:25
    |
73  |         const TYPE = c::STATX_TYPE;
    |                         ^^^^^^^^^^ ambiguous name
    |
    = note: ambiguous because of multiple glob imports of a name in the same module
note: `STATX_TYPE` could refer to the constant imported here
   --> /home/buildozer/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rustix-1.0.8/src/backend/libc/c.rs:7:16
    |
7   | pub(crate) use libc::*;
    |                ^^^^^^^
    = help: consider adding an explicit import of `STATX_TYPE` to disambiguate
note: `STATX_TYPE` could also refer to the constant imported here
   --> /home/buildozer/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/rustix-1.0.8/src/backend/libc/c.rs:512:16
    |
512 | pub(crate) use statx_flags::*;
    |                ^^^^^^^^^^^^^^
    = help: consider adding an explicit import of `STATX_TYPE` to disambiguate
```

---

_Comment by @SkyBird233 on 2025-08-29 01:49_

FYI @andypost, here's the tracking issue for the rustix problem: bytecodealliance/rustix#1496. It appears to be unrelated to uv.

---

_Comment by @andypost on 2025-08-29 10:44_

@SkyBird233 thank you, following

---

_Comment by @MingcongBai on 2025-08-29 13:00_

> My two cents... (Disclaimer: IANAL and I'm Chinese, unaffiliated with Loongson though, so take my words with a grain of salt. I'm just trying to bring some constructive points forward because I am a LoongArch user and community contributor myself.)
> 
> Technically the sanctions only affect the company entity, and LoongArch is just an abstract ISA, even though right now all readily available LoongArch CPU models are from Loongson. And the contributor is not a Loongson employee AFAIK. So maybe having anything to do with this PR doesn't imply any relationship with or endorsement of Loongson?
> 
> On the other hand, like it or not politics is not going anywhere, and not producing LoongArch binaries wouldn't stop the community from doing so downstream, so maybe the net effect is just making community packagers' and LoongArch daily-drivers' lives harder. Is it possible to both keep being "compliant" and somehow not regress downstream UX when they want to just install `uv`, like allowing people to specify download mirrors or anything equivalent?

Hi @zanieb, were you able to hear back from your lawyers?

---

_Comment by @zanieb on 2025-08-29 13:18_

I can't comment on that until we have a policy, sorry. We're working on it.

---

_Comment by @MingcongBai on 2025-08-29 13:22_

> I can't comment on that until we have a policy, sorry. We're working on it.

Good to know, thanks.

---

_Comment by @zanieb on 2025-09-05 21:01_

We can accept this change.

Before we do, I'm reflecting back on the RISCV changes that broke our release process, e.g., see https://github.com/astral-sh/uv/pull/14009. Do we need to do something here to prevent that?

---

_Comment by @SkyBird233 on 2025-09-06 04:39_

Oh that's a point. Since there's no official loongarch64 support in [pypa/manylinux](https://github.com/pypa/manylinux/blob/main/README.rst) right now, I'm filtering it out from the PyPI publishing workflow.


---

_Closed by @SkyBird233 on 2025-09-06 04:39_

---

_Reopened by @SkyBird233 on 2025-09-06 04:39_

---

_Comment by @SkyBird233 on 2025-09-06 04:42_

My apologies, I accidentally closed the pull request while using the GitHub UI. I have reopened it.

---

_@zanieb approved on 2025-09-06 11:52_

---

_Merged by @zanieb on 2025-09-06 11:52_

---

_Closed by @zanieb on 2025-09-06 11:52_

---
