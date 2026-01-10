```yaml
number: 2449
title: Apply from-URL credentials in authentication middleware
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/auth
created_at: 2024-03-14T03:57:19Z
updated_at: 2024-03-15T16:21:38Z
url: https://github.com/astral-sh/uv/pull/2449
synced_at: 2026-01-10T14:49:08Z
```

# Apply from-URL credentials in authentication middleware

---

_Pull request opened by @charliermarsh on 2024-03-14 03:57_

## Summary

Right now, the middleware doesn't apply credentials that were _originally_ sourced from a URL. This requires that we call `with_url_encoded_auth` whenever we create a request to ensure that any credentials that were passed in as part of an index URL (for example) are respected.

This PR modifies `uv-auth` to instead apply those credentials in the middleware itself. This seems preferable to me. As far as I can tell, we can _only_ add in-URL credentials to the store ourselves (since in-URL credentials are converted to headers by the time they reach the middleware). And if we ever _didn't_ apply those credentials to new URLs, it'd be a bug in the logic that precedes the middleware (i.e., us forgetting to call `with_url_encoded_auth`).

## Test Plan

`cargo run pip install` with an authenticated index.


---

_Review requested from @zanieb by @charliermarsh on 2024-03-14 03:57_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/file.rs`:62 on 2024-03-14 03:58_

The real win here is we don't have to think about where / when all the auth is preserved. The middleware deals with it.

---

_@charliermarsh reviewed on 2024-03-14 03:58_

---

_Label `internal` added by @charliermarsh on 2024-03-14 04:10_

---

_Merged by @zanieb on 2024-03-15 16:21_

---

_Closed by @zanieb on 2024-03-15 16:21_

---

_Branch deleted on 2024-03-15 16:21_

---
