```yaml
number: 10716
title: Show non-critical Python discovery errors if no other interpreter is found
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/discovery-err
created_at: 2025-01-17T17:25:18Z
updated_at: 2025-01-21T21:11:56Z
url: https://github.com/astral-sh/uv/pull/10716
synced_at: 2026-01-10T11:45:05Z
```

# Show non-critical Python discovery errors if no other interpreter is found

---

_Pull request opened by @zanieb on 2025-01-17 17:25_

Previously, these errors would only be visible in the debug logs as "Skipping bad interpreter ..." which can lead us to making some ridiculous claims like "There is no virtual environment" or "Python is not installed" when really we just failed to query the interpreter for some reason.

We show the first error, sort of arbitrarily — but I think it matches user expectation, i.e., this would be the first Python on your PATH.

Related to #10713 

---

_Label `error messages` added by @zanieb on 2025-01-17 17:25_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:32 on 2025-01-17 17:27_

This looks like a bug in our test context; will address that..

---

_@zanieb reviewed on 2025-01-17 17:27_

---

_@zanieb reviewed on 2025-01-17 17:29_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:32 on 2025-01-17 17:29_

https://github.com/astral-sh/uv/pull/10716/commits/677e4730a55b79abf6059f8de647257ece4bedb5

---

_@zanieb reviewed on 2025-01-17 18:19_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_sync.rs`:66 on 2025-01-17 18:19_

It's not clear if this is a regression or not — the virtual environment is active but was deleted.

I think we can do better here, but separately.

---

_Marked ready for review by @zanieb on 2025-01-17 18:22_

---

_Review requested from @charliermarsh by @zanieb on 2025-01-21 15:00_

---

_Review requested from @konstin by @zanieb on 2025-01-21 15:00_

---

_@zanieb reviewed on 2025-01-21 18:50_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:728 on 2025-01-21 18:50_

These messages move to `find_python_installation` so we can avoid displaying a duplicate "skipping" log if we're going to throw it as an error.

---

_@zanieb reviewed on 2025-01-21 18:51_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:998 on 2025-01-21 18:51_

This is a little annoying... but basically we don't want to show the "skipped" log for the first interpreter if we're going to immediately throw it as an error; but we want to show it in the right position.

---

_@charliermarsh reviewed on 2025-01-21 20:58_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:998 on 2025-01-21 20:58_

It kind of seems ok to me to log it, then throw it as an error, if it makes the code simpler. I'm having some trouble with it as-is. Like, how does this guard against "first error is non-critical, don't know", then "second error is non-critical; show first and second", then we run out of Pythons, exit the loop, and show the first error as an error (again)?

---

_@zanieb reviewed on 2025-01-21 21:02_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:998 on 2025-01-21 21:02_

I'll revert it.

---

_@charliermarsh approved on 2025-01-21 21:04_

---

_@charliermarsh reviewed on 2025-01-21 21:05_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:998 on 2025-01-21 21:05_

It's ok if you want to keep it, just feedback as a reader.

---

_@zanieb reviewed on 2025-01-21 21:10_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:998 on 2025-01-21 21:10_

Naw I was actually confused by the output of `grep` in the integration test and it's not as bad as I thought.

---

_@zanieb reviewed on 2025-01-21 21:10_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:998 on 2025-01-21 21:10_

It was hard to write too.

---

_Merged by @zanieb on 2025-01-21 21:11_

---

_Closed by @zanieb on 2025-01-21 21:11_

---

_Branch deleted on 2025-01-21 21:11_

---
