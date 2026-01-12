```yaml
number: 185
title: Filter and store all distributions upfront
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/up
created_at: 2023-10-26T01:03:21Z
updated_at: 2023-10-26T13:00:23Z
url: https://github.com/astral-sh/uv/pull/185
synced_at: 2026-01-12T16:03:47Z
```

# Filter and store all distributions upfront

---

_@charliermarsh_

Modifies the resolver to remove any incompatible distributions upfront, and store them in an index by version. This will be necessary to support `--upgrade` semantics.

This actually does cause a meaningful slowdown right now (since we now iterate over all files, even if we otherwise never would've needed to touch them), but we should be able to optimize it out later.

---

_Merged by @charliermarsh on 2023-10-26 01:06_

---

_Closed by @charliermarsh on 2023-10-26 01:06_

---

_Branch deleted on 2023-10-26 01:06_

---

_@konstin reviewed on 2023-10-26 07:24_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver.rs`:510 on 2023-10-26 07:24_

Instead of caching the http reponse, i'd put this map in the cache. If it's still to slow, we can split it into one entry for the list of versions an one entry for each set of files

---

_@konstin reviewed on 2023-10-26 07:26_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver.rs`:511 on 2023-10-26 07:26_

Do we guarantee we visit wheels first here?

---

_@charliermarsh reviewed on 2023-10-26 12:59_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:510 on 2023-10-26 12:59_

If we cache this, we still need to respect HTTP semantics unfortunately — the response is not immutable.

---

_@charliermarsh reviewed on 2023-10-26 13:00_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:511 on 2023-10-26 13:00_

That doesn’t happen here — that’s the responsibility of the CandidateSelector, which does do that, yes.

---
