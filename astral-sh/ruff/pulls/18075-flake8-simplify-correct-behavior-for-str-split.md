```yaml
number: 18075
title: "[`flake8-simplify`] Correct behavior for `str.split`/`rsplit` with `maxsplit=0` (`SIM905`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix/issue-18069
created_at: 2025-05-13T18:29:23Z
updated_at: 2025-05-14T18:24:28Z
url: https://github.com/astral-sh/ruff/pull/18075
synced_at: 2026-01-10T18:51:01Z
```

# [`flake8-simplify`] Correct behavior for `str.split`/`rsplit` with `maxsplit=0` (`SIM905`)

---

_Pull request opened by @danparizher on 2025-05-13 18:29_

Fixes #18069

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR addresses a bug in the `flake8-simplify` rule `SIM905` (split-static-string) where `str.split(maxsplit=0)` and `str.rsplit(maxsplit=0)` produced incorrect results for empty strings or strings starting/ending with whitespace. The fix ensures that the linting rule's suggested replacements now align with Python's native behavior for these specific `maxsplit=0` scenarios.

## Test Plan

1.  Added new test cases to the existing `crates/ruff_linter/resources/test/fixtures/flake8_simplify/SIM905.py` fixture to cover the scenarios described in issue #18069.
2.  Ran `cargo test -p ruff_linter`.
3.  Verified and accepted the updated snapshots for `SIM905.py` using `cargo insta review`. The new snapshots confirm the corrected behavior for `maxsplit=0`.

---

_Comment by @github-actions[bot] on 2025-05-13 18:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-05-13 21:36_

---

_Label `fixes` added by @ntBre on 2025-05-13 21:36_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:195 on 2025-05-14 16:39_

Can we just use the Rust `trim_start` and `trim_end` methods here:

```suggestion
                let processed_str = if direction == Direction::Left {                               
                    string_val.trim_start()                                                         
                } else {                                                                            
                    string_val.trim_end()                                                           
                };                                                     
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:176 on 2025-05-14 16:40_

This is actually the exact implementation of [`trim`](https://doc.rust-lang.org/src/core/str/mod.rs.html#2125) 🙂 
```suggestion
                .trim()
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:195 on 2025-05-14 16:46_

With this, I think we can flatten the whole `match` arm to something like this:

```rust
            let processed_str = if direction == Direction::Left {
                string_val.trim_start()
            } else {
                string_val.trim_end()
            };
            let list_items: &[_] = if processed_str.is_empty() {
                &[]
            } else {
                &[processed_str]
            };
            Some(construct_replacement(
                list_items,
                str_value.first_literal_flags(),
            ))
```

---

_@ntBre reviewed on 2025-05-14 16:55_

Thanks! Just a couple of simplification suggestions, but the logic looks great to me. And thanks for all of the test cases!

---

_Comment by @danparizher on 2025-05-14 17:55_

Thanks for the review! This is definitely a lot better. I can also work on the logic for #18101 and include it in this PR as well. Let me know if that makes the most sense!

---

_Comment by @ntBre on 2025-05-14 18:16_

> Thanks for the review! This is definitely a lot better. I can also work on the logic for #18101 and include it in this PR as well. Let me know if that makes the most sense!

No problem, thanks for handling the suggestions!

I think we can hold off on #18101 for now. I went ahead and put SIM905 in the title since that's the rule I tested, but I think it's likely more widespread, and we might be able to handle it generally rather than in each individual rule (at least I hope so!).

---

_@ntBre approved on 2025-05-14 18:18_

Great, thanks!

---

_Renamed from "[`flake8-simplify`]: Correct behavior for str.split/rsplit with maxsplit=0 (`SIM905`)" to "[`flake8-simplify`] Correct behavior for `str.split/rsplit` with `maxsplit=0` (`SIM905`)" by @ntBre on 2025-05-14 18:19_

---

_Renamed from "[`flake8-simplify`] Correct behavior for `str.split/rsplit` with `maxsplit=0` (`SIM905`)" to "[`flake8-simplify`] Correct behavior for `str.split`/`rsplit` with `maxsplit=0` (`SIM905`)" by @ntBre on 2025-05-14 18:19_

---

_Merged by @ntBre on 2025-05-14 18:20_

---

_Closed by @ntBre on 2025-05-14 18:20_

---

_Branch deleted on 2025-05-14 18:24_

---
