```yaml
number: 2600
title: Enable PEP 517 builds for unnamed requirements
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/bare-viii
created_at: 2024-03-21T22:16:55Z
updated_at: 2024-03-22T02:46:40Z
url: https://github.com/astral-sh/uv/pull/2600
synced_at: 2026-01-12T16:05:07Z
```

# Enable PEP 517 builds for unnamed requirements

---

_@charliermarsh_

## Summary

This PR enables the source distribution database to be used with unnamed requirements (i.e., URLs without a package name). The (significant) upside here is that we can now use PEP 517 hooks to resolve unnamed requirement metadata _and_ reuse any computation in the cache.

The changes to `crates/uv-distribution/src/source/mod.rs` are quite extensive, but mostly mechanical. The core idea is that we introduce a new `BuildableSource` abstraction, which can either be a distribution, or an unnamed URL:

```rust
/// A reference to a source that can be built into a built distribution.
///
/// This can either be a distribution (e.g., a package on a registry) or a direct URL.
///
/// Distributions can _also_ point to URLs in lieu of a registry; however, the primary distinction
/// here is that a distribution will always include a package name, while a URL will not.
#[derive(Debug, Clone, Copy)]
pub enum BuildableSource<'a> {
    Dist(&'a SourceDist),
    Url(SourceUrl<'a>),
}
```

All the methods on the source distribution database now accept `BuildableSource`. `BuildableSource` has a `name()` method, but it returns `Option<&PackageName>`, and everything is required to work with and without a package name.

The main drawback of this approach (which isn't a terrible one) is that we can no longer include the package name in the cache. (We do continue to use the package name for registry-based distributions, since those always have a name.). The package name was included in the cache route for two reasons: (1) it's nice for debugging; and (2) we use it to power `uv cache clean flask`, to identify the entries that are relevant for Flask.

To solve this, I changed the `uv cache clean` code to look one level deeper. So, when we want to determine whether to remove the cache entry for a given URL, we now look into the directory to see if there are any wheels that match the package name. This isn't as nice, but it does work (and we have test coverage for it -- all passing).

I also considered removing the package name from the cache routes for non-registry _wheels_, for consistency... But, it would require a cache bump, and it didn't feel important enough to merit that.


---

_Review requested from @zanieb by @charliermarsh on 2024-03-21 22:20_

---

_Review requested from @konstin by @charliermarsh on 2024-03-21 22:20_

---

_Label `enhancement` added by @charliermarsh on 2024-03-21 22:20_

---

_Marked ready for review by @charliermarsh on 2024-03-21 22:21_

---

_Comment by @charliermarsh on 2024-03-21 22:21_

This doesn't actually leverage these improvements anywhere, it just enables them.


---

_Review comment by @zanieb on `crates/uv-dispatch/src/lib.rs`:289 on 2024-03-21 22:48_

Hm.. this is an interesting caveat. We should probably mention this in the `--only-binary` docs?

---

_@zanieb reviewed on 2024-03-21 22:48_

---

_@charliermarsh reviewed on 2024-03-21 23:10_

---

_Review comment by @charliermarsh on `crates/uv-dispatch/src/lib.rs`:289 on 2024-03-21 23:10_

Yeah I don’t really see any way around it. It was also already true for editables, interestingly.

---

_@charliermarsh reviewed on 2024-03-21 23:11_

---

_Review comment by @charliermarsh on `crates/uv-dispatch/src/lib.rs`:289 on 2024-03-21 23:11_

We could default to “false” here instead of true. That might be safer? At least for non-editables.

---

_@charliermarsh reviewed on 2024-03-21 23:11_

---

_Review comment by @charliermarsh on `crates/uv-dispatch/src/lib.rs`:289 on 2024-03-21 23:11_

That actually seems better since you can always add a package name to fix it.

---

_@charliermarsh reviewed on 2024-03-21 23:29_

---

_Review comment by @charliermarsh on `crates/uv-dispatch/src/lib.rs`:289 on 2024-03-21 23:29_

TIL that pip install does not enforce `--no-binary` for direct URL requirements. For example, `pip install "anyio @ https://files.pythonhosted.org/packages/db/4d/3970183622f0330d3c23d9b8a5f52e365e50381fd484d08e3285104333d3/anyio-4.3.0.tar.gz" --only-binary anyio --no-cache --force-reinstall` works fine.

---

_Merged by @charliermarsh on 2024-03-22 02:46_

---

_Closed by @charliermarsh on 2024-03-22 02:46_

---

_Branch deleted on 2024-03-22 02:46_

---
