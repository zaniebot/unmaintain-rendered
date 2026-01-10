```yaml
number: 5585
title: Add LICENSE for cloneable_seekable_reader.rs from ripunzip
type: pull_request
state: merged
author: musicinmybrain
labels: []
assignees: []
merged: true
base: main
head: ripunzip-license
created_at: 2024-07-30T02:03:29Z
updated_at: 2024-07-30T02:16:02Z
url: https://github.com/astral-sh/uv/pull/5585
synced_at: 2026-01-10T13:37:23Z
```

# Add LICENSE for cloneable_seekable_reader.rs from ripunzip

---

_Pull request opened by @musicinmybrain on 2024-07-30 02:03_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
If I’ve correctly analyzed the file `crates/uv-extract/src/vendor/cloneable_seekable_reader.rs` as vendored and forked from https://github.com/google/ripunzip/blob/v0.4.0/src/unzip/cloneable_seekable_reader.rs, then this PR adds the corresponding license text from https://github.com/google/ripunzip/blob/v0.4.0/LICENSE.

This would therefore fix https://github.com/astral-sh/uv/issues/5584.

## Test Plan

<!-- How was it tested? -->
N/A; inspect for appropriate contents.

---

_Merged by @charliermarsh on 2024-07-30 02:15_

---

_Closed by @charliermarsh on 2024-07-30 02:15_

---

_Comment by @charliermarsh on 2024-07-30 02:16_

Thanks, I thought adding the header was sufficient but this seems correct.

---
