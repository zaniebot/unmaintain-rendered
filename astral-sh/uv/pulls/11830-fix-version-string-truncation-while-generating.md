```yaml
number: 11830
title: Fix version string truncation while generating cache_key
type: pull_request
state: merged
author: nkitsaini
labels: []
assignees: []
merged: true
base: main
head: fix-version-truncation
created_at: 2025-02-27T11:30:55Z
updated_at: 2025-02-27T14:16:06Z
url: https://github.com/astral-sh/uv/pull/11830
synced_at: 2026-01-12T16:10:01Z
```

# Fix version string truncation while generating cache_key

---

_@nkitsaini_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Follow up for https://github.com/astral-sh/uv/pull/11738

I missed this while reviewing the truncation changes. 

`format!("{:.N}", value)` only truncates if the `fmt::Display` implementation supports it (by reading `f.precision()` in trait implementation).

So in our case `format!("{:.N}", version.to_string())` will work but not `format!("{:.N}", version)` unless `Version` supports it.

Since we only need it once, I am just truncating after the string is created. 

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @charliermarsh by @konstin on 2025-02-27 12:50_

---

_@charliermarsh approved on 2025-02-27 13:48_

Thanks.

---

_Merged by @charliermarsh on 2025-02-27 13:48_

---

_Closed by @charliermarsh on 2025-02-27 13:48_

---

_Branch deleted on 2025-02-27 14:16_

---
