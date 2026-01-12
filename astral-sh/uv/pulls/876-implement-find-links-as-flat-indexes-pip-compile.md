```yaml
number: 876
title: "Implement `--find-links` as flat indexes (pip-compile)"
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
base: main
head: konsti/flat-indexes
created_at: 2024-01-10T21:48:59Z
updated_at: 2024-01-15T09:02:06Z
url: https://github.com/astral-sh/uv/pull/876
synced_at: 2026-01-12T16:04:15Z
```

# Implement `--find-links` as flat indexes (pip-compile)

---

_@konstin_

Add directory `--find-links` support for local paths to pip-compile.

It seems that pip joins all sources and then picks the best package. We explicitly give find links packages precedence if the same exists on an index and locally by prefilling the `VersionMap`, otherwise they are added as another index and the existing rules of precedence apply.

Internally, the feature is called _flat index_, which is more meaningful than _find links_: We're not looking for links, we're picking up local directories, and (TBD) support another index format that's just a flat list of files instead of a nested index. I've kept the flat index terminology in the user facing parts for pip compatibility.

Previously, a `RegistryBuiltDist`/`RegistrySourceDist` would be formatted as `<name>==<version>` while path and url dists would be formatted as `<name> @ <url>`. Now there are flat index path and url dists that behave as additions to the index and should be formatted by version instead of by url. Instead of adding more `Dist` types, i chose to track their designation as `is_registry`. An alternative would be adding an `is_registry` field to path and url dists.

`PrioritizedDistribution` and `FlatIndex` have been moved to locations where we can use them in the upstack PR.

I added a `scripts/wheels` directory with stripped down wheels to use for testing.

We're lacking tests for correct tag priority precedence with flat indexes, i only confirmed this manually since it is not covered in the pip-compile or pip-sync output.

Part 1/3 (directory find links in pip-compile, find links in pip-sync, remote find links). 

---

_Marked ready for review by @konstin on 2024-01-11 10:45_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:74 on 2024-01-11 14:13_

Nit: we don't export any other modules in the crate this way. Mind changing to match the convention?

---

_Review comment by @charliermarsh on `crates/distribution-types/src/index_url.rs`:56 on 2024-01-11 14:14_

Nit: `URL`, `HTML`

---

_Review comment by @charliermarsh on `crates/distribution-types/src/index_url.rs`:70 on 2024-01-11 14:15_

This seems off. Should we not try to parse the file, and fall back to path? Or parse the file, check the scheme, then fall back to path?

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/main.rs`:162 on 2024-01-11 14:16_

Nit: two spaces after "distributions".

---

_Review comment by @charliermarsh on `crates/puffin-client/src/flat_index.rs`:19 on 2024-01-11 18:02_

I'd err on the side of removing this -- IMO it's better as a comment on the PR than as a comment in-code.

---

_Review comment by @charliermarsh on `crates/puffin-client/src/flat_index.rs`:45 on 2024-01-11 18:02_

This is stale, no? Per requires-python above.

---

_Review comment by @charliermarsh on `crates/puffin-client/src/flat_index.rs`:49 on 2024-01-11 18:03_

This seems like an error that we need to propagate, right?

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:117 on 2024-01-11 18:04_

Nit: I'd remove `// It doesn't feel like it matter for this one time call`, again it feels like a comment for reviewers that you could leave in the PR, not something that should live in code.

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:128 on 2024-01-11 18:04_

Should this be an actual `todo!`?

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:132 on 2024-01-11 18:04_

You could probably write this whole thing as a single iterator.

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:137 on 2024-01-11 18:06_

Nit: absolut

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:134 on 2024-01-11 18:07_

Would it be simpler for this to just return `Vec` and do the concat outside? That would also avoid `dir_dists = 0`.

---

_Review comment by @charliermarsh on `crates/puffin-client/src/flat_index.rs`:29 on 2024-01-11 18:10_

Maybe `from_dists`? I think it's confusing to have `FlatIndex::from_flat_index`.

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolution.rs`:38 on 2024-01-11 18:12_

I don't think I understand this part. Can you walk me through what happens if we omit this entirely?

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:705 on 2024-01-11 18:13_

I don't fully understand this part and it feels like something we need a better abstraction around. We call `package_id()` in a lot of places -- is this even safe?

Can you walk me through the problem here in more detail? If this is a file from a `--find-links`, what would go wrong?


---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:118 on 2024-01-11 18:14_

I'm a bit mixed on whether this belongs in `RegistryClient`.

---

_@charliermarsh reviewed on 2024-01-11 18:14_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:705 on 2024-01-11 18:15_

> Now there are flat index path and url dists that behave as additions to the index and should be formatted by version instead of by url. Instead of adding more Dist types, i chose to track their designation as is_registry. An alternative would be adding an is_registry field to path and url dists.

Clarifying: should `--find-links` versions _not_ be represented as URLs? Is it wrong to do so?

---

_@charliermarsh reviewed on 2024-01-11 18:15_

---

_@konstin reviewed on 2024-01-12 10:23_

---

_Review comment by @konstin on `crates/distribution-types/src/index_url.rs`:70 on 2024-01-12 10:23_

The cases we need to support are
```
--find-links https://packages.example.com/path/to/flat-index.html
--find-links path/to/local/flat-index
--find-links file:///absolute/path/to/local/flat-index
```
I don't really expect `file://` indexes to be use, but it would be strange to not support them here. First checking the `://` is to avoid that an invalid url accidentally falls back to a path and then gives confusing error messages.

---

_@konstin reviewed on 2024-01-12 10:28_

---

_Review comment by @konstin on `crates/puffin-client/src/flat_index.rs`:49 on 2024-01-12 10:28_

We know that `read_flat_index_dir` only returns absolute paths (we call `fs_err::canonicalize(path)` in the beginning), so we know that this call succeeds.

---

_@konstin reviewed on 2024-01-12 10:29_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:128 on 2024-01-12 10:29_

I had it like that in the beginning, but if we merge this PR before the html find links support PR i didn't want to have something that panics on main (and then explodes tokio's executor).

---

_@konstin reviewed on 2024-01-12 10:40_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:132 on 2024-01-12 10:40_

The problem is that we need to flat map (`read_flat_index_dir` returns a list of entries and should warn when it yielded 0 items) while also raising any error; I unfortunately wouldn't know any iterator to express this in a readable way.

---

_@konstin reviewed on 2024-01-12 10:42_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:137 on 2024-01-12 10:42_

![image](https://github.com/astral-sh/puffin/assets/6826232/e02f94d9-17a7-4205-a093-f23f4921303a)

It took me unreasonably long to see the problem

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:134 on 2024-01-12 10:43_

If we don't mind the allocations, sure

---

_@konstin reviewed on 2024-01-12 10:43_

---

_@konstin reviewed on 2024-01-12 10:43_

---

_Review comment by @konstin on `crates/puffin-client/src/flat_index.rs`:29 on 2024-01-12 10:43_

Definitely, the name should have changed with the refactoring

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver/mod.rs`:705 on 2024-01-12 10:58_

From looking at what pip and pip-tools do, i consider `--find-links` as an `--extra-index-url` with a different format, a flat index instead of a nested index. I consider `--index-url`, `--extra-index-url` and `--find-inks` together making up the search path, as opposed to saying e.g. i want the source dist at a specific url or at this location in the project.

If we run `jax[cuda12_pip]` through `pip-compile -f https://storage.googleapis.com/jax-releases/jax_cuda_releases.html jax.in`, we get:

```
[...prelude...]
--find-links https://storage.googleapis.com/jax-releases/jax_cuda_releases.html

jax[cuda12-pip,cuda12_pip]==0.4.23
    # via -r target/jax.in
jaxlib==0.4.23+cuda12.cudnn89
    # via jax
[...more deps...]
```

The alternative would would be something like below, where we pin a specific wheel: This would pin the resolution to a specific platform, while the `pip-compile` output is platform independent, and in this case even works across python versions.

```
[...prelude...]
jax[cuda12-pip,cuda12_pip]==0.4.23
    # via -r target/jax.in
jaxlib @ https://storage.googleapis.com/jax-releases/cuda12/jaxlib-0.4.23+cuda12.cudnn89-cp312-cp312-manylinux2014_x86_64.whl
    # via jax
[...more deps...]
```

The architectural problem is that the find links entries don't have a `File` (we can't really match paths on a `File`, no hashes and no url, and handling path find links different to url find links would be also inconsistent), so we can't build a `RegistryBuiltDist`/`RegistrySourceDist` out of them. I chose the less invasive route by just tracking the registry-ness outside of the `Dist` hierarchy. The alternatives would be migrating `Registry*Dist` away from `File` to a new type that also support find links, or adding a field to `DirectUrl*Dist` and `Path*Dist` that tracks their registry-ness.

---

_@konstin reviewed on 2024-01-12 10:58_

---

_@konstin reviewed on 2024-01-12 10:58_

---

_Review comment by @konstin on `crates/puffin-client/src/registry_client.rs`:118 on 2024-01-12 10:58_

Same, where'd you move it?

---

_Review comment by @charliermarsh on `crates/puffin-client/src/registry_client.rs`:137 on 2024-01-12 14:42_

Hahaha I'm sorry!

---

_@charliermarsh reviewed on 2024-01-12 14:42_

---

_@charliermarsh reviewed on 2024-01-12 14:44_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:705 on 2024-01-12 14:44_

Okay, I need to spend a bit more time reading this code today.

---

_@charliermarsh reviewed on 2024-01-12 19:00_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:718 on 2024-01-12 19:00_

Why is this not respected here?

---

_@charliermarsh reviewed on 2024-01-12 19:06_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:711 on 2024-01-12 19:06_

So is the _specific_ problem here that `Candidate` assumes it only gets registry dists? Because it returns `distribution_types::VersionOrUrl::Version(self.version.into())`? (Can we change _that_ somehow to avoid this? It doesn't seem _required_ that we use a `{name}-{version}` representation for `--find-links` distributions here, only that it's _assumed_ in other parts of the code.)

---

_@charliermarsh reviewed on 2024-01-12 19:08_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolution.rs`:205 on 2024-01-12 19:08_

Can we just remove this instead, and `dist_to_requirement`? It's only used in the `resolve-cli` in dev. 

---

_@charliermarsh reviewed on 2024-01-12 19:08_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolution.rs`:38 on 2024-01-12 19:08_

I can maybe live with this part though I find it clear evidence that we're missing an abstraction :(

---

_@charliermarsh reviewed on 2024-01-12 19:09_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolution.rs`:271 on 2024-01-12 19:09_

Is this actually true for source distributions?

---

_@charliermarsh reviewed on 2024-01-12 19:11_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/flat_index.rs`:32 on 2024-01-12 19:11_

I think I misread this originally, but the fact that this doesn't return `Self` feels really confusing from an API design perspective. I think you should consider adding a separate struct that wraps `FxHashMap<PackageName, FlatIndex>`, and putting this method on that separate struct.

---

_@charliermarsh reviewed on 2024-01-12 19:11_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:711 on 2024-01-12 19:11_

I think we need to fix this before merging and find some other solution. I can barely understand it _now_, so there's just no way I'll be able to follow in the future. (This isn't your fault, it's confusing, I'm just articulating that we should try to address this.)

---

_@charliermarsh reviewed on 2024-01-12 19:12_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:718 on 2024-01-12 19:12_

Can't we have a source distribution from a `--find-links` here?

---

_Comment by @charliermarsh on 2024-01-13 01:54_

My worst nightmare, I worked through all the merge conflicts here but now tests are just hanging...

---

_Comment by @charliermarsh on 2024-01-13 01:59_

@konstin - Did you a solid and fixed some rough merge conflicts here.

---

_@charliermarsh reviewed on 2024-01-13 02:07_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:711 on 2024-01-13 02:07_

Okay, seeing the problem better now. In `get_dependencies`, we use `PubGrubDistribution::from_registry`, so we _always_ get the registry ID.

---

_@charliermarsh reviewed on 2024-01-13 02:18_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:711 on 2024-01-13 02:18_

So it's like: there's a coupling (right now) between the way the dependency is declared (URL vs. version specifier) and the distribution type to which it resolves (URL is assumed to resolve to a URL, version specifier is assumed to resolve to a registry distribution). And now, we're returning a URL-based distribution for a version specifier...

Can this be solved by improving the distribution type hierarchy?


---

_@charliermarsh reviewed on 2024-01-13 14:32_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:711 on 2024-01-13 14:32_

I think we may want to consider adding a layer of indirection in the resolver between (requirement / `PubGrubPackage`) and distribution, but I need to try it.

---

_Closed by @charliermarsh on 2024-01-15 02:04_

---

_Branch deleted on 2024-01-15 09:02_

---
