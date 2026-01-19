```yaml
number: 22720
title: "[ty] Use `smallvec_inline!` more consistently"
type: pull_request
state: open
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
base: alex/parameter-fluent-style
head: alex/smallvec-inline
created_at: 2026-01-19T13:20:48Z
updated_at: 2026-01-19T13:22:40Z
url: https://github.com/astral-sh/ruff/pull/22720
synced_at: 2026-01-19T13:34:31Z
```

# [ty] Use `smallvec_inline!` more consistently

---

_@AlexWaygood_

## Summary

This is another refactor pulled out of #22718 that should have 0 behaviour changes. We were being a bit inconsistent on `main` about whether we were using `smallvec![]` or the newer `smallvec_inline![]` feature. This PR switches us over to using `smallvec_inline![]` wherever it's possible to do so.

## Test Plan

Existing tests


---

_Review requested from @carljm by @AlexWaygood on 2026-01-19 13:20_

---

_Review requested from @sharkdp by @AlexWaygood on 2026-01-19 13:20_

---

_Review requested from @dcreager by @AlexWaygood on 2026-01-19 13:20_

---

_Label `internal` added by @AlexWaygood on 2026-01-19 13:20_

---

_Label `ty` added by @AlexWaygood on 2026-01-19 13:20_

---

_@MichaReiser approved on 2026-01-19 13:22_

---

_Comment by @astral-sh-bot[bot] on 2026-01-19 13:22_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance/)

No changes detected ✅





---
