```yaml
number: 1621
title: Avoid early error for transistive constraints
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
base: main
head: charlie/constraint
created_at: 2024-02-18T02:24:55Z
updated_at: 2024-02-21T18:13:51Z
url: https://github.com/astral-sh/uv/pull/1621
synced_at: 2026-01-10T14:54:43Z
```

# Avoid early error for transistive constraints

---

_Pull request opened by @charliermarsh on 2024-02-18 02:24_

## Summary

If a constraint causes some requirement in a dependency to yield the empty set, we should avoid early termination. (I believe we do this right now because the error messages are better.) This is different than if you have (e.g.) two direct requirements with conflicting versions, which _should_ error immediately.

Closes https://github.com/astral-sh/uv/issues/1522.

---

_Review requested from @zanieb by @charliermarsh on 2024-02-18 02:24_

---

_@charliermarsh reviewed on 2024-02-18 02:25_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:3594 on 2024-02-18 02:25_

I need to improve this case before merging.

---

_Comment by @charliermarsh on 2024-02-18 02:25_

@zanieb - I think this is the thing that would've been solved by that PR you had at one point to allow returning multiple versions (i.e., to avoid this eager merging).

---

_Label `bug` added by @charliermarsh on 2024-02-18 02:25_

---

_Comment by @zanieb on 2024-02-18 07:53_

I'm not sure which one you're referring to anymore :)

---

_Comment by @charliermarsh on 2024-02-18 12:59_

@zanieb - Found it! https://github.com/astral-sh/uv/pull/383

---

_Comment by @zanieb on 2024-02-18 21:17_

Oh gosh we probably still want that don't we... 😭 

---

_Comment by @charliermarsh on 2024-02-18 21:20_

Yeah I think it would indirectly solve this problem

---

_Comment by @charliermarsh on 2024-02-18 21:20_

(Like in theory it would make this PR redundant and avoid the bad error message I commented on in the PR.)

---

_Comment by @zanieb on 2024-02-18 21:21_

I think the main blocker there was the URL dependency stuff and I think you had a better solution for that than me? Maybe you should take a look at it?

---

_Comment by @charliermarsh on 2024-02-19 18:03_

@zanieb - Yeah I can take this on. I have to prioritize some other things over it first. Do you think this is worth merging as-is even with the bad error message? Defer to you on that.

---

_@zanieb approved on 2024-02-19 18:15_

I'll take the incremental improvement.

Is there a test case covering a successful resolution for the bug? It looks like the new test case does not succeed?

---

_Comment by @charliermarsh on 2024-02-21 18:13_

Closing in favor of https://github.com/astral-sh/uv/pull/1796.

---

_Closed by @charliermarsh on 2024-02-21 18:13_

---
