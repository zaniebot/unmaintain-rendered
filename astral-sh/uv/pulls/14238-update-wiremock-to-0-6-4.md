```yaml
number: 14238
title: Update wiremock to 0.6.4
type: pull_request
state: merged
author: musicinmybrain
labels:
  - enhancement
assignees: []
merged: true
base: main
head: no-wiremock-fork
created_at: 2025-06-24T12:55:05Z
updated_at: 2025-06-24T13:04:56Z
url: https://github.com/astral-sh/uv/pull/14238
synced_at: 2026-01-12T16:11:06Z
```

# Update wiremock to 0.6.4

---

_@musicinmybrain_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

In e10881d49c8649a66f301af9052fd2bb17eb68d4, `uv` started using a fork of the `wiremock` crate, https://github.com/astral-sh/wiremock-rs, linking companion PR https://github.com/LukeMathWalker/wiremock-rs/pull/159. That PR was merged in `wiremock` 0.6.4, so this PR switches back to the crates.io version of `wiremock`, with a minimum version of 0.6.4.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

```
$ cargo run python install
$ cargo test
````


---

_@konstin approved on 2025-06-24 12:55_

Thanks!

---

_Label `enhancement` added by @konstin on 2025-06-24 12:59_

---

_Renamed from "Stop forking wiremock; the necessary change is in 0.6.4" to "Update wiremock to 0.6.4" by @konstin on 2025-06-24 12:59_

---

_Merged by @konstin on 2025-06-24 13:04_

---

_Closed by @konstin on 2025-06-24 13:04_

---
