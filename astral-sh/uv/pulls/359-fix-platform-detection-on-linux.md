```yaml
number: 359
title: fix platform detection on Linux
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/fix-linux-detection
created_at: 2023-11-07T16:03:12Z
updated_at: 2023-11-07T16:39:37Z
url: https://github.com/astral-sh/uv/pull/359
synced_at: 2026-01-12T16:03:54Z
```

# fix platform detection on Linux

---

_@BurntSushi_

Rejigger Linux platform detection

This change makes some very small improvements to the Linux platform
detection logic. In particular, the existing logic did not work on my
Archlinux machine since /lib64/ld-linux-x86-64.so.2 isn't a symlink. In
that case, the detection logic should have fallen back to the slower
`ldd --version` technique, but `read_link` fails outright when its
argument isn't a symbolic link. So we tweak the logic to allow it to
fail, and if it does, we still try the `ldd --version` approach instead
of giving up completely.

I also made some cosmetic improvements to the regex matching, as well as
ensuring that the regexes are only compiled exactly once.

---

_Review requested from @konstin by @BurntSushi on 2023-11-07 16:03_

---

_Review comment by @konstin on `crates/platform-host/src/linux.rs`:17 on 2023-11-07 16:05_

Is that preferred? I think we're just using `static MY_REGEX: Lazy<Regex> = Lazy::new(|| Regex::new(r".*").unwrap());` everywhere else

---

_Comment by @charliermarsh on 2023-11-07 16:05_

Love it.

---

_Review comment by @konstin on `crates/platform-host/src/linux.rs`:37 on 2023-11-07 16:07_

That is neat

---

_Review comment by @konstin on `crates/platform-host/src/linux.rs`:19 on 2023-11-07 16:09_

I think we should rename this to something like `glibc_version_from_ldd` (this was already wrong previously)

---

_@konstin approved on 2023-11-07 16:09_

Thanks!

---

_Review comment by @BurntSushi on `crates/platform-host/src/linux.rs`:17 on 2023-11-07 16:22_

Ah okay. I switched it over to `once_cell`'s `Lazy` type. I usually don't like bringing in a dependency just for this since `OnceLock` is in std, but I now see that `once_cell` was already a workspace dependency.

(I expect to add something like the `regex!` macro I wrote above to the `regex` crate proper once its MSRV is new enough. I do like it better than writing out `Lazy<Regex>`. It's terser and less noisy.)

---

_@BurntSushi reviewed on 2023-11-07 16:22_

---

_Merged by @BurntSushi on 2023-11-07 16:39_

---

_Closed by @BurntSushi on 2023-11-07 16:39_

---

_Branch deleted on 2023-11-07 16:39_

---
