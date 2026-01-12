```yaml
number: 251
title: Add support for URL dependencies
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/url
created_at: 2023-10-31T04:10:37Z
updated_at: 2023-11-01T13:21:45Z
url: https://github.com/astral-sh/uv/pull/251
synced_at: 2026-01-12T16:03:49Z
```

# Add support for URL dependencies

---

_@charliermarsh_

## Summary

This PR adds support for resolving and installing dependencies via direct URLs, like:

```
werkzeug @ https://files.pythonhosted.org/packages/ff/1d/960bb4017c68674a1cb099534840f18d3def3ce44aed12b5ed8b78e0153e/Werkzeug-2.0.0-py3-none-any.whl
```

These are fairly common (e.g., with `torch`), but you most often see them as Git dependencies.

Broadly, structs like `RemoteDistribution` and friends are now enums that can represent either registry-based dependencies or URL-based dependencies:

```rust
/// A built distribution (wheel) that exists as a remote file (e.g., on `PyPI`).
#[derive(Debug, Clone)]
#[allow(clippy::large_enum_variant)]
pub enum RemoteDistribution {
    /// The distribution exists in a registry, like `PyPI`.
    Registry(PackageName, Version, File),
    /// The distribution exists at an arbitrary URL.
    Url(PackageName, Url),
}
```

In the resolver, we now allow packages to take on an extra, optional `Url` field:

```rust
#[derive(Debug, Clone, Eq, Derivative)]
#[derivative(PartialEq, Hash)]
pub enum PubGrubPackage {
    Root,
    Package(
        PackageName,
        Option<DistInfoName>,
        #[derivative(PartialEq = "ignore")]
        #[derivative(PartialOrd = "ignore")]
        #[derivative(Hash = "ignore")]
        Option<Url>,
    ),
}
```

However, for the purpose of version satisfaction, we ignore the URL. This allows for the URL dependency to satisfy the transitive request in cases like:

```
flask==3.0.0
werkzeug @ https://files.pythonhosted.org/packages/c3/fc/254c3e9b5feb89ff5b9076a23218dafbc99c96ac5941e900b71206e6313b/werkzeug-3.0.1-py3-none-any.whl
```

There are a couple limitations in the current approach:

- The caching for remote URLs is done separately in the resolver vs. the installer. I decided not to sweat this too much... We need to figure out caching holistically.
- We don't support any sort of time-based cache for remote URLs -- they just exist forever. This will be a problem for URL dependencies, where we need some way to evict and refresh them. But I've deferred it for now.
- I think I need to redo how this is modeled in the resolver, because right now, we don't detect a variety of invalid cases, e.g., providing two different URLs for a dependency, asking for a URL dependency and a _different version_ of the same dependency in the list of first-party dependencies, etc.
- (We don't yet support VCS dependencies.)


---

_Review requested from @konstin by @charliermarsh on 2023-11-01 03:12_

---

_Marked ready for review by @charliermarsh on 2023-11-01 03:12_

---

_Review requested from @zanieb by @charliermarsh on 2023-11-01 03:12_

---

_Label `enhancement` added by @charliermarsh on 2023-11-01 03:21_

---

_Review comment by @konstin on `crates/puffin-cli/tests/snapshots/pip_compile__conflicting_transitive_url_dependency.snap`:9 on 2023-11-01 11:41_

This should be filtered out

---

_Review comment by @konstin on `crates/puffin-installer/src/url_index.rs`:33 on 2023-11-01 11:49_

I think we need a `url -> cache info` cache bucket and then index all the wheel by hashes url to avoid the filename collisions we currently have. Most of that can be done later but it think putting wheel in sha names directories (e.g. like cacache) to prevent one overriding the other would be good before merging

---

_Review comment by @konstin on `crates/puffin-resolver/src/error.rs`:24 on 2023-11-01 11:53_

Shouldn't this be wrapped in a PEP 508 error?

---

_Review comment by @konstin on `crates/puffin-resolver/src/source_distribution.rs`:98 on 2023-11-01 11:57_

Those should use a different cache

---

_Review comment by @konstin on `crates/puffin-resolver/src/wheel_finder.rs`:79 on 2023-11-01 11:58_

What's the function of the non-default hasher here?

---

_@konstin approved on 2023-11-01 11:59_

---

_@charliermarsh reviewed on 2023-11-01 13:04_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/url_index.rs`:33 on 2023-11-01 13:04_

This actually _does_ use the SHA of the URL as the cache key. It's doing roughly what you've described.

---

_@charliermarsh reviewed on 2023-11-01 13:04_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/error.rs`:24 on 2023-11-01 13:04_

Yes!

---

_@charliermarsh reviewed on 2023-11-01 13:04_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/wheel_finder.rs`:79 on 2023-11-01 13:04_

It's the default hasher -- you just _have_ to do this if you want to set a capacity on an FxHasher.

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/source_distribution.rs`:98 on 2023-11-01 13:18_

Good call. This should actually be a totally separate struct IMO.

---

_@charliermarsh reviewed on 2023-11-01 13:18_

---

_Merged by @charliermarsh on 2023-11-01 13:21_

---

_Closed by @charliermarsh on 2023-11-01 13:21_

---

_Branch deleted on 2023-11-01 13:21_

---
