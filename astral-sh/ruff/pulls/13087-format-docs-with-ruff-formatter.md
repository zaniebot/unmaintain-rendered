```yaml
number: 13087
title: Format docs with ruff formatter
type: pull_request
state: merged
author: calumy
labels:
  - ci
assignees: []
merged: true
base: main
head: format-with-ruff
created_at: 2024-08-24T16:24:45Z
updated_at: 2024-08-26T16:04:02Z
url: https://github.com/astral-sh/ruff/pull/13087
synced_at: 2026-01-10T21:38:32Z
```

# Format docs with ruff formatter

---

_Pull request opened by @calumy on 2024-08-24 16:24_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Now that Ruff provides a formatter, there is no need to rely on Black to check that the docs are formatted correctly in `check_docs_formatted.py`. This PR swaps out Black for the Ruff formatter and updates inconsistencies between the two. 

This PR will be a precursor to another PR ([branch](https://github.com/calumy/ruff/tree/format-pyi-in-docs)), updating the `check_docs_formatted.py` script to check for pyi files, fixing #11568.

## Test Plan

- CI to check that the docs are formatted correctly using the updated script.


---

_Comment by @github-actions[bot] on 2024-08-24 16:43_

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

_Review comment by @dhruvmanila on `scripts/check_docs_formatted.py`:141 on 2024-08-26 04:50_

I think we should use stdin instead of a temp file. You can find an example here:

https://github.com/astral-sh/ruff/blob/5af48337a566b493a8263ff4c641462035a3c558/scripts/fuzz-parser/fuzz.py#L41-L46

This removes the need for handling temp files :)

---

_Review comment by @dhruvmanila on `scripts/check_docs_formatted.py`:299 on 2024-08-26 04:52_

Should we instead use the latest released version? That way we don't need to wait for the build to complete. This would be added to the requirements file.

---

_@dhruvmanila reviewed on 2024-08-26 04:53_

---

_Label `ci` added by @dhruvmanila on 2024-08-26 04:53_

---

_@calumy reviewed on 2024-08-26 14:27_

---

_Review comment by @calumy on `scripts/check_docs_formatted.py`:141 on 2024-08-26 14:27_

Updated in 11ef36c8d

---

_@calumy reviewed on 2024-08-26 14:33_

---

_Review comment by @calumy on `scripts/check_docs_formatted.py`:299 on 2024-08-26 14:33_

My reason for not using the latest released version of Ruff was to avoid the situation where an update to the formatter results in an update to the docs. This could potentially result in the docs being formatted differently from the current release version of ruff and would only be fixed in a subsequent PR and release; however, using the current branch's version of the formatter allows the docs to be updated at the same time as the formatter by someone who has the context of the change. 

If this is not a concern, let me know and I can update to use the latest release. 

---

_Review requested from @dhruvmanila by @calumy on 2024-08-26 14:34_

---

_Review comment by @dhruvmanila on `scripts/check_docs_formatted.py`:299 on 2024-08-26 14:55_

I think any major style updates would only go out in a breaking release and will be stabilized once per year (similar to how Black does it). So, it shouldn't be too much work to update the docs then ;)

---

_@dhruvmanila reviewed on 2024-08-26 14:55_

---

_@calumy reviewed on 2024-08-26 15:09_

---

_Review comment by @calumy on `scripts/check_docs_formatted.py`:299 on 2024-08-26 15:09_

Updated in: b3ac44d92ca62e3f6cc7096c565eeaca994aab1b

---

_@dhruvmanila approved on 2024-08-26 15:48_

Thanks!

---

_Comment by @dhruvmanila on 2024-08-26 15:49_

Re-running the failed jobs, not sure what happened there

---

_Merged by @dhruvmanila on 2024-08-26 15:55_

---

_Closed by @dhruvmanila on 2024-08-26 15:55_

---
