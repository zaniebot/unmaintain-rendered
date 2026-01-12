```yaml
number: 8111
title: "Support PEP 723 metadata with `uv run -`"
type: pull_request
state: merged
author: manzt
labels:
  - enhancement
assignees: []
merged: true
base: main
head: stdin-pep723
created_at: 2024-10-10T21:58:16Z
updated_at: 2024-10-10T22:35:16Z
url: https://github.com/astral-sh/uv/pull/8111
synced_at: 2026-01-12T16:08:10Z
```

# Support PEP 723 metadata with `uv run -`

---

_@manzt_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #8097. One challenge is that the `Pep723Script` is used for both reading
and writing the metadata, so I wasn't sure about how to handle `script.write`
when stdin (currently just ignoring it, but maybe we should raise an error?).

## Test Plan

Added a test case copying the `test_stdin` with PEP 723 metadata.


---

_@manzt reviewed on 2024-10-10 22:00_

---

_Review comment by @manzt on `crates/uv/src/commands/project/run.rs`:117 on 2024-10-10 22:00_

Tried to move the `match` within writelin! but couldn't get the arm types to match.

---

_@manzt reviewed on 2024-10-10 22:00_

---

_Review comment by @manzt on `crates/uv/src/commands/project/run.rs`:215 on 2024-10-10 22:00_

I think the current dir makes sense here.

---

_Renamed from "Support PEP 723 metadata with uv run -" to "Support PEP 723 metadata with `uv run -`" by @manzt on 2024-10-10 22:01_

---

_@charliermarsh approved on 2024-10-10 22:22_

This looks reasonable to me. I may poke around at splitting into separate types to help with the awkwardness around writing to disk, but it doesn't need to block the change.

---

_Label `enhancement` added by @charliermarsh on 2024-10-10 22:22_

---

_Comment by @manzt on 2024-10-10 22:25_

Sounds good! Had a flight and this was a nice distraction from writing my thesis..

---

_Merged by @charliermarsh on 2024-10-10 22:35_

---

_Closed by @charliermarsh on 2024-10-10 22:35_

---

_Branch deleted on 2024-10-10 22:35_

---
