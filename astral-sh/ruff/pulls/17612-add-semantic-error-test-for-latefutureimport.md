```yaml
number: 17612
title: Add Semantic Error Test for LateFutureImport
type: pull_request
state: merged
author: maxmynter
labels:
  - internal
assignees: []
merged: true
base: main
head: extend-semantic-test-coverage
created_at: 2025-04-24T16:49:01Z
updated_at: 2025-04-25T12:32:58Z
url: https://github.com/astral-sh/ruff/pull/17612
synced_at: 2026-01-12T15:56:02Z
```

# Add Semantic Error Test for LateFutureImport

---

_@maxmynter_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Adresses a question in #17526.

## Summary
Adds a syntax error test for `__future__` import not at top of file. 

## Question: 
Is this a redundant with https://github.com/astral-sh/ruff/blob/8d2c79276d167fcfcf9143a2bc1b328bb9d0f876/crates/ruff_linter/resources/test/fixtures/pyflakes/F404_0.py#L1-L8 and https://github.com/astral-sh/ruff/blob/8d2c79276d167fcfcf9143a2bc1b328bb9d0f876/crates/ruff_linter/resources/test/fixtures/pyflakes/F404_1.py#L1-L5

which test pyflake `F404`?
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
This is a test
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-04-24 16:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Assigned to @ntBre by @ntBre on 2025-04-24 17:03_

---

_Comment by @ntBre on 2025-04-24 20:21_

Yeah I probably could have left the existing ruff rules out of the issue because they should be covered well by existing tests. I think I added a couple of new tests previously just for convenience or for especially tricky cases not covered by the existing tests.

But it doesn't really hurt to have more tests, so I'll approve and merge this if you want, or we can just mark it covered in the table. Up to you!

This is exactly the kind of PR I had in mind otherwise though, thank you!

---

_Label `internal` added by @ntBre on 2025-04-24 20:21_

---

_Comment by @maxmynter on 2025-04-25 12:08_

Nice, let's merge this then!

I can take care of some additional test cases next week. But will make a new PR for these :) 

---

_Marked ready for review by @maxmynter on 2025-04-25 12:08_

---

_@ntBre approved on 2025-04-25 12:32_

---

_Merged by @ntBre on 2025-04-25 12:32_

---

_Closed by @ntBre on 2025-04-25 12:32_

---
