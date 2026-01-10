```yaml
number: 6744
title: "Add support for `--with-editable` to `uv tool ru`"
type: pull_request
state: merged
author: kyoto7250
labels:
  - enhancement
assignees: []
merged: true
base: main
head: add_with_editable_in_uvx
created_at: 2024-08-28T10:50:08Z
updated_at: 2024-09-19T02:13:47Z
url: https://github.com/astral-sh/uv/pull/6744
synced_at: 2026-01-10T12:53:34Z
```

# Add support for `--with-editable` to `uv tool ru`

---

_Pull request opened by @kyoto7250 on 2024-08-28 10:50_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
<!-- What's the purpose of the change? What does it do, and why? -->
close #6272 

## Test Plan
<!-- How was it tested? -->
As in https://github.com/astral-sh/uv/pull/6262

---

_Converted to draft by @kyoto7250 on 2024-08-28 10:59_

---

_@zanieb reviewed on 2024-08-28 13:19_

---

_Review comment by @zanieb on `crates/uv/tests/tool_run.rs`:950 on 2024-08-28 13:19_

Just a quick comment, looks like this _doesn't_ reference `--with` like we'd hope :)

---

_@kyoto7250 reviewed on 2024-08-28 13:59_

---

_Review comment by @kyoto7250 on `crates/uv/tests/tool_run.rs`:950 on 2024-08-28 13:59_

Thank you for your review :)
I'll try working on it for a few more days 👍🏽 

---

_@kyoto7250 reviewed on 2024-09-02 16:05_

---

_Review comment by @kyoto7250 on `crates/uv/tests/tool_run.rs`:950 on 2024-09-02 16:05_

I've implemented the feature similarly to the pull request, but I'm running into an issue where an `anyhow::Error` is being returned, which is preventing me from adding context to the error. I need to investigate this further, but if you have any insights or know of a possible cause, I would greatly appreciate your input.

---

_Marked ready for review by @kyoto7250 on 2024-09-08 13:45_

---

_@kyoto7250 reviewed on 2024-09-08 13:45_

---

_Review comment by @kyoto7250 on `crates/uv/tests/tool_run.rs`:950 on 2024-09-08 13:45_

@zanieb 
Could you please check it once?

---

_Assigned to @zanieb by @zanieb on 2024-09-15 23:09_

---

_Comment by @zanieb on 2024-09-15 23:09_

Sorry for the delay! It's been a busy few weeks. I'll get to this soon.

---

_Renamed from "Add --with-editable flag in uv tool" to "Add support for `--with-editable` to `uv tool`" by @zanieb on 2024-09-17 20:42_

---

_Label `enhancement` added by @zanieb on 2024-09-17 20:42_

---

_@zanieb reviewed on 2024-09-17 20:43_

---

_Review comment by @zanieb on `crates/uv/tests/tool_run.rs`:950 on 2024-09-17 20:43_

73085aec6f48da64fb2edd63d7e3df38177466d2 has the relevant fix

---

_@zanieb approved on 2024-09-17 20:44_

---

_Merged by @zanieb on 2024-09-17 20:51_

---

_Closed by @zanieb on 2024-09-17 20:51_

---

_Renamed from "Add support for `--with-editable` to `uv tool`" to "Add support for `--with-editable` to `uv tool ru`" by @zanieb on 2024-09-19 02:13_

---
