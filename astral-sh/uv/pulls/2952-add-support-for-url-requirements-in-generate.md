```yaml
number: 2952
title: "Add support for URL requirements in `--generate-hashes`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/generate-hashes-ii
created_at: 2024-04-10T01:43:24Z
updated_at: 2024-04-10T20:02:46Z
url: https://github.com/astral-sh/uv/pull/2952
synced_at: 2026-01-10T14:43:31Z
```

# Add support for URL requirements in `--generate-hashes`

---

_Pull request opened by @charliermarsh on 2024-04-10 01:43_

## Summary

This PR enables hash generation for URL requirements when the user provides `--generate-hashes` to `pip compile`. While we include the hashes from the registry already, today, we omit hashes for URLs.

To power hash generation, we introduce a `HashPolicy` abstraction:

```rust
#[derive(Debug, Clone, Copy, PartialEq, Eq)]
pub enum HashPolicy<'a> {
    /// No hash policy is specified.
    None,
    /// Hashes should be generated (specifically, a SHA-256 hash), but not validated.
    Generate,
    /// Hashes should be validated against a pre-defined list of hashes. If necessary, hashes should
    /// be generated so as to ensure that the archive is valid.
    Validate(&'a [HashDigest]),
}
```

All of the methods on the distribution database now accept this policy, instead of accepting `&'a [HashDigest]`.

Closes #2378.


---

_Label `enhancement` added by @charliermarsh on 2024-04-10 01:43_

---

_@charliermarsh reviewed on 2024-04-10 01:45_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:358 on 2024-04-10 01:45_

Worst part of this PR but not 100% sure how to solve. It's fine in practice, just somewhat brittle.

---

_Renamed from "Introduce a hash policy abstraction" to "Add support for URL requirements in `--generate-hashes`" by @charliermarsh on 2024-04-10 01:45_

---

_Marked ready for review by @charliermarsh on 2024-04-10 01:45_

---

_@konstin approved on 2024-04-10 12:49_

---

_Merged by @charliermarsh on 2024-04-10 20:02_

---

_Closed by @charliermarsh on 2024-04-10 20:02_

---

_Branch deleted on 2024-04-10 20:02_

---
