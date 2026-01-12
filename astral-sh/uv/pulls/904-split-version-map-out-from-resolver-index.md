```yaml
number: 904
title: Split version map out from resolver index
type: pull_request
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
base: main
head: charlie/index
created_at: 2024-01-12T21:15:25Z
updated_at: 2024-01-16T05:20:13Z
url: https://github.com/astral-sh/uv/pull/904
synced_at: 2026-01-12T16:04:16Z
```

# Split version map out from resolver index

---

_@charliermarsh_

## Summary

This is a change I'm considering but not sold on. Right now, the `Index` in the resolver looks like:

```rust
/// In-memory index of package metadata.
#[derive(Default)]
pub(crate) struct Index {
    /// A map from package name to the metadata for that package and the index where the metadata
    /// came from.
    pub(crate) packages: OnceMap<PackageName, VersionMap>,

    /// A map from package ID to metadata for that distribution.
    pub(crate) distributions: OnceMap<PackageId, Metadata21>,

    /// A map from source URL to precise URL.
    pub(crate) redirects: OnceMap<Url, Url>,
}
```

I want to change this to something that can be universally shared across resolutions... But `packages` (for example) is _not_ shareable, because `VersionMap` is a function of "API response + tags + Python requirement", where the latter two are parameters to the resolution.

Specifically, if the user passed in `--python-version`, and we tried to reuse the stored `VersionMap` here when resolving dependencies for a source distribution build, we could end up doing the wrong thing, because the `VersionMap` would be _wrong_.

So, instead, I'm proposing that we change `Index` to resolution-agnostic data, and move any resolution-specific data into other fields on the resolver. That would allow us to share `Index` across resolutions safely.

The specific change here changes from `pub(crate) packages: OnceMap<PackageName, VersionMap>` to `pub(crate) packages: OnceMap<PackageName, (IndexUrl, BaseUrl, SimpleMetadata)>` -- that's static response data that doesn't rely on the resolution inputs. We then have to compute the `VersionMap` from those parameters and store it separately.

The main downside here is that we now need to clone `SimpleMetadata` (since we're storing it _and_ the `VersionMap`), and we need to use a separate hash map for the versions.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-01-12 21:17_

---

_Review requested from @konstin by @charliermarsh on 2024-01-12 21:17_

---

_@charliermarsh reviewed on 2024-01-12 21:19_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:836 on 2024-01-12 21:19_

@BurntSushi - I'd love to get your opinion on this. In short, we used to store `VersionMap` in `self.index.packages`. Now, `self.index.packages` stores the `SimpleMetadata`, which is an input to `VersionMap`, and so we need to compute the version map from that data. Here, I'm basically just doing that computation lazily and storing it in a `self.versions` hash map.

(The benefit here is that `self.index` becomes shareable across resolutions -- sometimes we need to perform a resolution _within_ a resolution, if we're trying to build a source distribution into a built wheel. Right now, it's not totally safe to share it, because `self.index` contains structs like `VersionMap` that can vary based on some of the parameters, and we don't always pass the same parameters to that sub-resolution.)


---

_Label `internal` added by @charliermarsh on 2024-01-13 02:59_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver/mod.rs`:529 on 2024-01-14 09:37_

```suggestion
                let version_entry =
                    self.versions
                        .entry(package_name.clone())
                        .or_insert_with(|| {
                            VersionMap::from_metadata(
                                metadata.clone(),
                                package_name,
                                index,
                                base,
                                self.tags,
                                &self.python_requirement,
                                &self.allowed_yanks,
                                self.exclude_newer.as_ref(),
                            )
                        });
```

---

_Comment by @konstin on 2024-01-14 09:38_

> Specifically, if the user passed in --python-version, and we tried to reuse the stored VersionMap here when resolving dependencies for a source distribution build, we could end up doing the wrong thing, because the VersionMap would be wrong.

Does this mean we assume the we python environment can change in puffin run?

---

_@konstin approved on 2024-01-14 09:40_

---

_Comment by @charliermarsh on 2024-01-14 15:21_

> Does this mean we assume the we python environment can change in puffin run?

The _specific_ problem here is for the top-level resolution vs. resolutions for source distributions, when using `--python-version`. The version maps can differ in that case, since the top-level resolution will ensure that its choices are compatible with `--python-version`, while the source distribution resolution only cares about the installed version.

An alternative would be to make it a hash map from PackageID to (some hash of the resolver configuration) to version map...

---

_Comment by @charliermarsh on 2024-01-14 15:32_

@konstin - Another option would be: if we're using `--python-version`, explicitly create a separate index for `SourceBuildContext`. Maybe that's simpler? But it's still sort of a shame.

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/resolver/index.rs`:14 on 2024-01-15 17:03_

What do you think about lifting this tuple into a named type?

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/resolver/mod.rs`:76 on 2024-01-15 17:05_

Wow the resolver has _a lot_ of state. I realize this PR isn't adding any new state, but seeing it laid out this way makes it hit home. It makes me wonder if there are some lurking refactorings here that might simplify things.

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/resolver/mod.rs`:233 on 2024-01-15 17:07_

Ooof, no rustfmt in macros like this is just brutal.

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/resolver/mod.rs`:905 on 2024-01-15 17:15_

It looks like you might be able to use a named type here if you define one instead of using the tuple.

---

_@BurntSushi approved on 2024-01-15 17:16_

At a conceptual level this makes sense to me I think. I'm unsure of the perf implications here, but it does feel to me like this is a step in the right direction (particularly from a correctness perspective).

---

_Comment by @charliermarsh on 2024-01-16 03:33_

I rebased this, but unfortunately it's ~8-10% slower on warm resolutions (at least on `trio.in`).

---

_Comment by @charliermarsh on 2024-01-16 03:41_

I may close this and instead change https://github.com/astral-sh/puffin/pull/906 to use separate indexes for top-level resolution vs. source distribution resolution when `--python-version` is specified.

---

_Closed by @charliermarsh on 2024-01-16 05:20_

---
