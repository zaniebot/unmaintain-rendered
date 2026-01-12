```yaml
number: 12171
title: Fix replacement edit range computation
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
merged: true
base: main
head: dhruv/fix-replacement
created_at: 2024-07-03T15:33:04Z
updated_at: 2024-07-04T03:54:09Z
url: https://github.com/astral-sh/ruff/pull/12171
synced_at: 2026-01-12T15:55:40Z
```

# Fix replacement edit range computation

---

_@dhruvmanila_

## Summary

This PR fixes various bugs for computing the replacement range between the original and modified source for the language server.

1. When finding the end offset of the source and modified range, we should apply `zip` on the reversed iterator. The bug was that it was reversing the already zipped iterator. The problem here is that the length of both slices aren't going to be the same unless the source wasn't modified at all. Refer to the [Rust playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=44f860d31bd26456f3586b6ab530c22f) where you can see this in action.
2. Skip the first line when computing the start offset because the first line start value will always be 0 and the default value of the source / modified range start is also 0. So, comparing 0 and 0 is not useful which means we can skip the first value.
3. While iterating in the reverse direction, we should only stop if the line start is strictly less than the source start i.e., we should use `<` instead of `<=`.

fixes: #12128 

## Test Plan

Add test cases where the text is being inserted, deleted, and replaced between the original and new source code, validate the replacement ranges.

---

_Label `bug` added by @dhruvmanila on 2024-07-03 15:33_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/replacement.rs`:25 on 2024-07-03 15:33_

Skipping the first line start i.e., change (2)

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/replacement.rs`:43 on 2024-07-03 15:33_

The `zip` and `rev` bug u.e., change (1)

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/replacement.rs`:46 on 2024-07-03 15:34_

Switching from `<=` to `<` i.e., change (3)

---

_@dhruvmanila reviewed on 2024-07-03 15:34_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-03 15:35_

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-07-03 15:35_

---

_Label `server` added by @MichaReiser on 2024-07-03 15:37_

---

_Review comment by @MichaReiser on `crates/ruff_server/src/edit/replacement.rs`:25 on 2024-07-03 15:39_

Is it guaranteed that `source_line_starts[0] == TextSize::new(0)`? 

---

_@MichaReiser approved on 2024-07-03 15:41_

---

_Comment by @github-actions[bot] on 2024-07-03 15:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila reviewed on 2024-07-04 03:53_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/edit/replacement.rs`:25 on 2024-07-04 03:53_

Yes:

https://github.com/astral-sh/ruff/blob/8e6f722d9bcba39a5f0ef508e4d6e454eb86f349/crates/ruff_source_file/src/line_index.rs#L28-L32

---

_Merged by @dhruvmanila on 2024-07-04 03:54_

---

_Closed by @dhruvmanila on 2024-07-04 03:54_

---

_Branch deleted on 2024-07-04 03:54_

---
