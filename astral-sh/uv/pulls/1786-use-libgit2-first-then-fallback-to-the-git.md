```yaml
number: 1786
title: "Use `libgit2` first then fallback to the `git` command on failure"
type: pull_request
state: closed
author: zanieb
labels:
  - internal
assignees: []
draft: true
base: main
head: zb/git-libgit2-fallback
created_at: 2024-02-20T22:40:27Z
updated_at: 2024-03-04T21:25:35Z
url: https://github.com/astral-sh/uv/pull/1786
synced_at: 2026-01-12T16:04:44Z
```

# Use `libgit2` first then fallback to the `git` command on failure

---

_@zanieb_

Some necessary context in https://github.com/astral-sh/uv/pull/1781.

This should be faster in the happy path but robust to less common authentication schemes. Cargo uses an environment variable to toggle instead of this "fallback" approach. I'm hesitant to require that from our users.

The downside of this is that we will always retry with the `git` command even if the failure is not related to authentication. This could result in confusing logs and is, of course, slower. In practice, I think the error messages from the `git` command are better as they give you something reproducible to test with and have more context, the `libgit2` errors are very opaque.

I've also added a rough way to reuse the discovered correct strategy for fetching (https://github.com/astral-sh/uv/pull/1786/commits/df4382f4d1b415d3c86cd119876734b2596cb311) we could extend this to avoid unnecessary calls to `libgit2` in the future.

We can definitely say this is overkill and just drop fetching with libgit2 entirely in favor of the git CLI. We could also only use the `git` command if we detect authentication via some heuristic?

---

_Review comment by @zanieb on `crates/uv-git/src/source.rs`:67 on 2024-02-20 22:42_

Awkward! Ideally `fetch_with_strategy` would still return `Fetch` and update `self.git` for you, but then it unconditionally takes ownership of `self` which means `fetch` cannot use `self` again to attempt another strategy.

---

_@zanieb reviewed on 2024-02-20 22:43_

---

_@zanieb reviewed on 2024-02-20 23:19_

---

_Review comment by @zanieb on `crates/uv-git/src/source.rs`:67 on 2024-02-20 23:19_

Refactoring this...

---

_@zanieb reviewed on 2024-02-20 23:30_

---

_Review comment by @zanieb on `crates/uv-git/src/source.rs`:67 on 2024-02-20 23:30_

https://github.com/astral-sh/uv/pull/1786/commits/51b38f9a871c42ac6638328718c04630b216e56f

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:1062 on 2024-02-20 23:43_

This feels a little dubious. Is there a better way to merge these errors?

---

_@zanieb reviewed on 2024-02-20 23:43_

---

_Label `internal` added by @zanieb on 2024-02-21 00:18_

---

_@zanieb reviewed on 2024-02-21 00:18_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:958 on 2024-02-21 00:18_

Would need to add filters to this

---

_Comment by @zanieb on 2024-02-21 01:21_

Currently a big problem here is that libgit2 will retry several times before we try git.

---

_Closed by @zanieb on 2024-03-04 21:25_

---

_Comment by @zanieb on 2024-03-04 21:25_

To be explored another day..

---
