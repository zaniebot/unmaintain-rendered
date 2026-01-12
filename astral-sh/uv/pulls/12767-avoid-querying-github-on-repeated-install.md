```yaml
number: 12767
title: Avoid querying GitHub on repeated install invocations
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/gh
created_at: 2025-04-09T01:34:09Z
updated_at: 2025-04-09T02:00:42Z
url: https://github.com/astral-sh/uv/pull/12767
synced_at: 2026-01-12T16:10:23Z
```

# Avoid querying GitHub on repeated install invocations

---

_@charliermarsh_

## Summary

If you run `cargo run pip install "pip-test-package @ git+https://github.com/pypa/pip-test-package@5547fa909e83df8bd743d3978d6667497983a4b7"` repeatedly, then every time, we'll take the "GitHub fast path" every time, even if the package is already cached. This PR adds logic to skip the fast path if the reference looks like a commit that we've already checked out.

Closes https://github.com/astral-sh/uv/issues/12760.


---

_Label `performance` added by @charliermarsh on 2025-04-09 01:34_

---

_Review requested from @zanieb by @charliermarsh on 2025-04-09 01:34_

---

_Review requested from @konstin by @charliermarsh on 2025-04-09 01:34_

---

_Comment by @charliermarsh on 2025-04-09 01:53_

I think there's a better fix here, which is: we move all this metadata querying into `uv-git`. So we have a method like `fetch_metadata` that looks like `fetch`, but is "allowed" to short-circuit by getting the metadata. It's much more involved, though.

---

_Merged by @charliermarsh on 2025-04-09 02:00_

---

_Closed by @charliermarsh on 2025-04-09 02:00_

---

_Branch deleted on 2025-04-09 02:00_

---
