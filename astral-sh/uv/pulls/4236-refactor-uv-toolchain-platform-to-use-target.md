```yaml
number: 4236
title: "Refactor `uv-toolchain::platform` to use `target-lexicon`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/toolchain-dltl
created_at: 2024-06-11T16:16:00Z
updated_at: 2024-06-12T14:12:26Z
url: https://github.com/astral-sh/uv/pull/4236
synced_at: 2026-01-10T13:54:02Z
```

# Refactor `uv-toolchain::platform` to use `target-lexicon`

---

_Pull request opened by @zanieb on 2024-06-11 16:16_

Closes https://github.com/astral-sh/uv/issues/3857

Instead of using custom `Arch`, `Os`, and `Libc` types I just use `target-lexicon`'s which enumerate way more variants and implement display and parsing. We use a wrapper type to represent a couple special cases to support the "x86" alias for "i686" and "macos" for "darwin". Alternatively we could try to use our `platform-tags` types but those capture more information (like operating system versions) that we don't have for downloads.

As discussed in https://github.com/astral-sh/uv/pull/4160, this is not sufficient for proper libc detection but that work is larger and will be handled separately.

---

_Label `internal` added by @zanieb on 2024-06-11 16:16_

---

_@zanieb reviewed on 2024-06-11 16:24_

---

_Review comment by @zanieb on `crates/uv-toolchain/fetch-download-metadata.py`:133 on 2024-06-11 16:24_

Just keeping this for consistency for now

---

_Review requested from @konstin by @zanieb on 2024-06-11 16:24_

---

_Marked ready for review by @zanieb on 2024-06-11 16:25_

---

_Review comment by @konstin on `crates/uv-toolchain/src/downloads.inc`:1 on 2024-06-11 17:05_

This file is one huge block, do we know if this affects compilation times?

---

_Review comment by @konstin on `crates/uv-toolchain/src/downloads.rs`:269 on 2024-06-11 17:10_

This type has a key of the same name, what's their relationship?

---

_Review comment by @konstin on `crates/uv-toolchain/src/platform.rs`:29 on 2024-06-11 17:13_

I would go with glibc as default on linux target for the start, otherwise the statically linked builds will behave differently than the glibc builds (and the python-build-standalone builds are currently unusable on musl anyway) 

---

_Review comment by @konstin on `crates/uv-toolchain/src/platform.rs`:104 on 2024-06-11 17:16_

Can you add a comment about the different `i?86` arches?

---

_@konstin approved on 2024-06-11 17:17_

Setting the right default libc on linux is critical, the rest looks good

---

_@zanieb reviewed on 2024-06-11 17:30_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/downloads.rs`:269 on 2024-06-11 17:30_

Sorry that was stale, fixed. I had to remove the static field because we don't map the operating system or architecture in the template anymore. 

---

_@zanieb reviewed on 2024-06-11 17:31_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/platform.rs`:29 on 2024-06-11 17:31_

Our statically linked builds, you mean?

---

_@zanieb reviewed on 2024-06-11 17:31_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/downloads.inc`:1 on 2024-06-11 17:31_

No clue, is there a good way to measure?

---

_@zanieb reviewed on 2024-06-11 17:45_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/platform.rs`:104 on 2024-06-11 17:45_

Actually.. What do you want me to say? We just alias the i686 variant in parse and display for users.

---

_Review comment by @konstin on `crates/uv-toolchain/src/platform.rs`:29 on 2024-06-11 17:46_

Yes, `uv-x86_64-unknown-linux-gnu.tar.gz` and `uv-x86_64-unknown-linux-musl.tar.gz` would behave differently on the same machine since one has been compile with glibc and thinks that current os and the other has been compile with musl and thinks that's the current os (or libc none because it's statically compiled, i haven't checked)

---

_@konstin reviewed on 2024-06-11 17:46_

---

_@zanieb reviewed on 2024-06-11 17:49_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/platform.rs`:29 on 2024-06-11 17:49_

I guess I feel like that's more reasonable behavior for now than _always_ forcing download of GNU Python builds? Like if we can't determine what your system is using you think we should always fallback to GNU?

---

_@konstin reviewed on 2024-06-11 17:58_

---

_Review comment by @konstin on `crates/uv-toolchain/src/platform.rs`:29 on 2024-06-11 17:58_

For actually shipping this as stable, we need libc detection and some kind of handling of the musl python-build-standalone situation.

For the preview phase, most linux users are on glibc (it's more like everyone is on glibc except when deploying in an alpine container) and musl python-build-standalone is broken, so assuming glibc works.

---

_@zanieb reviewed on 2024-06-11 18:11_

---

_Review comment by @zanieb on `crates/uv-toolchain/src/platform.rs`:29 on 2024-06-11 18:11_

> We need libc detection

This can still fail and I think we should have a reasonable fallback behavior. Matching the distribution's target seems more intuitive than falling back to glibc in that case?

---

_@konstin reviewed on 2024-06-12 11:07_

---

_Review comment by @konstin on `crates/uv-toolchain/src/platform.rs`:29 on 2024-06-12 11:07_

I don't think we can make a reasonable guess when libc detection fails, and in the old `detect_linux_libc` we would error if we couldn't detect a libc.

---

_@konstin reviewed on 2024-06-12 11:08_

---

_Review comment by @konstin on `crates/uv-toolchain/src/platform.rs`:104 on 2024-06-12 11:08_

In this case just that it doesn't matter which of those we pick

---

_@konstin reviewed on 2024-06-12 11:09_

---

_Review comment by @konstin on `crates/uv-toolchain/src/downloads.inc`:1 on 2024-06-12 11:09_

We could compare compile times with a dummy, but there aren't any good tools around.

---

_Merged by @zanieb on 2024-06-12 14:11_

---

_Closed by @zanieb on 2024-06-12 14:11_

---

_Branch deleted on 2024-06-12 14:11_

---

_Comment by @zanieb on 2024-06-12 14:12_

See https://github.com/astral-sh/uv/issues/4242 for proper musl support.

---
