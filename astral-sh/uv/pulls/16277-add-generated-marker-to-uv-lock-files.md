```yaml
number: 16277
title: "Add `@generated` marker to `uv.lock` files"
type: pull_request
state: open
author: jackrosenthal
labels:
  - needs-decision
assignees: []
base: main
head: at-generated
created_at: 2025-10-13T03:09:36Z
updated_at: 2025-10-16T21:30:38Z
url: https://github.com/astral-sh/uv/pull/16277
synced_at: 2026-01-10T06:36:15Z
```

# Add `@generated` marker to `uv.lock` files

---

_Pull request opened by @jackrosenthal on 2025-10-13 03:09_

## Summary

This adds a comment containing the `@generated` marker to the top of
all `uv.lock` files.  The `@generated` marker is a widely-adopted
convention (see https://generated.at/) that helps tools (like IDEs,
code formatters, and code review tools) identify files as generated
and handle them appropriately.

Other common tools use this marker in lock files as well (such as Cargo
and Poetry).

Fixes #13878.

## Test Plan

- [X] Unit tests
- [X] Observe marker in output of `uv lock`

---

_Comment by @zanieb on 2025-10-16 18:52_

I don't think we have reached consensus that this should be done.

---

_Label `needs-decision` added by @zanieb on 2025-10-16 18:52_

---

_@fabi125 reviewed on 2025-10-16 21:30_

---

_Review comment by @fabi125 on `crates/uv-resolver/src/lock/mod.rs`:1269 on 2025-10-16 21:30_

Could we use a single space after the period? Basically every single style guide on earth recommends a single space (even when using a monospace font).
e.g. see https://cmosshoptalk.com/2020/03/24/one-space-or-two/ or https://apastyle.apa.org/style-grammar-guidelines/paper-format/accessibility/typography#myth4


---
