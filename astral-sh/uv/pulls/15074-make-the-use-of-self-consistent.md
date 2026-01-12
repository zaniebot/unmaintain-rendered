```yaml
number: 15074
title: "Make the use of `Self` consistent."
type: pull_request
state: merged
author: adamnemecek
labels:
  - internal
assignees: []
merged: true
base: main
head: main
created_at: 2025-08-05T04:30:44Z
updated_at: 2025-08-05T19:17:16Z
url: https://github.com/astral-sh/uv/pull/15074
synced_at: 2026-01-12T16:11:34Z
```

# Make the use of `Self` consistent.

---

_@adamnemecek_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Make the use of `Self` consistent. Mostly done by running `cargo clippy --fix -- -A clippy::all -W clippy::use_self`.

## Test Plan

<!-- How was it tested? -->
No need.


---

_Comment by @charliermarsh on 2025-08-05 11:04_

Thanks! I'm cool with this though can we figure out why this isn't already enforced in our Clippy config? Would be nice to enable it.

---

_Comment by @adamnemecek on 2025-08-05 14:32_

Just pushed an update that adds it to clippy. 

---

_Merged by @charliermarsh on 2025-08-05 19:17_

---

_Closed by @charliermarsh on 2025-08-05 19:17_

---

_Label `internal` added by @charliermarsh on 2025-08-05 19:17_

---
