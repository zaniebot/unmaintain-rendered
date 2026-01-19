```yaml
number: 22724
title: "[ty] Fix panic on malformed mdtest assertion"
type: pull_request
state: open
author: Hugo-Polloli
labels:
  - testing
  - ty
assignees: []
base: main
head: fix-unclosed-assertion-quote
created_at: 2026-01-19T14:07:18Z
updated_at: 2026-01-19T15:08:38Z
url: https://github.com/astral-sh/ruff/pull/22724
synced_at: 2026-01-19T15:25:06Z
```

# [ty] Fix panic on malformed mdtest assertion

---

_@Hugo-Polloli_

## Summary

Fixes [#2548](https://github.com/astral-sh/ty/issues/2548)
Fix a panic in the mdtest assertion parser when encountering a malformed `# error` assertion that ends with a trailing quote (e.g. `# error: [rule]"`). The parser now treats this as an invalid/unclosed message instead of slicing with invalid bounds.

## Test Plan

Added this test : `cargo test -p ty_test trailing_quote_without_message_not_allowed`


---

_Label `testing` added by @AlexWaygood on 2026-01-19 14:07_

---

_Label `ty` added by @AlexWaygood on 2026-01-19 14:07_

---

_Marked ready for review by @Hugo-Polloli on 2026-01-19 14:27_

---

_Review requested from @carljm by @Hugo-Polloli on 2026-01-19 14:27_

---

_Review requested from @MichaReiser by @Hugo-Polloli on 2026-01-19 14:27_

---

_Review requested from @AlexWaygood by @Hugo-Polloli on 2026-01-19 14:27_

---

_Review requested from @sharkdp by @Hugo-Polloli on 2026-01-19 14:27_

---

_Review requested from @dcreager by @Hugo-Polloli on 2026-01-19 14:27_

---

_Review comment by @AlexWaygood on `crates/ty_test/src/matcher.rs`:1418 on 2026-01-19 14:31_

hmm, should we add a new `ErrorAssertionParseError` variant for the specific case where the assertion _does_ end with `"` as the final character, but the quotes were unclosed? This error message feels a bit confusing for this specific case, because `"` _was_ the final character here, after all!

---

_@AlexWaygood reviewed on 2026-01-19 14:31_

this looks great, thank you!!

---

_@AlexWaygood approved on 2026-01-19 15:07_

Perfect, thank you!

---

_@Hugo-Polloli reviewed on 2026-01-19 15:08_

---

_Review comment by @Hugo-Polloli on `crates/ty_test/src/matcher.rs`:1418 on 2026-01-19 15:08_

oops, tried to keep things a bit too minimal and not introduce a new variant but you're right, it ended up being confusing, I added a new one and I hope it's more explicit now!

---
