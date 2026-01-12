```yaml
number: 17785
title: "[semantic-syntax-tests] Add test fixtures for AwaitOutsideAsync"
type: pull_request
state: merged
author: maxmynter
labels:
  - testing
assignees: []
merged: true
base: main
head: semantic-syntax-v3
created_at: 2025-05-02T02:08:32Z
updated_at: 2025-05-05T18:02:06Z
url: https://github.com/astral-sh/ruff/pull/17785
synced_at: 2026-01-12T15:56:05Z
```

# [semantic-syntax-tests] Add test fixtures for AwaitOutsideAsync

---

_@maxmynter_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Re: #17526 
## Summary
Add test fixtures for `AwaitOutsideAsync` and `AsyncComprehensionOutsideAsyncFunction` errors. 

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
This is a test. 

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-05-02 02:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "(tests) Add test fixtures for AwaitOutsideAsync" to "[semantic-syntax-tests] Add test fixtures for AwaitOutsideAsync" by @maxmynter on 2025-05-02 02:38_

---

_Review requested from @ntBre by @MichaReiser on 2025-05-02 06:11_

---

_Label `testing` added by @ntBre on 2025-05-05 17:49_

---

_@ntBre approved on 2025-05-05 18:00_

Thanks!

This doesn't actually cover `AsyncComprehensionOutsideAsyncFunction`, which got renamed to `AsyncComprehensionInSyncComprehension` in #17460 because the old name was quite confusing. It corresponds very specifically to the use of an async comprehension inside of a sync comprehension (as the new name indicates) on a specific version of Python before this was allowed. I also added tests in that PR, though, so I will go ahead and mark it covered.

---

_Merged by @ntBre on 2025-05-05 18:02_

---

_Closed by @ntBre on 2025-05-05 18:02_

---
