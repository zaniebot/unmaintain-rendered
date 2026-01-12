```yaml
number: 15484
title: "Reject already-installed wheels that don't match the target platform"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - no-build
assignees: []
merged: true
base: main
head: charlie/wheel-validation
created_at: 2025-08-24T15:42:53Z
updated_at: 2025-08-26T07:14:00Z
url: https://github.com/astral-sh/uv/pull/15484
synced_at: 2026-01-12T16:11:46Z
```

# Reject already-installed wheels that don't match the target platform

---

_@charliermarsh_

## Summary

We've received several requests to validate that installed wheels match the current Python platform. This isn't _super_ common, since it requires that your platform changes in some meaningful way (e.g., you switch from x86 to ARM), though in practice, it sounds like it _can_ happen in HPC environments. This seems like a good thing to do regardless, so we now validate that the tags (as recoded in `WHEEL`) are consistent with the current platform during installs.

Closes https://github.com/astral-sh/uv/issues/15035.


---

_Label `no-build` added by @charliermarsh on 2025-08-24 15:42_

---

_Marked ready for review by @charliermarsh on 2025-08-24 18:17_

---

_Review requested from @zanieb by @charliermarsh on 2025-08-24 18:23_

---

_Review requested from @konstin by @charliermarsh on 2025-08-24 18:23_

---

_Label `enhancement` added by @charliermarsh on 2025-08-24 18:23_

---

_Review comment by @konstin on `crates/uv-resolver/src/candidate_selector.rs`:274 on 2025-08-25 09:04_

We should reuse the tags we just read over of calling the method again without unwrap 

---

_Review comment by @konstin on `crates/uv-resolver/src/candidate_selector.rs`:272 on 2025-08-25 09:04_

Is this unwrap-safe?

---

_Review comment by @konstin on `crates/uv-installer/src/site_packages.rs`:247 on 2025-08-25 09:05_

STOPSHIP

---

_Review comment by @konstin on `crates/uv-installer/src/satisfies.rs`:64 on 2025-08-25 09:36_

STOPSHIP

---

_Review comment by @konstin on `crates/uv-installer/src/satisfies.rs`:76 on 2025-08-25 09:37_

This check is in every branch, can we move it outside of the match?

---

_Review comment by @konstin on `crates/uv-distribution-filename/src/expanded_tags.rs`:89 on 2025-08-25 10:04_

Here, we're ignoring `Invalid*Tag` error types, while filtering out tags we don't recognise. The same problem exists in the original wheel filename implementation where `InvalidLanguageTag` is unused (`InvalidLanguageTag` has two hits in the codebase, this file and `wheel.rs`). In both cases we have `repr: SmallString` to cover tags we filtered out in error messages.

---

_Review comment by @konstin on `crates/uv-distribution-filename/src/expanded_tags.rs`:123 on 2025-08-25 10:06_

Can you add a test for an invalid segment, that we're using `repr` to not lose it?

---

_@konstin reviewed on 2025-08-25 10:13_

---

_@charliermarsh reviewed on 2025-08-25 12:21_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/candidate_selector.rs`:272 on 2025-08-25 12:21_

Sorry, no, this should have a STOPSHIP on it. I just wanted to see if this failed anywhere in CI.

---

_@charliermarsh reviewed on 2025-08-25 12:22_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/satisfies.rs`:76 on 2025-08-25 12:22_

I want it to run last in each branch though. I could put it _after_ the `match`, but I thought that might be more confusing.

---

_@charliermarsh reviewed on 2025-08-25 12:22_

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/expanded_tags.rs`:89 on 2025-08-25 12:22_

Yeah this is intentional, because we don't want to error out on tags we don't understand.

---

_Review requested from @konstin by @charliermarsh on 2025-08-25 12:29_

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:248 on 2025-08-25 12:32_

Just wondering, why do we throw the error here but drop it everywhere else?

---

_@zanieb reviewed on 2025-08-25 12:32_

---

_@zanieb reviewed on 2025-08-25 12:32_

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:250 on 2025-08-25 12:32_

👍 this seems nice to have, might be worth opening an issue for it maybe someone can pick it up

---

_@zanieb approved on 2025-08-25 12:33_

---

_@charliermarsh reviewed on 2025-08-25 12:33_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/site_packages.rs`:248 on 2025-08-25 12:33_

Because this is the "diagnostics" method where I think it's useful to see if it failed (it runs if you add `--strict` or do `uv pip check`). It should probably be its own diagnostic though rather than failing?

---

_@konstin approved on 2025-08-25 12:36_

---

_Merged by @charliermarsh on 2025-08-25 13:20_

---

_Closed by @charliermarsh on 2025-08-25 13:20_

---

_Branch deleted on 2025-08-25 13:20_

---
