```yaml
number: 15313
title: "Update cli.md to use proper uv cache subcommand \"clean\""
type: pull_request
state: merged
author: NewDestinyDan
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-08-15T19:19:34Z
updated_at: 2025-08-15T21:06:38Z
url: https://github.com/astral-sh/uv/pull/15313
synced_at: 2026-01-10T06:44:33Z
```

# Update cli.md to use proper uv cache subcommand "clean"

---

_Pull request opened by @NewDestinyDan on 2025-08-15 19:19_

Correct typo. "uv cache clear" is not a command.

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @zanieb on 2025-08-15 20:07_

Thanks! This file is generated. Can you change this in the source too please?

---

_Comment by @NewDestinyDan on 2025-08-15 20:24_

> Thanks! This file is generated. Can you change this in the source too please?

I'm new to pull requests and OSS. Would be happy to make the change in the source file. Can you help point me in the right direction?

---

_Comment by @zanieb on 2025-08-15 20:40_

Yeah it comes from, e.g.,

https://github.com/astral-sh/uv/blob/82d5b6780a17fa4b03fed44f605c723b5df481f5/crates/uv-cli/src/lib.rs#L2776

---

_Comment by @zanieb on 2025-08-15 20:40_

I think you could just do a global search for it.

---

_Label `documentation` added by @zanieb on 2025-08-15 21:04_

---

_Comment by @zanieb on 2025-08-15 21:05_

Thanks!

---

_Merged by @zanieb on 2025-08-15 21:06_

---

_Closed by @zanieb on 2025-08-15 21:06_

---
