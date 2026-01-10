```yaml
number: 10209
title: "Add support for optional `--description` in `uv init`"
type: pull_request
state: merged
author: guptaarnav
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: uv-init-desc
created_at: 2024-12-27T22:41:28Z
updated_at: 2024-12-28T00:44:41Z
url: https://github.com/astral-sh/uv/pull/10209
synced_at: 2026-01-10T11:44:38Z
```

# Add support for optional `--description` in `uv init`

---

_Pull request opened by @guptaarnav on 2024-12-27 22:41_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Closes #7913 by adding an optional `--description` argument to `uv init` that fills the description field in the pyproject.toml with the supplied arg value.  

Updated `uv init` docs to describe this new optional argument.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Added snapshot tests in `uv/crates/uv/tests/it/init.rs` to test this functionality.
<!-- How was it tested? -->


---

_@charliermarsh approved on 2024-12-27 23:55_

Thanks!

---

_Renamed from "adding optional --description to uv init" to "Add support for optional `--description` in `uv init`" by @charliermarsh on 2024-12-27 23:55_

---

_Label `enhancement` added by @charliermarsh on 2024-12-27 23:55_

---

_Label `cli` added by @charliermarsh on 2024-12-27 23:55_

---

_Merged by @charliermarsh on 2024-12-28 00:06_

---

_Closed by @charliermarsh on 2024-12-28 00:06_

---

_Branch deleted on 2024-12-28 00:44_

---
