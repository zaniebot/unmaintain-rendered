```yaml
number: 15932
title: "[red-knot] MDTest: Fix line numbers in error messages"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/fix-mdtest-line-numbers
created_at: 2025-02-04T13:28:34Z
updated_at: 2025-02-04T13:44:06Z
url: https://github.com/astral-sh/ruff/pull/15932
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] MDTest: Fix line numbers in error messages

---

_Pull request opened by @sharkdp on 2025-02-04 13:28_

## Summary

I broke the line number reporting in error messages with my previous PR.

## Test Plan

Introduced an error in a Markdown test and made sure that the line in the error message matches.


---

_Label `testing` added by @sharkdp on 2025-02-04 13:28_

---

_Label `red-knot` added by @sharkdp on 2025-02-04 13:28_

---

_Review requested from @carljm by @sharkdp on 2025-02-04 13:28_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-04 13:28_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-04 13:28_

---

_@sharkdp reviewed on 2025-02-04 13:29_

---

_Review comment by @sharkdp on `crates/red_knot_test/src/parser.rs`:423 on 2025-02-04 13:29_

I changed this to explicitly pass in the offset instead of relying on a specific state of the lexer.

---

_@MichaReiser approved on 2025-02-04 13:32_

---

_@MichaReiser reviewed on 2025-02-04 13:33_

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:309 on 2025-02-04 13:33_

```suggestion
                        let backtick_offset = self.offset() - "```".text_len();
```

---

_Merged by @sharkdp on 2025-02-04 13:44_

---

_Closed by @sharkdp on 2025-02-04 13:44_

---

_Branch deleted on 2025-02-04 13:44_

---
