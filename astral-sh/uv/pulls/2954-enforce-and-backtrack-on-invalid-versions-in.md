```yaml
number: 2954
title: Enforce and backtrack on invalid versions in source metadata
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/docutils
created_at: 2024-04-10T05:03:07Z
updated_at: 2024-04-10T05:13:34Z
url: https://github.com/astral-sh/uv/pull/2954
synced_at: 2026-01-10T14:43:31Z
```

# Enforce and backtrack on invalid versions in source metadata

---

_Pull request opened by @charliermarsh on 2024-04-10 05:03_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

If we build a source distribution from the registry, and the version doesn't match that of the filename, we should error, just as we do for mismatched package names. However, we should also backtrack here, which we didn't previously.

Closes https://github.com/astral-sh/uv/issues/2953.

## Test Plan

Verified that `cargo run pip install docutils --verbose --no-cache --reinstall` installs `docutils==0.21` instead of the invalid `docutils==0.21.post1`.

In the logs, I see:

```
WARN Unable to extract metadata for docutils: Package metadata version `0.21` does not match given version `0.21.post1`
```


---

_Renamed from "Validate built version in source distribution database" to "Enforce and backtrack on invalid versions in source metadata" by @charliermarsh on 2024-04-10 05:03_

---

_Label `enhancement` added by @charliermarsh on 2024-04-10 05:03_

---

_Marked ready for review by @charliermarsh on 2024-04-10 05:03_

---

_Merged by @charliermarsh on 2024-04-10 05:13_

---

_Closed by @charliermarsh on 2024-04-10 05:13_

---

_Branch deleted on 2024-04-10 05:13_

---
