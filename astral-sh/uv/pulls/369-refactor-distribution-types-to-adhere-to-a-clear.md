```yaml
number: 369
title: Refactor distribution types to adhere to a clear hierarchy
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/source
created_at: 2023-11-08T21:37:13Z
updated_at: 2023-11-10T02:45:42Z
url: https://github.com/astral-sh/uv/pull/369
synced_at: 2026-01-10T15:50:28Z
```

# Refactor distribution types to adhere to a clear hierarchy

---

_Pull request opened by @charliermarsh on 2023-11-08 21:37_

## Summary

This PR refactors our `RemoteDistribution` type such that it now follows a clear hierarchy that matches the actual variants, and encodes the differences between source and built distributions:

```rust
pub enum Distribution {
    Built(BuiltDistribution),
    Source(SourceDistribution),
}

pub enum BuiltDistribution {
    Registry(RegistryBuiltDistribution),
    DirectUrl(DirectUrlBuiltDistribution),
}

pub enum SourceDistribution {
    Registry(RegistrySourceDistribution),
    DirectUrl(DirectUrlSourceDistribution),
    Git(GitSourceDistribution),
}

/// A built distribution (wheel) that exists in a registry, like `PyPI`.
pub struct RegistryBuiltDistribution {
    pub name: PackageName,
    pub version: Version,
    pub file: File,
}

/// A built distribution (wheel) that exists at an arbitrary URL.
pub struct DirectUrlBuiltDistribution {
    pub name: PackageName,
    pub url: Url,
}

/// A source distribution that exists in a registry, like `PyPI`.
pub struct RegistrySourceDistribution {
    pub name: PackageName,
    pub version: Version,
    pub file: File,
}

/// A source distribution that exists at an arbitrary URL.
pub struct DirectUrlSourceDistribution {
    pub name: PackageName,
    pub url: Url,
}

/// A source distribution that exists in a Git repository.
pub struct GitSourceDistribution {
    pub name: PackageName,
    pub url: Url,
}
```

Most of the PR just stems downstream from this change. There are no behavioral changes, so I'm largely relying on lint, tests, and the compiler for correctness.

---

_@charliermarsh reviewed on 2023-11-08 21:39_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/file.rs`:11 on 2023-11-08 21:39_

We should consider making this part of the `Distribution` API.

---

_Review requested from @konstin by @charliermarsh on 2023-11-08 21:41_

---

_Review requested from @zanieb by @charliermarsh on 2023-11-08 21:41_

---

_@charliermarsh reviewed on 2023-11-08 21:42_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/registry_index.rs`:14 on 2023-11-08 21:42_

As an example of the kind of thing we can now enforce: the `RegistryIndex` only contains distributions that were derived from a registry (rather than a direct URL).

---

_@charliermarsh reviewed on 2023-11-08 21:43_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/distribution/built_distribution.rs`:31 on 2023-11-08 21:43_

As an example of the kind of thing we can now enforce: this no longer accepts an arbitrary remote distribution (like an sdist), it _has_ to be a wheel. Same for the `SourceDistributionFetcher`.

---

_Comment by @charliermarsh on 2023-11-08 21:44_

If no one wants to review this that's fine, it's a slog. If anything, it would be most helpful just to get feedback on the type hierarchy in the summary (which I posted earlier on Discord).

---

_Review comment by @zanieb on `crates/puffin-distribution/src/any.rs`:8 on 2023-11-08 22:18_

Is this correct? Should it be `AnyBuiltDistribution` if so?

---

_@zanieb reviewed on 2023-11-08 22:18_

---

_Review comment by @konstin on `crates/puffin-distribution/src/cached.rs`:147 on 2023-11-09 09:29_

```suggestion
        let Some((name, version)) = file_name.rsplit_once('-') else {
```
Our cache should have correct normalization but this is what we're using elsewhere already

---

_Review comment by @konstin on `crates/puffin-distribution/src/direct_url.rs`:69 on 2023-11-09 09:32_

We should check and raise an explicit unsupported error if there is a `+` in the scheme that isn't `git+`

---

_Review comment by @konstin on `crates/puffin-distribution/src/direct_url.rs`:82 on 2023-11-09 09:33_

It's a bit strange (but not blocking) that `DirectUrl` lives in `pypi_types` when it's distinctly not a pypi type. Maybe `pypackaging_types`?

---

_Review comment by @konstin on `crates/puffin-distribution/src/direct_url.rs`:101 on 2023-11-09 09:35_

I plan looking into these

---

_Review comment by @konstin on `crates/puffin-distribution/src/installed.rs`:34 on 2023-11-09 09:41_

I'd put `name`, `version`, `path` into a shared `InstalledDistribution` struct and call the enum `InstalledDistributionSource`. Depending on whether we have methods that only take a `InstalledRegistryDistribution` or a `InstalledDirectUrlDistribution`m, `InstalledDistribution` could get a `source: InstalledDistributionSource` field, otherwise this just abstracts away the three fields

---

_Review comment by @konstin on `crates/puffin-distribution/src/traits.rs`:11 on 2023-11-09 09:43_

This is a good indication we actually want to use `Dist` instead of `Distribution`

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver.rs`:829 on 2023-11-09 09:49_

Shouldn't this be symmetric, where one request kind always produces on response kind?

---

_@konstin approved on 2023-11-09 09:49_

---

_@charliermarsh reviewed on 2023-11-09 20:45_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/traits.rs`:11 on 2023-11-09 20:45_

I'll do this in a separate PR.

---

_Merged by @charliermarsh on 2023-11-10 02:45_

---

_Closed by @charliermarsh on 2023-11-10 02:45_

---

_Branch deleted on 2023-11-10 02:45_

---
