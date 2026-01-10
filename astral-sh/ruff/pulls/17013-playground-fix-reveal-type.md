```yaml
number: 17013
title: "[playground] Fix `reveal_type`"
type: pull_request
state: merged
author: sharkdp
labels:
  - playground
assignees: []
merged: true
base: main
head: david/playground-fix-reveal_type
created_at: 2025-03-27T16:10:41Z
updated_at: 2025-03-27T16:24:23Z
url: https://github.com/astral-sh/ruff/pull/17013
synced_at: 2026-01-10T19:40:36Z
```

# [playground] Fix `reveal_type`

---

_Pull request opened by @sharkdp on 2025-03-27 16:10_

## Summary

Capture both `stdout` and `stderr` in a single stream. This fixes `reveal_type`, which prints to `stderr` by default.

## Test Plan

Tested with a simple `reveal_type(1)` example and got the output:
```
Runtime value is '1'
Runtime type is 'int'
```

---

_Label `playground` added by @sharkdp on 2025-03-27 16:10_

---

_Comment by @MichaReiser on 2025-03-27 16:16_

I'm confused. Why does `reveal_type` print to `stderr`? Isn't it using `print`?

---

_@MichaReiser approved on 2025-03-27 16:16_

---

_Comment by @sharkdp on 2025-03-27 16:21_

There are two sources of output in this patched version. `print` prints to stdout, but the original `typing.reveal_type` prints to stderr.

---

_Merged by @sharkdp on 2025-03-27 16:21_

---

_Closed by @sharkdp on 2025-03-27 16:21_

---

_Branch deleted on 2025-03-27 16:21_

---
