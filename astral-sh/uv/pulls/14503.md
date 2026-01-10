```yaml
number: 14503
title: Normalize trailing slashes only during lockfile validation
type: pull_request
state: closed
author: jtfmumm
labels:
  - bug
assignees: []
base: main
head: jtfm/normalize-slash-on-check
created_at: 2025-07-08T09:46:13Z
updated_at: 2025-07-09T09:48:47Z
url: https://github.com/astral-sh/uv/pull/14503
synced_at: 2026-01-10T06:53:02Z
```

# Normalize trailing slashes only during lockfile validation

---

_Pull request opened by @jtfmumm on 2025-07-08 09:46_

NOTE: This is a possible alternative to #14387.

We were normalizing index URLs to remove trailing slashes, but this is incorrect behavior for `find-links` URLs (#14367). However, special-casing `find-links` URLs would require that [we stop normalizing URLs when validating lockfiles](https://github.com/astral-sh/uv/pull/14387#issuecomment-3047046094) (or only normalize Simple API URLs at that point). Unfortunately, when parsing lockfiles, we no longer know whether a registry source was a Simple API or `find-links` URL.

The purpose of URL normalization was to prevent lockfile validation failure when the only difference is the trailing slash in an index (#13707). This PR moves all normalization to the lockfile validation stage. This way, when performing ordinary operations, we use the URLs as provided.

Fixes #14367


---

_Label `bug` added by @jtfmumm on 2025-07-08 09:46_

---

_@jtfmumm reviewed on 2025-07-08 09:52_

---

_Review comment by @jtfmumm on `crates/uv-pep508/src/verbatim_url.rs`:199 on 2025-07-08 09:52_

This mostly duplicates the logic found in `UrlString::without_trailing_slash`. We could factor out the shared logic, but I'm not sure which crate it would go on (or if it's really worth creating a new crate if that's necessary).

---

_Review comment by @jtfmumm on `crates/uv-resolver/src/lock/mod.rs`:4832 on 2025-07-08 10:07_

We only currently call `VerbatimUrl::without_trailing_slash` in cases where we need the owned value, so it's not strictly necessary to return a `Cow`. Instead of `VerbatimUrl::from_url(index).without_trailing_slash().into_owned()` it could be something like `VerbatimUrl::from_url_normalized(index)`. 

I haven't made this change because returning a `Cow` could still be beneficial if we end up needing this elsewhere.

---

_@jtfmumm reviewed on 2025-07-08 10:07_

---

_@charliermarsh reviewed on 2025-07-08 16:08_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/file.rs`:181 on 2025-07-08 16:08_

Why is this part necessary?

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/file.rs`:181 on 2025-07-08 17:07_

It's not. Removed

---

_@jtfmumm reviewed on 2025-07-08 17:07_

---

_@zanieb reviewed on 2025-07-08 17:25_

---

_Review comment by @zanieb on `crates/uv-pep508/src/verbatim_url.rs`:199 on 2025-07-08 17:25_

Can't this just operate on `self.url` and use `pop_if_empty` instead of reimplementing it?

---

_Comment by @jtfmumm on 2025-07-09 09:48_

Closing in favor of #14511.

---

_Closed by @jtfmumm on 2025-07-09 09:48_

---
