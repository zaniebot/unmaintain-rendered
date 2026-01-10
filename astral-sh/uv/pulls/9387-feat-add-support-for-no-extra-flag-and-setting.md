```yaml
number: 9387
title: "feat: add support for `--no-extra` flag and setting"
type: pull_request
state: merged
author: alan910127
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: main
created_at: 2024-11-23T16:24:54Z
updated_at: 2024-11-24T02:25:09Z
url: https://github.com/astral-sh/uv/pull/9387
synced_at: 2026-01-10T12:00:00Z
```

# feat: add support for `--no-extra` flag and setting

---

_Pull request opened by @alan910127 on 2024-11-23 16:24_

<!--  
Thank you for contributing to uv! To help us review effectively, please ensure that:  
- The pull request includes a summary of the change.  
- The title is descriptive and concise.  
- Relevant issues are referenced where applicable.  
-->

## Summary

Resolves #9333  

This pull request introduces support for the `--no-extra` command-line flag and the corresponding `no-extra` UV setting.  

### Behavior
- When `--all-extras` is supplied, the specified extras in `--no-extra` will be excluded from the installation.  
- If `--all-extras` is not supplied, `--no-extra` has no effect and is safely ignored.  

## Test Plan

Since `ExtrasSpecification::from_args` and `ExtrasSpecification::extra_names` are the most important parts in the implementation, I added the following tests in the `uv-configuration/src/extras.rs` module:

- **`test_no_extra_full`**: Verifies behavior when `no_extra` includes the entire list of extras.  
- **`test_no_extra_partial`**: Tests partial exclusion, ensuring only specified extras are excluded.  
- **`test_no_extra_empty`**: Confirms that no extras are excluded if `no_extra` is empty.  
- **`test_no_extra_excessive`**: Ensures the implementation ignores `no_extra` values that don't match any available extras.  
- **`test_no_extra_without_all_extras`**: Validates that `no_extra` has no effect when `--all-extras` is not supplied.  
- **`test_no_extra_without_package_extras`**: Confirms correct behavior when no extras are available in the package.  
- **`test_no_extra_duplicates`**: Verifies that duplicate entries in `pkg_extras` or `no_extra` do not cause errors.  


---

_Renamed from ".0" to "feat: add support for `--no-extra` flag and setting" by @alan910127 on 2024-11-23 16:25_

---

_Comment by @alan910127 on 2024-11-23 17:03_

Hello, uv maintainers, I managed to get the CI to pass, except for the `cargo test | macos` job. The error message indicates it failed to fetch something due to a "too many requests" error. Unfortunately, I wasn’t able to resolve this issue.

---

_Marked ready for review by @alan910127 on 2024-11-23 17:24_

---

_Comment by @charliermarsh on 2024-11-24 02:01_

This looks good, but I think `--extra foo --no-extra foo` should omit `foo`.

---

_@charliermarsh reviewed on 2024-11-24 02:09_

---

_Review comment by @charliermarsh on `crates/uv-configuration/src/extras.rs`:25 on 2024-11-24 02:09_

Maybe we filter `extra` in the `else` branch here, to remove anything in `no_extra`?

---

_@charliermarsh approved on 2024-11-24 02:16_

Excellent PR, thank you!

---

_Label `enhancement` added by @charliermarsh on 2024-11-24 02:16_

---

_Label `cli` added by @charliermarsh on 2024-11-24 02:16_

---

_Merged by @charliermarsh on 2024-11-24 02:25_

---

_Closed by @charliermarsh on 2024-11-24 02:25_

---
