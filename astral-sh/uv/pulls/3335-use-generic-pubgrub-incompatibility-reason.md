```yaml
number: 3335
title: Use generic pubgrub incompatibility reason
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/pubgrub-generic-incompatibility
created_at: 2024-05-02T08:13:53Z
updated_at: 2024-05-08T08:40:16Z
url: https://github.com/astral-sh/uv/pull/3335
synced_at: 2026-01-12T16:05:35Z
```

# Use generic pubgrub incompatibility reason

---

_@konstin_

Pubgrub got a new feature where all unavailability is a custom, instead of the reasonless `UnavailableDependencies` and our custom `String` type previously (https://github.com/pubgrub-rs/pubgrub/pull/208). This PR introduces a `UnavailableReason` that tracks either an entire version being unusable, or a specific version. The error messages now also track this difference properly.

The pubgrub commit is our main rebased onto the merged https://github.com/pubgrub-rs/pubgrub/pull/208, i'll push `konsti/main-rebase-generic-reason` to `main` after checking for rebase problems.

---

_Label `error messages` added by @konstin on 2024-05-02 08:13_

---

_Review requested from @zanieb by @konstin on 2024-05-02 08:13_

---

_Comment by @codspeed-hq[bot] on 2024-05-02 08:16_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti/pubgrub-generic-incompatibility)

### Merging #3335 will **not alter performance**

<sub>Comparing <code>konsti/pubgrub-generic-incompatibility</code> (b491904) with <code>main</code> (bd7860d)</sub>



### Summary

`✅ 12` untouched benchmarks






---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:594 on 2024-05-02 11:49_

Could we consider this change separately? It doesn't seem related to this pull request.

---

_@zanieb reviewed on 2024-05-02 11:49_

---

_@zanieb reviewed on 2024-05-02 11:50_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:952 on 2024-05-02 11:50_

This one doesn't seem like an improvement. Can you explain your thought here?

---

_@zanieb reviewed on 2024-05-02 11:51_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:369 on 2024-05-02 11:51_

Usually these are for packages that are fully unavailable, not a version.

---

_@konstin reviewed on 2024-05-03 12:25_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:369 on 2024-05-03 12:25_

Attached it to the relevant branch

---

_Comment by @konstin on 2024-05-03 13:13_

The whole plural handling imho makes a lot of effort over just reformulating plural-independent

---

_@konstin reviewed on 2024-05-03 13:14_

---

_Review comment by @konstin on `crates/uv/tests/pip_compile.rs`:4563 on 2024-05-03 13:14_

Here the formatter methods would need to match on the reason whether it's a whole package or only a version

---

_@konstin reviewed on 2024-05-07 11:05_

---

_Review comment by @konstin on `crates/uv/tests/pip_compile.rs`:4671 on 2024-05-07 11:05_

The message improves here because it's the versions of black, not that specific version, that is missing in the cache

---

_Marked ready for review by @konstin on 2024-05-07 11:05_

---

_Review requested from @zanieb by @zanieb on 2024-05-07 13:14_

---

_@zanieb reviewed on 2024-05-07 16:34_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:4661 on 2024-05-07 16:34_

I'm confused why does this test error? Shouldn't this resolve cc @charliermarsh 

---

_@zanieb approved on 2024-05-07 16:35_

Nice!

---

_Merged by @konstin on 2024-05-08 08:40_

---

_Closed by @konstin on 2024-05-08 08:40_

---

_Branch deleted on 2024-05-08 08:40_

---
