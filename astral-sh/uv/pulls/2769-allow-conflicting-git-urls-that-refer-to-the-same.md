```yaml
number: 2769
title: Allow conflicting Git URLs that refer to the same commit SHA
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/allowed-urls
created_at: 2024-04-02T02:27:28Z
updated_at: 2024-04-02T23:36:36Z
url: https://github.com/astral-sh/uv/pull/2769
synced_at: 2026-01-12T16:05:12Z
```

# Allow conflicting Git URLs that refer to the same commit SHA

---

_@charliermarsh_

## Summary

This PR leverages our lookahead direct URL resolution to significantly improve the range of Git URLs that we can accept (e.g., if a user provides the same requirement, once as a direct dependency, and once as a tag). We did some of this in #2285, but the solution here is more general and works for arbitrary transitive URLs.

Closes https://github.com/astral-sh/uv/issues/2614.


---

_Marked ready for review by @charliermarsh on 2024-04-02 02:27_

---

_Label `enhancement` added by @charliermarsh on 2024-04-02 02:27_

---

_Review requested from @zanieb by @zanieb on 2024-04-02 02:52_

---

_Comment by @zanieb on 2024-04-02 02:52_

Nice. I'll give this a real read in the morning.

---

_Comment by @charliermarsh on 2024-04-02 02:53_

Thanks :)

---

_@zanieb reviewed on 2024-04-02 21:18_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:1676 on 2024-04-02 21:18_

Is there a test case like this for a commit that does not match a tag? Do we display the commit that the tag resolves to in that case?

---

_@zanieb approved on 2024-04-02 21:18_

Way better! Minor comment regarding a test

---

_@charliermarsh reviewed on 2024-04-02 22:19_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:1676 on 2024-04-02 22:19_

I don't _think_ so. I could add that, though.

---

_@zanieb reviewed on 2024-04-02 22:47_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:1676 on 2024-04-02 22:47_

I'd at least like a snapshot, we could consider improving the output in the future 

---

_@charliermarsh reviewed on 2024-04-02 23:18_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:1676 on 2024-04-02 23:18_

Sorry, yes, we do already have this (`incompatible_narrowed_url_dependency`).

---

_Merged by @charliermarsh on 2024-04-02 23:36_

---

_Closed by @charliermarsh on 2024-04-02 23:36_

---

_Branch deleted on 2024-04-02 23:36_

---
