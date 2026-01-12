```yaml
number: 12725
title: Ignore non-file workspace URL
type: pull_request
state: merged
author: dhruvmanila
labels:
  - server
assignees: []
merged: true
base: main
head: dhruv/ignore-non-workspace-url
created_at: 2024-08-07T05:04:02Z
updated_at: 2024-08-07T09:25:04Z
url: https://github.com/astral-sh/ruff/pull/12725
synced_at: 2026-01-12T15:55:42Z
```

# Ignore non-file workspace URL

---

_@dhruvmanila_

## Summary

This PR updates the server to ignore non-file workspace URL.

This is to avoid crashing the server if the URL scheme is not "file". We'd still raise an error if the URL to file path conversion fails.

Also, as per the docs of [`to_file_path`](https://docs.rs/url/2.5.2/url/struct.Url.html#method.to_file_path):

> Note: This does not actually check the URL’s scheme, and may give nonsensical results for other schemes. It is the user’s responsibility to check the URL’s scheme before calling this.

resolves: #12660

## Test Plan

I'm not sure how to test this locally but the change is small enough to validate on its own.


---

_Label `server` added by @dhruvmanila on 2024-08-07 05:04_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-08-07 05:06_

---

_Comment by @github-actions[bot] on 2024-08-07 05:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_server/src/session/index.rs`:204 on 2024-08-07 05:48_

Logging an info message is probably fine, but I could see users being confused by ruff isn't working in a workspace. What do you think of showing a warning message?

---

_@MichaReiser approved on 2024-08-07 05:48_

---

_@dhruvmanila reviewed on 2024-08-07 08:01_

---

_Review comment by @dhruvmanila on `crates/ruff_server/src/session/index.rs`:204 on 2024-08-07 08:01_

Yeah, that seems reasonable.

---

_Merged by @dhruvmanila on 2024-08-07 09:15_

---

_Closed by @dhruvmanila on 2024-08-07 09:15_

---

_Branch deleted on 2024-08-07 09:15_

---
