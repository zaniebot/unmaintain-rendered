```yaml
number: 8669
title: Merge uv-pubgrub into uv-pep440
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/version-ranges-2
created_at: 2024-10-29T16:50:46Z
updated_at: 2024-10-29T19:15:20Z
url: https://github.com/astral-sh/uv/pull/8669
synced_at: 2026-01-12T16:08:26Z
```

# Merge uv-pubgrub into uv-pep440

---

_@konstin_

Enabled by #8667 and https://github.com/pubgrub-rs/pubgrub/pull/262, we can remove the uv-pubgrub crate and move the conversion between pep440 specifiers and version ranges directly into pep440.

In a next step, we can remove the `VersionRangesSpecifier` intermediary and perform the conversion directly.


---

_@konstin reviewed on 2024-10-29 16:51_

---

_Review comment by @konstin on `crates/uv-pep440/src/version_ranges_specifier.rs`:25 on 2024-10-29 16:51_

@BurntSushi Can this error ever happen / could we catch that in the parser and make the conversion infallible?

---

_Review comment by @BurntSushi on `crates/uv-pep440/src/version_ranges_specifier.rs`:25 on 2024-10-29 17:09_

You mean the parser for marker expressions? I think there are tests covering it?

https://github.com/astral-sh/uv/blob/36102dbd0ef08b5bc9b8ab251aa57e9d645584d3/crates/uv-pep508/src/marker/tree.rs#L2053

Whether `~=` is actually used in the real world, I'm not sure. I don't recall seeing it.

(I'm not 100% certain I've understood everything correctly here.)

Oh, I think I did misunderstand. Perhaps it's the _combination_ of `~=` and a lack of two release segments. Indeed, PEP 440 says:

> This operator MUST NOT be used with a single segment version number such as ~=1.

So I guess we could indeed catch this at parse time.

A secondary problem is how to represent this in the type system. Otherwise you'll need a panicking branch here instead of an error. Which I think seems fine as long as we document our guarantees.

---

_@BurntSushi reviewed on 2024-10-29 17:09_

---

_@zanieb approved on 2024-10-29 17:48_

---

_@konstin reviewed on 2024-10-29 19:15_

---

_Review comment by @konstin on `crates/uv-pep440/src/version_ranges_specifier.rs`:25 on 2024-10-29 19:15_

Thanks!

I will document the invariant and change to to an infallible conversion.

---

_Merged by @konstin on 2024-10-29 19:15_

---

_Closed by @konstin on 2024-10-29 19:15_

---

_Branch deleted on 2024-10-29 19:15_

---
