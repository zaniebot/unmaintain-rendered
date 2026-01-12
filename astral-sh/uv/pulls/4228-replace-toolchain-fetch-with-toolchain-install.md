```yaml
number: 4228
title: "Replace `toolchain fetch` with `toolchain install`"
type: pull_request
state: merged
author: SigureMo
labels:
  - tracing
  - preview
assignees: []
merged: true
base: main
head: replace-toolchain-fetch-with-toolchain-install
created_at: 2024-06-11T11:33:05Z
updated_at: 2024-06-11T13:10:23Z
url: https://github.com/astral-sh/uv/pull/4228
synced_at: 2026-01-12T16:06:06Z
```

# Replace `toolchain fetch` with `toolchain install`

---

_@SigureMo_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Something that looks like it was forgotten to replace in #4164.

## Test Plan

<!-- How was it tested? -->

Run `cargo run toolchain install` should display the warning: ``warning: `uv toolchain install` is experimental and may change without warning.``

---

_@ibraheemdev approved on 2024-06-11 12:00_

Thanks!

---

_Merged by @ibraheemdev on 2024-06-11 12:00_

---

_Closed by @ibraheemdev on 2024-06-11 12:00_

---

_Branch deleted on 2024-06-11 12:01_

---

_Comment by @zanieb on 2024-06-11 13:10_

Thanks @SigureMo 

---

_Label `tracing` added by @zanieb on 2024-06-11 13:10_

---

_Label `preview` added by @zanieb on 2024-06-11 13:10_

---
