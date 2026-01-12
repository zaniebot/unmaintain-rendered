```yaml
number: 12462
title: Do not bail code action resolution when a quick fix is requested
type: pull_request
state: merged
author: osiewicz
labels:
  - server
assignees: []
merged: true
base: main
head: code-action-resolve-quick-fix-noop
created_at: 2024-07-22T19:18:33Z
updated_at: 2024-07-23T05:00:03Z
url: https://github.com/astral-sh/ruff/pull/12462
synced_at: 2026-01-12T15:55:41Z
```

# Do not bail code action resolution when a quick fix is requested

---

_@osiewicz_

## Summary

When working on improving Ruff integration with Zed I noticed that it errors out when we try to resolve a code action of a `QUICKFIX` kind; apparently, per @dhruvmanila we shouldn't need to resolve it, as the edit is provided in the initial response for the code action. However, it's possible for the `resolve` call to fill out other fields (such as `command`). 
AFAICT Helix also tries to resolve the code actions unconditionally (as in, when either `edit` or `command` is absent); so does VSC. They can still apply the quickfixes though, as they do not error out on a failed call to resolve code actions - Zed does. Following suit on Zed's side does not cut it though, as we still get a log request from Ruff for that failure (which is surfaced in the UI).
There are also other language servers (such as [rust-analyzer](https://github.com/rust-lang/rust-analyzer/blob/c1c9e10f72ffd2e829d20ff1439ff49c2e121731/crates/rust-analyzer/src/handlers/request.rs#L1257)) that fill out both `command` and `edit` fields as a part of code action resolution.

This PR makes the resolve calls for quickfix actions return the input value.

## Test Plan

N/A


---

_Review requested from @dhruvmanila by @charliermarsh on 2024-07-22 19:23_

---

_Comment by @charliermarsh on 2024-07-22 19:23_

Thanks! Assigning to @dhruvmanila.

---

_Label `server` added by @charliermarsh on 2024-07-22 19:23_

---

_@dhruvmanila approved on 2024-07-23 04:58_

I think this makes sense. We do this in `ruff-lsp` as well: https://github.com/astral-sh/ruff-lsp/blob/5488448c23274b9102747f867518b649f9234cfd/ruff_lsp/server.py#L1150-L1175

---

_Renamed from "ruff_server: Do not bail code action resolution when a quick fix is requested" to "Do not bail code action resolution when a quick fix is requested" by @dhruvmanila on 2024-07-23 04:59_

---

_Merged by @dhruvmanila on 2024-07-23 05:00_

---

_Closed by @dhruvmanila on 2024-07-23 05:00_

---
