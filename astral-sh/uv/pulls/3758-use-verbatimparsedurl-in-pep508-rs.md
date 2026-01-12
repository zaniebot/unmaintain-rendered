```yaml
number: 3758
title: "Use `VerbatimParsedUrl` in `pep508_rs`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/parse-url-in-pep508-rs
created_at: 2024-05-22T19:17:34Z
updated_at: 2024-05-23T21:01:41Z
url: https://github.com/astral-sh/uv/pull/3758
synced_at: 2026-01-12T16:05:50Z
```

# Use `VerbatimParsedUrl` in `pep508_rs`

---

_@konstin_

When parsing requirements from any source, directly parse the url parts (and reject unsupported urls) instead of parsing url parts at a later stage. This removes a bunch of error branches and concludes the work parsing url parts once and passing them around everywhere.

Many usages of the assembled `VerbatimUrl` remain, but these can be removed incrementally.

Please review commit-by-commit.

---

_Label `internal` added by @konstin on 2024-05-22 19:17_

---

_Comment by @charliermarsh on 2024-05-23 14:31_

I will review today.

---

_Review requested from @charliermarsh by @charliermarsh on 2024-05-23 14:31_

---

_@charliermarsh reviewed on 2024-05-23 19:36_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:5463 on 2024-05-23 19:36_

My only concern here is: isn't this a valid PEP 508 URL? It's just that uv doesn't support it?

---

_@charliermarsh reviewed on 2024-05-23 19:39_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:5463 on 2024-05-23 19:39_

Oh I see, I guess this only appears because we parameterize the type on `VerbatimParsedUrl` instead of `VerbatimUrl`?

---

_@charliermarsh approved on 2024-05-23 19:45_

---

_Comment by @charliermarsh on 2024-05-23 19:46_

Is the intent to remove `VerbatimUrl` entirely and just store `given` on `VerbatimParsedUrl`?

---

_Merged by @charliermarsh on 2024-05-23 19:52_

---

_Closed by @charliermarsh on 2024-05-23 19:52_

---

_Branch deleted on 2024-05-23 19:52_

---

_@konstin reviewed on 2024-05-23 20:59_

---

_Review comment by @konstin on `crates/uv/tests/pip_compile.rs`:5463 on 2024-05-23 20:59_

Yes i've pushed to parsing down. If we decide wanted to support preinstalled urls we don't support, we can add an `Unknown` variant to `VerbatimParsedUrl`, moving some of the error handling back later again.


---

_Comment by @konstin on 2024-05-23 21:01_

Yes, this is tracked in https://github.com/astral-sh/uv/issues/3410.

---
