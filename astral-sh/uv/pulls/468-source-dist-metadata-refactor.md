```yaml
number: 468
title: Source dist metadata refactor
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: source-dist-metadata-refactor
created_at: 2023-11-20T11:41:25Z
updated_at: 2023-11-24T17:47:59Z
url: https://github.com/astral-sh/uv/pull/468
synced_at: 2026-01-12T16:03:58Z
```

# Source dist metadata refactor

---

_@konstin_

## Summary and motivation

For a given source dist, we store the metadata of each wheel built through it in `built-wheel-metadata-v0/pypi/<source dist filename>/metadata.json`. During resolution, we check the cache status of the source dist. If it is fresh, we check `metadata.json` for a matching wheel. If there is one we use that metadata, if there isn't, we build one. If the source is stale, we build a wheel and override `metadata.json` with that single wheel. This PR thereby ties the local built wheel metadata cache to the freshness of the remote source dist. This functionality is available through `SourceDistCachedBuilder`.

`puffin_installer::Builder`, `puffin_installer::Downloader` and `Fetcher` are removed, instead there are now `FetchAndBuild` which calls into the also new `SourceDistCachedBuilder`. `FetchAndBuild` is the new main high-level abstraction: It spawns parallel fetching/building, for wheel metadata it calls into the registry client, for wheel files it fetches them, for source dists it calls `SourceDistCachedBuilder`. It handles locks around builds, and newly added also inter-process file locking for git operations. 

Fetching and building source distributions now happens in parallel in `pip-sync`, i.e. we don't have to wait for the largest wheel to be downloaded to start building source distributions.

In a follow-up PR, I'll also clear built wheels when they've become stale.

Another effect is that in a fully cached resolution, we need neither zip reading nor email parsing.

Closes #473

## Source dist cache structure 

Entries by supported sources:
 * `<build wheel metadata cache>/pypi/foo-1.0.0.zip/metadata.json`
 * `<build wheel metadata cache>/<sha256(index-url)>/foo-1.0.0.zip/metadata.json`
 * `<build wheel metadata cache>/url/<sha256(url)>/foo-1.0.0.zip/metadata.json`
But the url filename does not need to be a valid source dist filename
(<https://github.com/search?q=path%3A**%2Frequirements.txt+master.zip&type=code>),
so it could also be the following and we have to take any string as filename:
 * `<build wheel metadata cache>/url/<sha256(url)>/master.zip/metadata.json`

Example:
```text
# git source dist
pydantic-extra-types @ git+https://github.com/pydantic/pydantic-extra-types.git
# pypi source dist
django_allauth==0.51.0
# url source dist
werkzeug @ https://files.pythonhosted.org/packages/0d/cc/ff1904eb5eb4b455e442834dabf9427331ac0fa02853bf83db817a7dd53d/werkzeug-3.0.1.tar.gz
```
will be stored as
```text
built-wheel-metadata-v0
├── git
│   └── 5c56bc1c58c34c11
│       └── 843b753e9e8cb74e83cac55598719b39a4d5ef1f
│           └── metadata.json
├── pypi
│   └── django-allauth-0.51.0.tar.gz
│       └── metadata.json
└── url
    └── 6781bd6440ae72c2
        └── werkzeug-3.0.1.tar.gz
            └── metadata.json
```

The inside of a `metadata.json`:
```json
{
  "data": {
    "django_allauth-0.51.0-py3-none-any.whl": {
      "metadata-version": "2.1",
      "name": "django-allauth",
      "version": "0.51.0",
      ...
    }
  }
}
```

---

_Comment by @konstin on 2023-11-20 14:16_

Current dependencies on/for this PR:
* main
  * **PR #462** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/462?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #468** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/468?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/468?utm_source=stack-comment).

---

_Marked ready for review by @konstin on 2023-11-21 15:50_

---

_Review comment by @charliermarsh on `crates/puffin-git/src/source.rs`:131 on 2023-11-21 19:30_

Why use these rather than the traits?

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/source_dist.rs`:157 on 2023-11-21 19:34_

Why can't we use `.filename()` here? Isn't this already available as a trait?

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/source_dist.rs`:117 on 2023-11-21 19:37_

Can you talk me through this struct a bit? How does it relate to the `Fetcher` struct? It looks like it duplicates a lot of the responsibilities and functionality. By introducing this, aren't we again forking behavior, since this doesn't seem to be used in the installer?

Put differently: why are the behaviors here not part of `Fetcher` directly?

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/source_dist.rs`:117 on 2023-11-21 19:39_

Why is this not being used? This feels slightly off to me.

---

_Review comment by @charliermarsh on `Cargo.toml`:38 on 2023-11-21 19:41_

(Minor nit: feel like these dep changes could've been their own PR so that readers don't think these are new.)

---

_@charliermarsh reviewed on 2023-11-21 19:41_

---

_@konstin reviewed on 2023-11-22 08:29_

---

_Review comment by @konstin on `Cargo.toml`:38 on 2023-11-22 08:29_

https://github.com/astral-sh/puffin/pull/483

---

_@konstin reviewed on 2023-11-22 16:36_

---

_Review comment by @konstin on `crates/puffin-git/src/source.rs`:131 on 2023-11-22 16:36_

For me, `From`/`Into` are for loss-less conversions and not field access. Do we just want to make those fields public?

---

_@konstin reviewed on 2023-11-22 16:43_

---

_Review comment by @konstin on `crates/puffin-distribution/src/source_dist.rs`:139 on 2023-11-22 16:43_

This breaks the pattern from the other reporters cause `impl Reporter` didn't work here, but i'm happy to switch to a better suggestion

---

_@charliermarsh reviewed on 2023-11-22 17:06_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/fetch_and_build.rs`:223 on 2023-11-22 17:06_

Doesn't `wheel` already have a `filename` attribute on it?

---

_@charliermarsh reviewed on 2023-11-22 17:06_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/fetch_and_build.rs`:222 on 2023-11-22 17:06_

Can this not use `wheel.url.filename()?`? We have a trait for it.

---

_@charliermarsh reviewed on 2023-11-22 17:08_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/fetch_and_build.rs`:77 on 2023-11-22 17:08_

What about... `DistributionDatabase`? We frame it as a database for accessing distribution metadata, contents, and builds.

---

_@charliermarsh reviewed on 2023-11-22 17:09_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/metadata.rs`:129 on 2023-11-22 17:09_

I think we can rename this to `built_wheel_dir`.

---

_@charliermarsh reviewed on 2023-11-22 17:12_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/fetch_and_build.rs`:362 on 2023-11-22 17:12_

Is this strictly necessary?

---

_@charliermarsh reviewed on 2023-11-22 17:15_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/metadata.rs`:32 on 2023-11-22 17:15_

I'm a little confused by what URL is passed-in here vs. what values are rebound the the `url` variable name in the match below.

---

_@charliermarsh reviewed on 2023-11-22 17:16_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:229 on 2023-11-22 17:16_

Maybe we just stick to this for now and remove the first branch. We already have extra logging so that users know we built any relevant packages.

---

_@charliermarsh reviewed on 2023-11-22 17:16_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_sync.rs`:189 on 2023-11-22 17:16_

I am assuming that the user-facing output did not change here, so we still show `Fetching X` and `Build X` and such, plus a progress bar. Is that correct?

---

_@charliermarsh reviewed on 2023-11-22 17:18_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/metadata.rs`:43 on 2023-11-22 17:18_

Since every branch here is just appending to `cache_with_bucket`, I think we should remove that argument. Just return `git.join(digest(&CanonicalUrl::new(url)))`, and the caller can do the prepend, right?

---

_@charliermarsh reviewed on 2023-11-22 17:18_

---

_Review comment by @charliermarsh on `crates/puffin-cache/src/metadata.rs`:131 on 2023-11-22 17:18_

I would remove the `&cache_with_bucket` argument. It makes the method less flexible. Instead, we can do `cache.join(BUILT_WHEEL_METADATA_CACHE).join(self.append_to_bucket(url)).join(filename)` or whatever.

---

_@charliermarsh reviewed on 2023-11-22 17:20_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/fetch_and_build.rs`:38 on 2023-11-22 17:20_

Does this caching need to be unified with the built wheel caching...?

---

_@charliermarsh reviewed on 2023-11-22 17:20_

Left some questions, but I don't think this is too far away.

---

_@konstin reviewed on 2023-11-22 18:46_

---

_Review comment by @konstin on `crates/puffin-cli/src/commands/pip_sync.rs`:189 on 2023-11-22 18:46_

pip-compile:

[Screencast from 2023-11-22 19-42-02.webm](https://github.com/astral-sh/puffin/assets/6826232/693d00e3-94bd-4d51-a71d-96df9608a6c2)

pip-sync:

[Screencast from 2023-11-22 19-45-48.webm](https://github.com/astral-sh/puffin/assets/6826232/55af0455-da41-4dfb-8120-deadeb563e19)

There's no explicit `Fetching X`, but the output looks right in general


---

_@konstin reviewed on 2023-11-22 19:23_

---

_Review comment by @konstin on `crates/puffin-distribution/src/fetch_and_build.rs`:38 on 2023-11-22 19:23_

Yes, and we also need to fix the storage paths. i added the same todo comment as for the other archives.

---

_@konstin reviewed on 2023-11-22 19:25_

---

_Review comment by @konstin on `crates/puffin-distribution/src/fetch_and_build.rs`:362 on 2023-11-22 19:25_

Removed it here (with hopes that libgit2's isolation is as good as that stackoverflow answer indicates), kept it in the place where we really need 

---

_Comment by @konstin on 2023-11-22 19:25_

I think i addressed all comments

---

_@charliermarsh reviewed on 2023-11-24 15:43_

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/source_dist.rs`:486 on 2023-11-24 15:43_

My question was more: do we need this at all? If so, why? I think the underlying Git abstractions will actually error if you try and manipulate them twice at once.

---

_@charliermarsh approved on 2023-11-24 15:59_

Nice, let's roll with it!

---

_Review comment by @charliermarsh on `crates/puffin-distribution/src/source_dist.rs`:486 on 2023-11-24 16:23_

Like, I'd kind of rather remove this unless we learn that we need it.

---

_@charliermarsh reviewed on 2023-11-24 16:23_

---

_Comment by @charliermarsh on 2023-11-24 17:44_

(Going to merge this to avoid conflicts.)

---

_Merged by @charliermarsh on 2023-11-24 17:47_

---

_Closed by @charliermarsh on 2023-11-24 17:47_

---

_Branch deleted on 2023-11-24 17:47_

---
