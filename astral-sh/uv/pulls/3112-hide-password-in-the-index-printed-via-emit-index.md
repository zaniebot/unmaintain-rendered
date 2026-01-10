```yaml
number: 3112
title: "Hide password in the index printed via `--emit-index-annotation`"
type: pull_request
state: merged
author: ChannyClaus
labels:
  - bug
assignees: []
merged: true
base: main
head: hide-password
created_at: 2024-04-18T03:21:51Z
updated_at: 2024-04-18T03:59:44Z
url: https://github.com/astral-sh/uv/pull/3112
synced_at: 2026-01-10T14:43:31Z
```

# Hide password in the index printed via `--emit-index-annotation`

---

_Pull request opened by @ChannyClaus on 2024-04-18 03:21_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
resolves https://github.com/astral-sh/uv/issues/3106
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
added a simple test where the password provided in `UV_INDEX_URL` is hidden in the output as expected.
<!-- How was it tested? -->


---

_@charliermarsh reviewed on 2024-04-18 03:30_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution.rs`:750 on 2024-04-18 03:30_

Maybe we should just omit the username and password entirely -- what do you think?

---

_@ChannyClaus reviewed on 2024-04-18 03:33_

---

_Review comment by @ChannyClaus on `crates/uv-resolver/src/resolution.rs`:750 on 2024-04-18 03:33_

yup, that is cleaner. making the changes -

---

_@charliermarsh approved on 2024-04-18 03:54_

Thanks!

---

_@charliermarsh reviewed on 2024-04-18 03:54_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolution.rs`:750 on 2024-04-18 03:54_

Just moved it out to a method. Thanks!

---

_Renamed from "hide password in the index printed via --emit-index-annotation" to "Hide password in the index printed via `--emit-index-annotation`" by @charliermarsh on 2024-04-18 03:54_

---

_Label `bug` added by @charliermarsh on 2024-04-18 03:54_

---

_Merged by @charliermarsh on 2024-04-18 03:59_

---

_Closed by @charliermarsh on 2024-04-18 03:59_

---
