```yaml
number: 12917
title: Check dist name to handle bogus redirect
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/check-distribution-name
created_at: 2025-04-16T13:20:27Z
updated_at: 2025-04-22T15:36:29Z
url: https://github.com/astral-sh/uv/pull/12917
synced_at: 2026-01-12T16:10:27Z
```

# Check dist name to handle bogus redirect

---

_@konstin_

When an index performs a bogus redirect or otherwise returns a different distribution name than expected, uv currently hangs.

In the example case, requesting the simple index page for any package returns the page for anyio. This mean querying the sniffio version map returns only anyio entries, and the version maps resolves to an anyio version. When the resolver makes a query for sniffio and waits for it to resolve, the main thread finds an anyio and resolves only that in the wait map, causing the hang.

We fix this by checking the name of the returned distribution against the name of the requested distribution. For good measure, we add the same check in `Request::Dist` and `Request::Installed`. For performance and complexity reasons, we don't perform this check in the version map itself, but only after a candidate distribution has been selected.

---

_Label `bug` added by @konstin on 2025-04-16 13:20_

---

_@zanieb reviewed on 2025-04-16 14:50_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:10362 on 2025-04-16 14:50_

Should we include the URL in this error message? It's not clear to me what's going on here as a user.

---

_@zanieb reviewed on 2025-04-16 14:50_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:10362 on 2025-04-16 14:50_

Should we include the URL in this error message? It's not clear to me what's going on here as a user.

---

_@konstin reviewed on 2025-04-16 21:04_

---

_Review comment by @konstin on `crates/uv/tests/it/pip_install.rs`:10362 on 2025-04-16 21:04_

At the point where we're observing the mismatch, we don't have the offending URL anymore (it's not the dist URL, it's the index page URL that list the dist for the wrong package). I don't think it's worth the effort adding it given that this has not been reported for any real index (we only observed this in a bogus test case) and it's not user fixable but an index problem. The usual better approach, eager checking after receiving an index page, would be prohibitively expensive in the version map as it's incompatible with the lazy parsing. We could perform this check in the version map selector, but then would need to start tracking the index page URL for each `VersionMap` instance, which is imho too much complexity and performance risk to be worthwhile here.

I've added a sentence about this being an index problem.

---

_@zanieb reviewed on 2025-04-16 22:25_

---

_Review comment by @zanieb on `crates/uv-resolver/src/error.rs`:127 on 2025-04-16 22:25_

nit
```suggestion
    #[error("The index returned metadata for the wrong package: expected {request} for {expected}, got {request} for {actual}")]
```

---

_@zanieb approved on 2025-04-16 22:25_

---

_Merged by @konstin on 2025-04-22 15:36_

---

_Closed by @konstin on 2025-04-22 15:36_

---

_Branch deleted on 2025-04-22 15:36_

---
