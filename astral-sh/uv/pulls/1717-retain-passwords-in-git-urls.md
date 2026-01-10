```yaml
number: 1717
title: Retain passwords in Git URLs
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/git-auth-pat
created_at: 2024-02-19T20:12:37Z
updated_at: 2024-02-21T00:12:58Z
url: https://github.com/astral-sh/uv/pull/1717
synced_at: 2026-01-10T15:33:24Z
```

# Retain passwords in Git URLs

---

_Pull request opened by @zanieb on 2024-02-19 20:12_

Fixes handling of GitHub PATs in HTTPS URLs, which were otherwise dropped. We now supporting the following authentication schemes:

```
git+https://<user>:<token>/...
git+https://<token>/...
```

On Windows, the username is required. We can consider adding a special-case for this in the future, but this just matches libgit2's behavior.

I tested with fine-grained tokens, OAuth tokens, and "classic" tokens. There's test coverage for fine-grained tokens in CI where we use a real private repository and PAT. Yes, the PAT is committed to make this test usable by anyone. It has read-only permissions to the single repository, expires Feb 1 2025, and is in an isolated organization and GitHub account.

Does not yet address SSH authentication (see #1781)

Related:
- https://github.com/astral-sh/uv/issues/1514
- https://github.com/astral-sh/uv/issues/1452

---

_Label `bug` added by @zanieb on 2024-02-19 20:12_

---

_Comment by @zanieb on 2024-02-19 20:33_

Interesting this works locally but not in CI. I'm not sure why it's reaching out to the credentials helper when the PAT is in the URL?

Perhaps relevant.

- https://github.com/rust-lang/cargo/issues/5227

---

_Comment by @zanieb on 2024-02-19 20:56_

So the reason this was failing is because the PAT was being read as a username instead of a password. We'll need to figure out a long term solution for supporting authentication without passwords.

Separately, it looks like these test packages are not being installed correctly (they are missing from site-packages).

---

_Review comment by @zanieb on `crates/cache-key/src/canonical_url.rs`:118 on 2024-02-20 18:56_

This is awkward, is there a better pattern?

---

_@zanieb reviewed on 2024-02-20 18:56_

---

_Marked ready for review by @zanieb on 2024-02-20 18:57_

---

_Review requested from @charliermarsh by @zanieb on 2024-02-20 20:34_

---

_@charliermarsh reviewed on 2024-02-20 23:20_

---

_Review comment by @charliermarsh on `crates/cache-key/src/canonical_url.rs`:118 on 2024-02-20 23:20_

I'll play with it, I'm not sure...

---

_@charliermarsh reviewed on 2024-02-20 23:24_

---

_Review comment by @charliermarsh on `crates/cache-key/src/canonical_url.rs`:118 on 2024-02-20 23:24_

One option:

```rust
if url.scheme().starts_with("git+") {
    if let Some(prefix) = url.path().rsplit_once('@').map(|(prefix, _suffix)| prefix.to_string()) {
        url.set_path(&prefix);
    }
}
```

---

_@charliermarsh approved on 2024-02-20 23:25_

---

_@zanieb reviewed on 2024-02-21 00:07_

---

_Review comment by @zanieb on `crates/cache-key/src/canonical_url.rs`:118 on 2024-02-21 00:07_

Still kind of strange but it seems better.

---

_@zanieb reviewed on 2024-02-21 00:07_

---

_Review comment by @zanieb on `crates/cache-key/src/canonical_url.rs`:118 on 2024-02-21 00:07_

Thanks!

---

_@charliermarsh reviewed on 2024-02-21 00:09_

---

_Review comment by @charliermarsh on `crates/cache-key/src/canonical_url.rs`:118 on 2024-02-21 00:09_

Like... mildly better because it doesn't clone the suffix, I guess, and it's contained without another binding.

---

_@zanieb reviewed on 2024-02-21 00:10_

---

_Review comment by @zanieb on `crates/cache-key/src/canonical_url.rs`:118 on 2024-02-21 00:10_

In the other case we need to clone the suffix too lol

---

_Review comment by @zanieb on `crates/cache-key/src/canonical_url.rs`:118 on 2024-02-21 00:10_

https://github.com/astral-sh/uv/pull/1717/commits/ae3ba9df5a6f13b8c291387547a9f2f3dfa8cc8e

---

_@zanieb reviewed on 2024-02-21 00:10_

---

_@zanieb reviewed on 2024-02-21 00:10_

---

_Review comment by @zanieb on `crates/cache-key/src/canonical_url.rs`:118 on 2024-02-21 00:10_

The cloned original URL bit is weird though I prefer this

---

_Merged by @zanieb on 2024-02-21 00:12_

---

_Closed by @zanieb on 2024-02-21 00:12_

---

_Branch deleted on 2024-02-21 00:12_

---
