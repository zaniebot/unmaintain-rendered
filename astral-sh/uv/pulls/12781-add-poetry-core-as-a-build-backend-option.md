```yaml
number: 12781
title: Add poetry-core as a build backend option
type: pull_request
state: merged
author: Secrus
labels: []
assignees: []
merged: true
base: main
head: add-poetry-core
created_at: 2025-04-09T14:02:09Z
updated_at: 2025-04-29T09:14:51Z
url: https://github.com/astral-sh/uv/pull/12781
synced_at: 2026-01-12T16:10:23Z
```

# Add poetry-core as a build backend option

---

_@Secrus_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This adds `poetry-core` as a build backend choice. 

## Test Plan

<!-- How was it tested? -->


---

_Assigned to @konstin by @konstin on 2025-04-25 13:55_

---

_Comment by @konstin on 2025-04-25 16:29_

Sorry, this PR slipped through. Could you add test (in `crates/uv/tests/it/init.rs`) and run `cargo dev generate-all` to update the docs?

---

_@konstin reviewed on 2025-04-25 16:29_

---

_Review comment by @konstin on `crates/uv-configuration/src/project_build_backend.rs`:30 on 2025-04-25 16:29_

I'd call this entry `Poetry` and keep the poetry core variants alias, similar to hatch/hatchling

---

_@Secrus reviewed on 2025-04-25 20:08_

---

_Review comment by @Secrus on `crates/uv-configuration/src/project_build_backend.rs`:30 on 2025-04-25 20:08_

So, something like this?
```rs
    #[serde(alias = "poetry-core")]
    #[cfg_attr(feature = "clap", value(alias = "poetry-core", alias = "poetry_core"))]
    Poetry,
```

---

_@konstin reviewed on 2025-04-28 10:07_

---

_Review comment by @konstin on `crates/uv-configuration/src/project_build_backend.rs`:30 on 2025-04-28 10:07_

yep!

---

_Comment by @Secrus on 2025-04-28 14:15_

@konstin could you please take a look at the test failure? It looks like the test suite is unable to find `poetry-core > 2.x`. Do I need to add some fixture data or something?

---

_Comment by @konstin on 2025-04-28 15:36_

That happens cause the tests are using an exclude-newer cutoff by default to keep the tests deterministic, and that is older than poetry 2.0. You can use `with_exclude_newer("...")` on the `TestContext` to set it to a more recent timestamp.

---

_Marked ready for review by @Secrus on 2025-04-28 17:03_

---

_Review requested from @konstin by @Secrus on 2025-04-28 17:03_

---

_@konstin approved on 2025-04-28 19:02_

Thanks!

---

_Merged by @konstin on 2025-04-28 19:11_

---

_Closed by @konstin on 2025-04-28 19:11_

---

_Branch deleted on 2025-04-29 09:14_

---
