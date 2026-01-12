```yaml
number: 3645
title: Pin the zip crate to 0.6
type: pull_request
state: merged
author: musicinmybrain
labels:
  - internal
  - external
assignees: []
merged: true
base: main
head: zip-0.6
created_at: 2024-05-17T19:13:34Z
updated_at: 2024-05-18T17:31:53Z
url: https://github.com/astral-sh/uv/pull/3645
synced_at: 2026-01-12T16:05:46Z
```

# Pin the zip crate to 0.6

---

_@musicinmybrain_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Restore API-compatibility with pre-1.1.0 versions of the `zip` crate, and pin the dependency to the 0.6 series, due to concerns discussed in https://github.com/astral-sh/uv/issues/3642.

## Test Plan

<!-- How was it tested? -->

```
cargo run -p uv-dev -- fetch-python
cargo test
```

---

_@zanieb approved on 2024-05-17 21:18_

I think we may need to add an exception to the Renovate config too

---

_Label `internal` added by @zanieb on 2024-05-18 17:03_

---

_Label `upstream` added by @zanieb on 2024-05-18 17:03_

---

_Merged by @charliermarsh on 2024-05-18 17:31_

---

_Closed by @charliermarsh on 2024-05-18 17:31_

---
