```yaml
number: 18262
title: "[`flake8_async`] Fix sleep argument name for anyio (`ASYNC115`, `ASYNC116`)"
type: pull_request
state: merged
author: Vasanth-96
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix/async115_and_async116_name_resolution
created_at: 2025-05-22T19:01:07Z
updated_at: 2025-05-26T09:53:18Z
url: https://github.com/astral-sh/ruff/pull/18262
synced_at: 2026-01-12T15:56:15Z
```

# [`flake8_async`] Fix sleep argument name for anyio (`ASYNC115`, `ASYNC116`)

---

_@Vasanth-96_

#18164
<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fix incorrect argument name detection for `anyio.sleep` and `trio.sleep` in ASYNC115 lint check.  
The linter previously assumed the keyword argument for sleep duration was always `seconds`, causing false positives when `delay` was used with `anyio.sleep`.  
This update correctly distinguishes between `seconds` (Trio) and `delay` (AnyIO), improving accuracy.

Fixes https://github.com/astral-sh/ruff/issues/18164

## Test Plan

- Added test cases covering both `anyio.sleep(delay=0)` and `trio.sleep(seconds=0)` usage.  
- Verified ASYNC115 warnings are correctly triggered or suppressed based on argument names.  
- Manual testing with examples from the linked issue.


---

_Comment by @MichaReiser on 2025-05-23 20:36_

Would you mind filling in the summary and test plan section of your PR? It would help us review your change

---

_Comment by @Vasanth-96 on 2025-05-24 09:54_

Apologies for the delay — I got caught up with work and wasn’t able to write the tests or summary yet. I’ll update it shortly.

---

_@MichaReiser requested changes on 2025-05-25 11:14_

Thanks and also thank you for updating the summary. It helped me a lot to this PR a lot! 

I think we should change the added tests because they currently wouldn't catch a regression (they're all okay). I think it's as easy as changing the argument provided to `sleep`/`delay` to `0` (which makes the rule trigger). 

---

_Comment by @Vasanth-96 on 2025-05-25 13:49_

done @MichaReiser thank you very much for the review, have updated the tests.

---

_Label `bug` added by @MichaReiser on 2025-05-26 08:09_

---

_Label `rule` added by @MichaReiser on 2025-05-26 08:09_

---

_@MichaReiser approved on 2025-05-26 08:43_

---

_Closed by @MichaReiser on 2025-05-26 09:24_

---

_Reopened by @MichaReiser on 2025-05-26 09:24_

---

_Comment by @github-actions[bot] on 2025-05-26 09:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[flake8_async] Refactor argument name resolution for async sleep func…" to "[`flake8_async`] Fix sleep argument name for anyio (`ASYNC115`, `ASYNC116`)" by @MichaReiser on 2025-05-26 09:51_

---

_Merged by @MichaReiser on 2025-05-26 09:53_

---

_Closed by @MichaReiser on 2025-05-26 09:53_

---
