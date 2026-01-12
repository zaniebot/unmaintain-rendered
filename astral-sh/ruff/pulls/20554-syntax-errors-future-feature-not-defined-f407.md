```yaml
number: 20554
title: "[syntax-errors]: future-feature-not-defined (F407)"
type: pull_request
state: merged
author: 11happy
labels:
  - internal
assignees: []
merged: true
base: main
head: F407
created_at: 2025-09-24T16:47:34Z
updated_at: 2025-09-25T17:52:25Z
url: https://github.com/astral-sh/ruff/pull/20554
synced_at: 2026-01-12T15:57:04Z
```

# [syntax-errors]: future-feature-not-defined (F407)

---

_@11happy_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR implements https://docs.astral.sh/ruff/rules/future-feature-not-defined/ (F407) as a semantic syntax error.

## Test Plan

<!-- How was it tested? -->

I have written inline tests as directed in #17412 


---

_Review requested from @MichaReiser by @11happy on 2025-09-24 16:47_

---

_Review requested from @dhruvmanila by @11happy on 2025-09-24 16:47_

---

_Renamed from "[synatx-errors]: future-feature-not-defined (F406)" to "[synatx-errors]: future-feature-not-defined (F407)" by @11happy on 2025-09-24 16:49_

---

_Comment by @github-actions[bot] on 2025-09-24 17:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "[synatx-errors]: future-feature-not-defined (F407)" to "[syntax-errors]: future-feature-not-defined (F407)" by @11happy on 2025-09-24 17:00_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyflakes/mod.rs`:2973 on 2025-09-24 18:21_

I don't think this should change. We may need to preserve the `__future__` check in `statement.rs` that was previously handled by the `else if`.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:75 on 2025-09-24 18:26_

One other interesting case might be multiple invalid features. I think we'll emit one diagnostic per invalid feature, but it could be nice to see that in a test case. And maybe one case with one known and one unknown diagonstic.

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:59 on 2025-09-24 18:29_

nit: I would probably just make this a standalone function after the `impl SemanticSyntaxChecker` block rather than an associated function. It also doesn't need to be `pub`.

I think a name like `is_known_future_feature` might be better too, now that it's not in a `future` module.

```suggestion
    fn is_known_future_feature(name: &str) -> bool {
```

---

_@ntBre reviewed on 2025-09-24 18:32_

Thank you! This looks great overall, just a few small suggestions

---

_Label `internal` added by @ntBre on 2025-09-24 18:32_

---

_@11happy reviewed on 2025-09-25 16:54_

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/pyflakes/mod.rs`:2973 on 2025-09-25 16:54_

done

---

_@11happy reviewed on 2025-09-25 16:54_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:75 on 2025-09-25 16:54_

done

---

_@11happy reviewed on 2025-09-25 16:54_

---

_Review comment by @11happy on `crates/ruff_python_parser/src/semantic_errors.rs`:59 on 2025-09-25 16:54_

done

---

_@ntBre approved on 2025-09-25 17:52_

Thank you!

---

_Merged by @ntBre on 2025-09-25 17:52_

---

_Closed by @ntBre on 2025-09-25 17:52_

---
