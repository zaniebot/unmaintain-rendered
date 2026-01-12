```yaml
number: 14591
title: Add social icons to the footer
type: pull_request
state: merged
author: sbrugman
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2024-11-25T18:14:36Z
updated_at: 2024-11-27T13:48:51Z
url: https://github.com/astral-sh/ruff/pull/14591
synced_at: 2026-01-12T15:55:48Z
```

# Add social icons to the footer

---

_@sbrugman_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Consistency with `uv`

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Tested locally. `mkdocs-material` update is required for the `x-twitter` icon.


---

_Label `documentation` added by @zanieb on 2024-11-25 18:20_

---

_Review requested from @dhruvmanila by @zanieb on 2024-11-25 18:20_

---

_@dhruvmanila reviewed on 2024-11-26 03:37_

---

_Review comment by @dhruvmanila on `docs/requirements.txt`:4 on 2024-11-26 03:37_

I think we'll want to keep this in sync with the insider version as well. Currently, our insider version is at a commit where the public version matches `9.5.38`, so can you update it accordingly:

* `mkdocs-material` (public): `9.5.38`
* `mkdocs-material` (insider): 	`39da7a5e761410349e9a1b8abf593b0cdd5453ff`

---

_Review comment by @dhruvmanila on `docs/requirements-insiders.txt`:2 on 2024-11-26 03:38_

Do we need to update Ruff in this PR? I would prefer to avoid making unrelated changes.

---

_@dhruvmanila reviewed on 2024-11-26 03:38_

---

_Renamed from "Docs: add footer socials" to "Add social icons to the footer" by @dhruvmanila on 2024-11-27 11:03_

---

_Merged by @dhruvmanila on 2024-11-27 11:07_

---

_Closed by @dhruvmanila on 2024-11-27 11:07_

---

_Branch deleted on 2024-11-27 13:48_

---
