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
draft: true
base: main
head: fix-unclosed-assertion-quote
created_at: 2026-01-19T14:07:18Z
updated_at: 2026-01-19T14:07:54Z
url: https://github.com/astral-sh/ruff/pull/22724
synced_at: 2026-01-19T14:36:03Z
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
