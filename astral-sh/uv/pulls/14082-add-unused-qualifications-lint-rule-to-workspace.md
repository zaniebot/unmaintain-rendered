```yaml
number: 14082
title: "Add `unused_qualifications` lint rule to `[workspace.lints.rust]`"
type: pull_request
state: closed
author: chirizxc
labels: []
assignees: []
draft: true
base: main
head: lint/unused-qualifications
created_at: 2025-06-16T18:53:48Z
updated_at: 2025-06-17T20:48:55Z
url: https://github.com/astral-sh/uv/pull/14082
synced_at: 2026-01-12T16:11:01Z
```

# Add `unused_qualifications` lint rule to `[workspace.lints.rust]`

---

_@chirizxc_

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

_Converted to draft by @chirizxc on 2025-06-16 18:57_

---

_Marked ready for review by @chirizxc on 2025-06-16 19:03_

---

_Converted to draft by @zanieb on 2025-06-16 19:14_

---

_Converted to draft by @zanieb on 2025-06-16 19:14_

---

_Comment by @zanieb on 2025-06-16 19:14_

Can you explain the motivation for your changes please?

---

_Comment by @chirizxc on 2025-06-16 19:19_

> Can you explain the motivation for your changes please?

I saw the warnings in RustRover about [unused_qualifications](https://doc.rust-lang.org/rustc/lints/listing/allowed-by-default.html#unused-qualifications), thought i could solve them by adding this rule for lint

---

_Comment by @chirizxc on 2025-06-17 20:48_

As I realized from ruff in discussions, this rule is more stylistic and that RustRover has slightly different default lintig rules, so I'll close this 

---

_Closed by @chirizxc on 2025-06-17 20:48_

---

_Branch deleted on 2025-06-17 20:48_

---
