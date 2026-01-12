```yaml
number: 16548
title: "Allow for unnormalized names in the METADATA file (#16547)"
type: pull_request
state: merged
author: caschb
labels:
  - bug
assignees: []
merged: true
base: main
head: uppercase-wheel-metadata
created_at: 2025-11-01T20:37:18Z
updated_at: 2025-11-02T01:57:37Z
url: https://github.com/astral-sh/uv/pull/16548
synced_at: 2026-01-12T16:12:19Z
```

# Allow for unnormalized names in the METADATA file (#16547)

---

_@caschb_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Deserialize project name into both a String and a ProjectName, this way we can keep using the normalized name elsewhere while respecting the original name from the `pyproject.toml` file

This PR addresses issue #16547

## Test Plan

I added a new test for this, and I ran the test suite in the `metadata.rs` file.


---

_@charliermarsh reviewed on 2025-11-01 21:37_

---

_Review comment by @charliermarsh on `crates/uv-build-backend/src/metadata.rs`:126 on 2025-11-01 21:37_

I'd probably suggest adding a `VerbatimPackageName` struct with its own `Deserialize` implementation that stores the two.

---

_Label `bug` added by @charliermarsh on 2025-11-01 21:47_

---

_@charliermarsh approved on 2025-11-01 21:53_

---

_Merged by @charliermarsh on 2025-11-01 21:58_

---

_Closed by @charliermarsh on 2025-11-01 21:58_

---

_Comment by @charliermarsh on 2025-11-01 22:03_

Thanks!

---

_Branch deleted on 2025-11-02 01:57_

---
