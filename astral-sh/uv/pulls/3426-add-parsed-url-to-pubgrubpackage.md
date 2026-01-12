```yaml
number: 3426
title: Add parsed url to PubGrubPackage
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/add-parsed-url-to-pubgrub
created_at: 2024-05-07T15:32:51Z
updated_at: 2024-05-14T00:55:22Z
url: https://github.com/astral-sh/uv/pull/3426
synced_at: 2026-01-12T16:05:38Z
```

# Add parsed url to PubGrubPackage

---

_@konstin_

Avoid reparsing urls by storing the parsed parts across resolution on `PubGrubPackage`.

Part 1 of #3408

---

_Label `internal` added by @konstin on 2024-05-07 15:32_

---

_Renamed from "Add parsed url to pubgrub" to "Add parsed url to PUbGrubPackage" by @konstin on 2024-05-08 13:11_

---

_Marked ready for review by @konstin on 2024-05-08 13:41_

---

_Renamed from "Add parsed url to PUbGrubPackage" to "Add parsed url to PubGrubPackage" by @konstin on 2024-05-08 13:45_

---

_@charliermarsh reviewed on 2024-05-08 14:20_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/urls.rs`:72 on 2024-05-08 14:20_

What's this?

---

_Review comment by @charliermarsh on `crates/distribution-types/src/parsed_url.rs`:30 on 2024-05-08 14:21_

Why is this necessary? Where are we relying on this? I find that this pattern tends to cause a lot of subtle bugs.

---

_@charliermarsh reviewed on 2024-05-08 14:21_

---

_@konstin reviewed on 2024-05-08 15:04_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/urls.rs`:72 on 2024-05-08 15:04_

At some future point in time, we should use an equality check by parts

---

_@konstin reviewed on 2024-05-08 15:05_

---

_Review comment by @konstin on `crates/distribution-types/src/parsed_url.rs`:30 on 2024-05-08 15:05_

We were previously relying on equality between two `VerbatimUrl`s. This derive preserve this behavior, so we don't get regression when the `ParsedUrl` is slightly different from the `VerbatimUrl`. Eventually, we should remove that and rely on `ParsedUrl` comparison

---

_Review comment by @charliermarsh on `crates/distribution-types/src/parsed_url.rs`:30 on 2024-05-08 20:14_

I think my question is: where are we relying on this? Any way we can make this explicit at those sites?

---

_@charliermarsh reviewed on 2024-05-08 20:14_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/urls.rs`:37 on 2024-05-09 14:47_

The construction of `VerbatimParsedUrl` seems awkward here, just a lot of conversion. Will it be removed eventually?

---

_@charliermarsh reviewed on 2024-05-09 14:47_

---

_@charliermarsh reviewed on 2024-05-09 14:48_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/redirect.rs`:64 on 2024-05-09 14:48_

Should we just return the original URL here instead?

---

_@konstin reviewed on 2024-05-09 19:38_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/urls.rs`:37 on 2024-05-09 19:38_

Yes, `LocalEditable` will switch away from `url: VerbatimUrl` to a `VerbatimParsedUrl`.

---

_Review comment by @konstin on `crates/uv-resolver/src/redirect.rs`:64 on 2024-05-09 19:40_

Made this a debug assert, this would be a bug in uv but not one we have to concern the user with (we only fail the already-install checked if tha happens).

---

_@konstin reviewed on 2024-05-09 19:40_

---

_@konstin reviewed on 2024-05-09 19:42_

---

_Review comment by @konstin on `crates/distribution-types/src/parsed_url.rs`:30 on 2024-05-09 19:42_

I removed since it doesn't fail on our test suite, if we find regressions i'll fix them

---

_@charliermarsh approved on 2024-05-14 00:46_

---

_Merged by @charliermarsh on 2024-05-14 00:55_

---

_Closed by @charliermarsh on 2024-05-14 00:55_

---

_Branch deleted on 2024-05-14 00:55_

---
