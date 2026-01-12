```yaml
number: 11824
title: Warn when duplicate index names found in single file
type: pull_request
state: merged
author: sou1118
labels:
  - error messages
assignees: []
merged: true
base: main
head: feature/warn-duplicate-index-names
created_at: 2025-02-27T07:09:08Z
updated_at: 2025-02-28T08:28:50Z
url: https://github.com/astral-sh/uv/pull/11824
synced_at: 2026-01-12T16:10:01Z
```

# Warn when duplicate index names found in single file

---

_@sou1118_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:
- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
## Summary

This pull request introduces validation for unique index names in the `tool.uv.index` field and adds corresponding tests to ensure the functionality. The most important changes include adding a custom deserializer function, updating the `ToolUv` struct to use the new deserializer, and adding tests to verify the behavior.

Validation and deserialization:

* [`crates/uv-workspace/src/pyproject.rs`](diffhunk://#diff-e12cd255985adfd45ab06f398cb420d2f543841ccbeea4175ccf827aa9215b9dR283-R311): Added a custom deserializer function `deserialize_index_vec` to validate that index names in the `tool.uv.index` field are unique.
* [`crates/uv-workspace/src/pyproject.rs`](diffhunk://#diff-e12cd255985adfd45ab06f398cb420d2f543841ccbeea4175ccf827aa9215b9dR374): Updated the `ToolUv` struct to use the `deserialize_index_vec` function for the `index` field.

Testing:

* [`crates/uv/tests/it/lock.rs`](diffhunk://#diff-82edd36151736f44055f699a34c8b19a63ffc4cf3c86bf5fb34d69f8ac88a957R15336): Added a test `lock_repeat_named_index` to verify that duplicate index names result in an error. [[1]](diffhunk://#diff-82edd36151736f44055f699a34c8b19a63ffc4cf3c86bf5fb34d69f8ac88a957R15336) [[2]](diffhunk://#diff-82edd36151736f44055f699a34c8b19a63ffc4cf3c86bf5fb34d69f8ac88a957R15360-R15402)
* [`crates/uv/tests/it/lock.rs`](diffhunk://#diff-82edd36151736f44055f699a34c8b19a63ffc4cf3c86bf5fb34d69f8ac88a957R15360-R15402): Added a test `lock_unique_named_index` to verify that unique index names result in successful lock file generation.

Schema update:

* [`uv.schema.json`](diffhunk://#diff-c669473b258a19ba6d3557d0369126773b68b27171989f265333a77bc5cb935bR205): Updated the schema to set the default value of the `index` field to `null`.

Fixes #11804

## Test Plan
### Steps to reproduce and verify the fix:

1. Clone the repository and checkout the feature branch
   ```bash
   git clone https://github.com/astral-sh/uv.git
   cd uv
   git checkout feature/warn-duplicate-index-names
   ```

2. Build the modified binary
   ```bash
   cargo build
   ```

3. Create a test project using the system installed uv
   ```bash
   uv init uv-test
   cd uv-test
   ```

4. Manually edit pyproject.toml to add duplicate index names
   ```toml
   [[tool.uv.index]]
   name = "alpha_b"
   url = "<omitted>"

   [[tool.uv.index]]
   name = "alpha_b"
   url = "<omitted>"
   ```

5. Try to add a package using the modified binary
   ```bash
   ../target/debug/uv add numpy
   ```

### Results
Before: use release binary
![スクリーンショット 2025-02-27 15 52 28](https://github.com/user-attachments/assets/2823a4b4-b3ba-40aa-aa41-e593d35c3f3b)

After: use self build binary
![スクリーンショット 2025-02-27 15 51 58](https://github.com/user-attachments/assets/9ac773a9-58cd-4d4b-8685-148bf6dc85fb)

Now when attempting to use a pyproject.toml with duplicate index names, the modified binary correctly detects the issue and produces an error message:
```
error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 9, column 1
  |
9 | [[tool.uv.index]]
  | ^^^^^^^^^^^^^^^^^
duplicate index name `alpha_b`
```


---

_Converted to draft by @sou1118 on 2025-02-27 07:23_

---

_Marked ready for review by @sou1118 on 2025-02-27 09:03_

---

_Review requested from @charliermarsh by @konstin on 2025-02-27 09:06_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-27 21:16_

---

_Label `error messages` added by @charliermarsh on 2025-02-27 21:16_

---

_@charliermarsh approved on 2025-02-27 21:23_

---

_Comment by @charliermarsh on 2025-02-27 21:24_

Thanks!

---

_Merged by @charliermarsh on 2025-02-27 21:33_

---

_Closed by @charliermarsh on 2025-02-27 21:33_

---

_Branch deleted on 2025-02-28 08:28_

---
