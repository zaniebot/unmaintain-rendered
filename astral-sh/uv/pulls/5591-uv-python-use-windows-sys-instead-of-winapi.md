```yaml
number: 5591
title: "uv-python: use windows-sys instead of winapi"
type: pull_request
state: merged
author: ognevny
labels:
  - enhancement
  - windows
assignees: []
merged: true
base: main
head: use-windows-sys
created_at: 2024-07-30T09:22:58Z
updated_at: 2024-08-23T14:51:08Z
url: https://github.com/astral-sh/uv/pull/5591
synced_at: 2026-01-10T13:09:50Z
```

# uv-python: use windows-sys instead of winapi

---

_Pull request opened by @ognevny on 2024-07-30 09:22_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

use windows-sys bindings maintained by microsoft devs. winapi didn't has any updates for more than 3 years

## Test Plan

cargo test. it failed locally because I don't have Python 3.12 installed


---

_Label `enhancement` added by @konstin on 2024-07-30 09:41_

---

_Label `windows` added by @konstin on 2024-07-30 09:42_

---

_Comment by @konstin on 2024-07-30 09:42_

> cargo test. it failed locally because I don't have Python 3.12 installed

`cargo run python install` fixes that.

---

_@konstin approved on 2024-07-30 09:43_

Thanks

---

_Merged by @konstin on 2024-07-30 09:43_

---

_Closed by @konstin on 2024-07-30 09:43_

---

_Branch deleted on 2024-07-30 09:45_

---
