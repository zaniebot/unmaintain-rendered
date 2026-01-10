```yaml
number: 15259
title: Add a test for overshadowing redirects
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - testing
assignees: []
merged: true
base: main
head: tests-redirects
created_at: 2025-01-04T16:17:28Z
updated_at: 2025-01-05T08:50:33Z
url: https://github.com/astral-sh/ruff/pull/15259
synced_at: 2026-01-10T20:34:00Z
```

# Add a test for overshadowing redirects

---

_Pull request opened by @InSyncWithFoo on 2025-01-04 16:17_

## Summary

Resolves #15257.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @InSyncWithFoo on 2025-01-04 16:23_

The test added currently does not pass, but it will once #15258 is merged. For the record, here's what the message looks like:

```text
────────────
     Summary [  10.999s] 3795 tests run: 3794 passed, 1 failed, 23 skipped
        FAIL [   0.004s] ruff_linter rule_redirects::tests::overshadowing_redirects
──── STDOUT:             ruff_linter rule_redirects::tests::overshadowing_redirects

running 1 test
test rule_redirects::tests::overshadowing_redirects ... FAILED

failures:

failures:
    rule_redirects::tests::overshadowing_redirects

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 2217 filtered out; finished in 0.00s

──── STDERR:             ruff_linter rule_redirects::tests::overshadowing_redirects
thread 'rule_redirects::tests::overshadowing_redirects' panicked at crates/ruff_linter/src/rule_redirects.rs:155:17:
Rule RUF025 is overshadowed by a redirect, which points to C420.
```

As for Clippy, I'm not sure why it says the imports are unused.

---

_Label `testing` added by @MichaReiser on 2025-01-05 08:41_

---

_Comment by @MichaReiser on 2025-01-05 08:42_

Thank you so much for fixing this so quickly! 

Clippy complaint because the `mod test` missed the `#[cfg(test)]` gating. 

---

_Merged by @MichaReiser on 2025-01-05 08:47_

---

_Closed by @MichaReiser on 2025-01-05 08:47_

---

_Comment by @github-actions[bot] on 2025-01-05 08:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Branch deleted on 2025-01-05 08:50_

---
