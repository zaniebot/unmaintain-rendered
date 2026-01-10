```yaml
number: 9098
title: Defer reporting of build failures in resolver
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/err
created_at: 2024-11-13T20:29:59Z
updated_at: 2024-11-14T13:39:49Z
url: https://github.com/astral-sh/uv/pull/9098
synced_at: 2026-01-10T12:00:00Z
```

# Defer reporting of build failures in resolver

---

_Pull request opened by @charliermarsh on 2024-11-13 20:29_

## Summary

In https://github.com/astral-sh/uv/issues/9078, resolution fails because we fail to build `jsmin`. However... if you look at what's actually happening, `jsmin` fails to build during _prefetching_. And we never actually attempt to access its metadata later on.

This PR modifies the metadata result handling such that we don't raise these errors until the resolver actually asks for the metadata, so https://github.com/astral-sh/uv/issues/9078 now succeeds.

I actually had to make this change anyway in pursuing https://github.com/astral-sh/uv/issues/8962, so I've decided to carve it out here.

Closes https://github.com/astral-sh/uv/issues/9078.


---

_Label `bug` added by @charliermarsh on 2024-11-13 20:30_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-11-13 20:30_

---

_Review requested from @zanieb by @charliermarsh on 2024-11-13 20:30_

---

_Review requested from @konstin by @charliermarsh on 2024-11-13 20:30_

---

_@charliermarsh reviewed on 2024-11-13 20:30_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/error.rs`:100 on 2024-11-13 20:30_

I don't think I can avoid this `Arc`... I need to be able to clone this, because it's stored in the `OnceMap`, but then needs to be cloned to be returned as a `Result` in the resolver. And `uv_distribution::Error` doesn't implement `Clone`.

---

_@charliermarsh reviewed on 2024-11-13 20:31_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/provider.rs`:219 on 2024-11-13 20:31_

This method now _never_returns an error. But I left it as a `Result` since it's implementing the provider trait, and different clients could do different things here?

---

_Comment by @charliermarsh on 2024-11-13 20:32_

To clarify: in general, we will still fail the resolution if a build fails. But we won't fail if a build fails during prefetching, and that package is then never requested while resolving.

---

_Comment by @zanieb on 2024-11-13 20:47_

I thought @konstin had already done this in a previous pull request, but perhaps I'm misremembering.

---

_@zanieb approved on 2024-11-13 20:48_

---

_Merged by @charliermarsh on 2024-11-13 20:49_

---

_Closed by @charliermarsh on 2024-11-13 20:49_

---

_Branch deleted on 2024-11-13 20:49_

---

_@BurntSushi reviewed on 2024-11-13 21:49_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/error.rs`:100 on 2024-11-13 21:49_

`Arc` inside an error is totally natural: https://github.com/BurntSushi/jiff/blob/16376c7175bca14a8ccb37a178bde92598af1a5f/src/error.rs#L47-L57

The main alternative here, and one I would suggest if possible, is to make `uv_distribution::Error` embed an `Arc` internally so that it can implement `Clone` itself. Then you don't need to spread `Arc`s out everywhere else.

---

_Comment by @konstin on 2024-11-14 12:23_

> I thought @konstin had already done this in a previous pull request, but perhaps I'm misremembering.

I'm glad it's not just me remembering this

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/provider.rs`:219 on 2024-11-14 13:39_

SGTM

---

_@BurntSushi reviewed on 2024-11-14 13:39_

LGTM

---
