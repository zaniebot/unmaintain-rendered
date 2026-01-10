```yaml
number: 15933
title: "[red-knot] Use unambiguous invalid-syntax-construct for suppression comment test"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/suppression-comment-test
created_at: 2025-02-04T13:49:29Z
updated_at: 2025-02-04T14:24:55Z
url: https://github.com/astral-sh/ruff/pull/15933
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Use unambiguous invalid-syntax-construct for suppression comment test

---

_Pull request opened by @sharkdp on 2025-02-04 13:49_

## Summary

I experimented with [not trimming trailing newlines in code snippets](https://github.com/astral-sh/ruff/pull/15926#discussion_r1940992090), but since came to the conclusion that the current behavior is better because otherwise, there is no way to write snippets without a trailing newline at all. And when you copy the code from a Markdown snippet in GitHub, you also don't get a trailing newline.

I was surprised to see some test failures when I played with this though, and decided to make this test independent from this implementation detail.


---

_Label `red-knot` added by @sharkdp on 2025-02-04 13:49_

---

_Review requested from @carljm by @sharkdp on 2025-02-04 13:49_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-04 13:49_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-04 13:49_

---

_@sharkdp reviewed on 2025-02-04 13:51_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/suppressions/knot_ignore.md`:80 on 2025-02-04 13:51_

The previous syntax error here is kind of brittle for this test, as it moves to the next line *if there is a trailing newline in the file*. I made a minor modification here that unambiguously places the syntax error in this line.

---

_Label `testing` added by @sharkdp on 2025-02-04 13:51_

---

_@MichaReiser approved on 2025-02-04 13:58_

---

_Merged by @sharkdp on 2025-02-04 14:24_

---

_Closed by @sharkdp on 2025-02-04 14:24_

---

_Branch deleted on 2025-02-04 14:24_

---
