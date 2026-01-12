```yaml
number: 14824
title: Add derivation chains for dependency errors
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/derive
created_at: 2025-07-22T17:33:37Z
updated_at: 2025-07-22T19:09:01Z
url: https://github.com/astral-sh/uv/pull/14824
synced_at: 2026-01-12T16:11:26Z
```

# Add derivation chains for dependency errors

---

_@charliermarsh_

## Summary

This PR adds derivation chain for another class of resolver failures. For example, if we encounter a transitive URL dependency, we now tell the user which package included it, and the full derivation chain:

```
  × Failed to resolve dependencies for `foo` (v0.1.0)
  ╰─▶ Package `flask` was included as a URL dependency. URL dependencies must be
      expressed as direct requirements or constraints. Consider adding `flask @
      https://files.pythonhosted.org/packages/3d/68/9d4508e893976286d2ead7f8f571314af6c2037af34853a30fd769c02e9d/flask-3.1.1-py3-none-any.whl`
      to your dependencies or constraints file.
  help: `foo` (v0.1.0) was included because `baz` (v0.1.0) depends on `foo`
```

Closes #14795.


---

_Label `error messages` added by @charliermarsh on 2025-07-22 17:33_

---

_Review requested from @zanieb by @charliermarsh on 2025-07-22 17:43_

---

_Marked ready for review by @charliermarsh on 2025-07-22 17:43_

---

_@zanieb reviewed on 2025-07-22 19:03_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:3863 on 2025-07-22 19:03_

Should it be `unwrap_or_default()` or `return error`?

---

_@zanieb reviewed on 2025-07-22 19:04_

---

_Review comment by @zanieb on `crates/uv/tests/it/branching_urls.rs`:71 on 2025-07-22 19:04_

A little sad to see usage of miette increase rather than decrease, but it's fine.

---

_@zanieb approved on 2025-07-22 19:05_

---

_@charliermarsh reviewed on 2025-07-22 19:08_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/branching_urls.rs`:71 on 2025-07-22 19:08_

I have a branch that removes all of our usages. I can push it.

---

_Merged by @charliermarsh on 2025-07-22 19:08_

---

_Closed by @charliermarsh on 2025-07-22 19:08_

---

_Branch deleted on 2025-07-22 19:08_

---

_@charliermarsh reviewed on 2025-07-22 19:09_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:3863 on 2025-07-22 19:09_

This is what we do at other usage sites, I can't quite remember, this might be when you're looking at a root dependency. We just don't render the hint if it's empty (later).

---
