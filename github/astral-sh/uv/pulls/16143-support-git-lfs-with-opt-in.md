---
number: 16143
title: Support Git LFS with opt-in
type: pull_request
state: closed
author: samypr100
labels:
  - enhancement
  - "test:macos"
assignees: []
base: main
head: conditional_lfs_support
created_at: 2025-10-06T21:13:10Z
updated_at: 2025-12-06T12:08:39Z
url: https://github.com/astral-sh/uv/pull/16143
synced_at: 2026-01-07T13:12:19-06:00
---

# Support Git LFS with opt-in

---

_Pull request opened by @samypr100 on 2025-10-06 21:13_

## Summary

Follow up to https://github.com/astral-sh/uv/pull/15563
Closes https://github.com/astral-sh/uv/issues/13485

This is a first-pass at adding support for conditional support for Git LFS between git sources, initial feedback welcome.

e.g.
```
[tool.uv.sources]
test-lfs-repo = { git = "https://github.com/zanieb/test-lfs-repo.git", lfs = true }
```

For context previously a user had to set `UV_GIT_LFS` to have uv fetch lfs objects on git sources. This env var was all or nothing, meaning you must always have it set to get consistent behavior and it applied to all git sources. If you fetched lfs objects at a revision and then turned off lfs (or vice versa), the git db, corresponding checkout lfs artifacts would not be updated properly. Similarly, when git source distributions were built, there would be no distinction between sources with lfs and without lfs. Hence, it could corrupt the git, sdist, and archive caches.

In order to support some sources being LFS enabled and other not, this PR adds a stateful layer roughly similar to how `subdirectory` works but for `lfs` since the git database, the checkouts and the corresponding caching layers needed to be LFS aware (requested vs installed). The caches also had to isolated and treated entirely separate when handling LFS sources.

Summary
* Adds `lfs = true` or `lfs = false` to git sources in pyproject.toml
* Added `lfs=true` query param / fragments to most relevant url structs (not parsed as user input)
  * In the case of uv add / uv tool, `--lfs` is supported instead
  * `UV_GIT_LFS` environment variable support is still functional for non-project entrypoints (e.g. uv pip)
* `direct-url.json` now has an custom `git_lfs` entry under VcsInfo (note, this is not in the spec currently -- see caveats).
* git database and checkouts have an different cache key as the sources should be treated effectively different for the same rev.
* sdists cache also differ in the cache key of a built distribution if it was built using LFS enabled revisions to distinguish between non-LFS same revisions. This ensures the strong assumption for archive-v0 that an unpacked revision "doesn't change sources" stays valid.

Caveats
* `pylock.toml` import support has not been added via git_lfs=true, going through the spec it wasn't clear to me it's something we'd support outside of the env var (for now).
* direct-url struct was modified by adding a non-standard `git_lfs` field under VcsInfo which may be undersirable although the PEP 610 does say `Additional fields that would be necessary to support such VCS SHOULD be prefixed with the VCS command name` which could be interpret this change as ok.
* There will be a slight lockfile and cache churn for users that use `UV_GIT_LFS` as all git lockfile entries will get a `lfs=true` fragment. The cache version does not need an update, but LFS sources will get their own namespace under git-v0 and sdist-v9/git hence a cache-miss will occur once but this can be sufficient to label this as breaking for workflows always setting `UV_GIT_LFS`.

## Test Plan

Some initial tests were added. More tests likely to follow as we reach consensus on a final approach.

For IT test, we may want to move to use a repo under astral namespace in order to test lfs functionality.

Manual testing was done for common pathological cases like killing LFS fetch mid-way, uninstalling LFS after installing an sdist with it and reinstalling, fetching LFS artifacts in different commits, etc.

PSA: Please ignore the docker build failures as its related to depot OIDC issues.


---

_Comment by @codspeed-hq[bot] on 2025-10-17 22:11_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/samypr100%3Aconditional_lfs_support?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #16143 will **not alter performance**

<sub>Comparing <code>samypr100:conditional_lfs_support</code> (dc1b6ba) with <code>main</code> (5947fb0)</sub>



### Summary

`✅ 6` untouched  





---

_Marked ready for review by @samypr100 on 2025-10-21 15:59_

---

_Label `enhancement` added by @konstin on 2025-10-30 14:59_

---

_Review comment by @konstin on `docs/guides/tools.md`:148 on 2025-10-30 17:45_

Do we have a real repo for showcasing this? Not blocking but we mostly have real examples

---

_Review comment by @konstin on `docs/concepts/projects/dependencies.md`:372 on 2025-10-30 17:50_

Does that show a clear error on failure? If so, this can be `!!! important`, then it doesn't need to be a warning. 

---

_Review comment by @konstin on `crates/uv/src/commands/project/mod.rs`:1804 on 2025-10-30 17:59_

This looks like it could be `with_lfs` on `UnresolvedRequirement`.

---

_Review comment by @konstin on `crates/uv/tests/it/common/mod.rs`:883 on 2025-10-30 18:08_

What's the problem with reading the system gitconfig first?

---

_Review comment by @konstin on `crates/uv/tests/it/common/mod.rs`:918 on 2025-10-30 18:10_

We should document in the contributor docs that git lfs is required to run the tests

---

_Review comment by @konstin on `crates/uv/tests/it/sync.rs`:13684 on 2025-10-30 18:11_

Note to self: Move to astral-sh before merging

---

_Review comment by @konstin on `crates/uv/tests/it/sync.rs`:13579 on 2025-10-30 18:14_

We should run those with cache, both for test performance and to check when we reuse the cache.

---

_Comment by @samypr100 on 2025-10-30 19:46_

@zanieb This should be good to go. Given the size, how would you like to approach reviewing this best? I can also do a walkthrough of any of the changes and their rationale sync or async if desired. I tried to keep the commits idependently reviewable, although GH doesn't make it easy to review that way.

---

_Referenced in [astral-sh/uv#16539](../../astral-sh/uv/issues/16539.md) on 2025-11-01 03:17_

---

_Review comment by @konstin on `crates/uv-cache-key/src/canonical_url.rs`:209 on 2025-11-03 14:54_

This should be a named struct so `Option<bool>` has a name

---

_Review comment by @konstin on `crates/uv-cache-key/src/canonical_url.rs`:468 on 2025-11-03 15:12_

Can we remove `PartialEq` and those tests? These are currently unused and when we use them, we should be explicit about whether we consider `Some(false)` equal to `None` or not.

---

_Review comment by @konstin on `crates/uv-git-types/src/lib.rs`:15 on 2025-11-03 18:17_

Do we need this to be global state, is this slow?

---

_Review comment by @konstin on `crates/uv-git-types/src/lib.rs`:204 on 2025-11-03 18:25_

This feels slightly dangerous because it changes all instances of this type depending on the environment in a `TryFrom`

---

_Review comment by @konstin on `crates/uv/tests/it/common/mod.rs`:917 on 2025-11-03 18:50_

Are those filters from some documentation?

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:3972 on 2025-11-03 19:24_

Same here as for the other site where we're reading the env var, I'm a bit worried about building our requirements state from an env var, especially one that we read inline.

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:3967 on 2025-11-03 19:28_

If I understand the code correctly, do we track lfs as a three-state true/false/None on the `pyproject.toml` side, but then we're only tracking two state true/not-true here, so we have an asymmetry between `pyproject.toml` and `uv.lock`.

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:1440 on 2025-11-03 19:50_

We could use the tool table, but given that the spec doesn't say anything, for now it's fine to not handle LFS beyond respecting the env var.

---

_Review comment by @konstin on `crates/uv-distribution-types/src/requirement.rs`:448 on 2025-11-03 20:00_

We don't need the string "lfs" here. In the other branches, we're serializing different string values, but adding a constant doesn't change the cache key. 

---

_Review comment by @konstin on `crates/uv-cache-key/src/canonical_url.rs`:227 on 2025-11-03 20:08_

I'd go with 0 and 1 in the respective here for clarity. Our specific implementation puts a 0xFF between strings, but making this explicit makes us independent of the implementation state and makes it clear to all reads that `https://example.com/` with `lfs=true` and `https://example.com/lfs` have different cache keys.

One of the drawbacks of the PR (and this change specifically) is that we're invalidating the caches. I think that's fine, we've done that several for different buckets and features, but I want to document it for this PR.

---

_Review comment by @konstin on `crates/uv-installer/src/satisfies.rs`:192 on 2025-11-03 20:35_

This condition is failing for me locally with:

```
dependencies = [
    "test-lfs-repo",
]

[tool.uv.sources]
test-lfs-repo = { git = "https://github.com/samypr100/test-lfs-repo", rev = "657500f0703dc173ac5d68dfa1d7e8c985c84424", lfs = false }
```

```
DEBUG Resolving despite existing lockfile due to mismatched requirements for: `debug==0.1.0`
  Requested: {Requirement { name: PackageName("test-lfs-repo"), extras: [], groups: [], marker: true, source: Git { git: GitUrl { repository: DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/samypr100/test-lfs-repo", query: None, fragment: None }, reference: BranchOrTagOrCommit("657500f0703dc173ac5d68dfa1d7e8c985c84424"), precise: None, lfs: Disabled }, subdirectory: None, url: VerbatimUrl { url: DisplaySafeUrl { scheme: "git+https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/samypr100/test-lfs-repo@657500f0703dc173ac5d68dfa1d7e8c985c84424", query: None, fragment: None }, given: None } }, origin: None }}
  Existing: {Requirement { name: PackageName("test-lfs-repo"), extras: [], groups: [], marker: true, source: Git { git: GitUrl { repository: DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/samypr100/test-lfs-repo", query: None, fragment: None }, reference: BranchOrTagOrCommit("657500f0703dc173ac5d68dfa1d7e8c985c84424"), precise: None, lfs: Enabled }, subdirectory: None, url: VerbatimUrl { url: DisplaySafeUrl { scheme: "git+https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/samypr100/test-lfs-repo@657500f0703dc173ac5d68dfa1d7e8c985c84424", query: None, fragment: Some("lfs=true") }, given: None } }, origin: None }}
 ```

It doesn't show as re-installed because we later see that it's up-to-date, but we shouldn't fail this check.

---

_Review comment by @konstin on `crates/uv-git/src/git.rs`:466 on 2025-11-03 20:54_

Do we need both files, or can we go with the `.ok` for both checkout and LFS, and similarly retry both when it's missing?

---

_Review comment by @konstin on `crates/uv-git/src/source.rs`:93 on 2025-11-04 02:55_

What's with the inverse case, we have `lfs = false` but the DB contains LFS artifacts from a previous checkout, how do we prevent the LFS files from still being checked out?

---

_Review comment by @konstin on `crates/uv/tests/it/tool_install.rs`:1863 on 2025-11-04 02:57_

We should use something smaller here, black is convenient but Git plus the black dependencies is noticeable in test suite performance.

---

_@konstin reviewed on 2025-11-04 02:58_

This is great work!

---

_@samypr100 reviewed on 2025-11-04 03:05_

---

_Review comment by @samypr100 on `docs/guides/tools.md`:148 on 2025-11-04 03:05_

Indeed, I was looking for a good example but didn't find any at first. Maybe can use the one reported in https://github.com/astral-sh/uv/issues/16539 😅 

---

_@samypr100 reviewed on 2025-11-04 03:09_

---

_Review comment by @samypr100 on `docs/concepts/projects/dependencies.md`:372 on 2025-11-04 03:09_

On main it does not, but now it does. I'll change to `!!! important`.

---

_@samypr100 reviewed on 2025-11-04 03:15_

---

_Review comment by @samypr100 on `crates/uv/src/commands/project/mod.rs`:1804 on 2025-11-04 03:15_

I went this route to kept it consistent with this other very similar function https://github.com/astral-sh/uv/blob/54596a02082762ee6b04a7f87c60d74024983d5e/crates/uv/src/commands/project/add.rs#L1204-L1286

Do you think we should add it to UnresolvedRequirement instead?

---

_@samypr100 reviewed on 2025-11-04 03:18_

---

_Review comment by @samypr100 on `crates/uv/tests/it/common/mod.rs`:883 on 2025-11-04 03:18_

Good catch, I forgot to remove this. I was testing various configurations locally and needed to disable my system config temporarily as it was getting in the way of tests. I can re-enable it as it shouldn't be a problem in most cases given git's config resolution order.

---

_@samypr100 reviewed on 2025-11-04 03:27_

---

_Review comment by @samypr100 on `crates/uv-git-types/src/lib.rs`:15 on 2025-11-04 03:27_

Yes, it can called 1-4 times depending on the command and cache state.

---

_@samypr100 reviewed on 2025-11-04 03:28_

---

_Review comment by @samypr100 on `crates/uv-git-types/src/lib.rs`:204 on 2025-11-04 03:28_

Agreed, I mostly left it as is from https://github.com/astral-sh/uv/pull/16143/commits/f110cc9ee3a81f6fe52c0a4d1fcb8ddcbe39aa24

---

_@samypr100 reviewed on 2025-11-04 03:29_

---

_Review comment by @samypr100 on `crates/uv/tests/it/common/mod.rs`:917 on 2025-11-04 03:29_

Yes, good catch. I'll add a link.

---

_@samypr100 reviewed on 2025-11-04 03:31_

---

_Review comment by @samypr100 on `crates/uv-resolver/src/lock/mod.rs`:3972 on 2025-11-04 03:31_

I did so for backwards compatibility with users who are already leveraging `UV_GIT_LFS` to have their lockfiles properly updated. Alternatively, we can remove it and keep the lockfiles pristine regardless of `UV_GIT_LFS` envvar being set.

---

_@samypr100 reviewed on 2025-11-04 03:57_

---

_Review comment by @samypr100 on `crates/uv-resolver/src/lock/mod.rs`:3967 on 2025-11-04 03:57_

This is correct. The only scenario where you can explicitly specify `false` is in `pyproject.toml`, ending up in tree-state. All other cases are two-state. The reason being that `false` in pyproject.toml is used only to prevent Git LFS initialization regardless of `UV_GIT_LFS` being set. We could have some symmetry, although I do find it odd to consider these two requirements different when they're the same in the scenario below.

```
     - test-lfs-repo==0.1.0 (from git+https://github.com/samypr100/test-lfs-repo.git@657500f0703dc173ac5d68dfa1d7e8c985c84424#lfs=false)
     + test-lfs-repo==0.1.0 (from git+https://github.com/samypr100/test-lfs-repo.git@657500f0703dc173ac5d68dfa1d7e8c985c84424)
```

---

_@samypr100 reviewed on 2025-11-04 03:58_

---

_Review comment by @samypr100 on `crates/uv-distribution-types/src/requirement.rs`:448 on 2025-11-04 03:58_

Agreed

---

_@samypr100 reviewed on 2025-11-04 03:59_

---

_Review comment by @samypr100 on `crates/uv-cache-key/src/canonical_url.rs`:227 on 2025-11-04 03:59_

Agreed on using `1u8` when `lfs = true`. I was explicitly avoiding the None/false scenario from resulting in a different cache key. In other words, when `lfs = false` under the same repo the cache-key for `GitRepositoryUrl` is the same as the cache key for `RepositoryUrl`.

---

_@samypr100 reviewed on 2025-11-04 04:09_

---

_Review comment by @samypr100 on `crates/uv-git/src/git.rs`:466 on 2025-11-04 04:09_

At least from my testing we would need both files as I've managed to corrupt the cache when we don't have it. This is primarily because `.ok` can be written even when LFS fetch succeeds but the blobs may end up not being present. This can happen due to invalid filter configuration, or other reasons (e.g. uninstalling Git LFS and attempting to update the db with an lfs repo). When validation is skipped for the database we also have no guarantees that the LFS objects are present in the checkout or that they're fresh hence they additional check only on LFS-enabled sources.

---

_@samypr100 reviewed on 2025-11-04 04:16_

---

_Review comment by @samypr100 on `crates/uv-git/src/source.rs`:93 on 2025-11-04 04:16_

When `lfs = false` it goes into a different cache bucket, so it would not be affected by it. Unitializing/removing LFS objects didn't seem like a supported operation either.

The `db` for repo `foo` with `lfs = false` or `None` would go into cache bucket `git-v0/<bucket_cache_key_a>/**`
The `db` for repo `foo` with `lfs = true` would go into cache bucket `git-v0/<bucket_cache_key_b>/**` 

---

_@konstin reviewed on 2025-11-04 13:30_

---

_Review comment by @konstin on `crates/uv-git-types/src/lib.rs`:15 on 2025-11-04 13:30_

The question is like, is this slow enough that we need to use a global `static` for this? Or even better, can we parse the value in one function an pass it around? Sometimes it too much effort to do that, but we try to avoid having global state or accessing env vars in nested functions, it usually comes back to bite us later, either because it's accessed from multiple locations (we had https://github.com/astral-sh/uv/issues/16447 recently) or because you want to change something and the access is in a lot of locations.

---

_Review comment by @samypr100 on `crates/uv/tests/it/tool_install.rs`:1863 on 2025-11-04 13:34_

I believe I kept it black for consistency because all the `tool_install.rs` tests use black. I added this test because I realized there were no tests for tool install using git (no lfs). Is there an alternative you'd think is better?

---

_@samypr100 reviewed on 2025-11-04 13:34_

---

_@samypr100 reviewed on 2025-11-04 13:37_

---

_Review comment by @samypr100 on `crates/uv-installer/src/satisfies.rs`:192 on 2025-11-04 13:37_

Isn't this expected? I consider `lfs = true` and `lfs = false` / `None` as mostly different source trees in the same fashion as different subdirectories. While in the LFS scenario I expect no variability in pyproject.toml contents the final dist may have different sources and assets. 

---

_@samypr100 reviewed on 2025-11-04 14:12_

---

_Review comment by @samypr100 on `crates/uv-git-types/src/lib.rs`:15 on 2025-11-04 14:12_

Yes, when running benchmarks I saw a non-trivial slowdown. It was introduced as a function in f110cc9ee3a81f6fe52c0a4d1fcb8ddcbe39aa24, I later changed it to a global static in 5e791b7659b5ae355a41125f5012beb3f4bde270 due to the performance hit on the airflow benchmark.

---

_Referenced in [astral-sh/uv#16592](../../astral-sh/uv/pulls/16592.md) on 2025-11-04 16:23_

---

_@konstin reviewed on 2025-11-04 18:54_

---

_Review comment by @konstin on `crates/uv/src/commands/project/mod.rs`:1804 on 2025-11-04 18:54_

Fixed: https://github.com/astral-sh/uv/pull/16592

---

_@zanieb reviewed on 2025-11-04 19:14_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_install.rs`:1863 on 2025-11-04 19:14_

I'm also happy to consider switching to a smaller package as a separate issue — we can always make our own small package if we need to.

---

_Review comment by @samypr100 on `docs/concepts/projects/dependencies.md`:372 on 2025-11-05 01:51_

Changed in 2c988eefd997e9b36f93d6aa9da19ee4d08481f1

---

_@samypr100 reviewed on 2025-11-05 01:51_

---

_@samypr100 reviewed on 2025-11-05 01:52_

---

_Review comment by @samypr100 on `crates/uv/src/commands/project/mod.rs`:1804 on 2025-11-05 01:52_

Thanks, I started fix this in 28d9182a923de989a87903d1f3349191eaf49c3d, but I'll rebase your changes and unify them

---

_@samypr100 reviewed on 2025-11-05 01:52_

---

_Review comment by @samypr100 on `crates/uv/tests/it/common/mod.rs`:883 on 2025-11-05 01:52_

Fixed in 2b06aefb25645eebcfa2968105194da3752b98c2

---

_@samypr100 reviewed on 2025-11-05 01:52_

---

_Review comment by @samypr100 on `crates/uv/tests/it/common/mod.rs`:918 on 2025-11-05 01:52_

Fixed in 2b06aefb25645eebcfa2968105194da3752b98c2

---

_@samypr100 reviewed on 2025-11-05 01:53_

---

_Review comment by @samypr100 on `crates/uv/tests/it/sync.rs`:13579 on 2025-11-05 01:53_

Fixed in ed2d9482a16051c48670882560a66fbf696d2cf1

The intentional on/off cache tests ended up primarily being part of edit.rs

---

_@samypr100 reviewed on 2025-11-05 01:53_

---

_Review comment by @samypr100 on `crates/uv-cache-key/src/canonical_url.rs`:209 on 2025-11-05 01:53_

Fixed in aabd0706a467756d8c5f6413dc0047d72d303b66

---

_@samypr100 reviewed on 2025-11-05 01:54_

---

_Review comment by @samypr100 on `crates/uv-cache-key/src/canonical_url.rs`:468 on 2025-11-05 01:54_

Fixed in aabd0706a467756d8c5f6413dc0047d72d303b66

---

_@samypr100 reviewed on 2025-11-05 01:55_

---

_Review comment by @samypr100 on `crates/uv-distribution-types/src/requirement.rs`:448 on 2025-11-05 01:55_

Fixed in b46a479c876e968466be73fbef10eaabda9c0e59, also deleted the else branch as it was not intended.

---

_@samypr100 reviewed on 2025-11-05 01:56_

---

_Review comment by @samypr100 on `crates/uv-cache-key/src/canonical_url.rs`:227 on 2025-11-05 01:56_

Fixed as part of aabd0706a467756d8c5f6413dc0047d72d303b66

---

_@samypr100 reviewed on 2025-11-05 02:13_

---

_Review comment by @samypr100 on `crates/uv/src/commands/project/mod.rs`:1804 on 2025-11-05 02:13_

Fixed in the latest rebase to use your change https://github.com/astral-sh/uv/blob/d28be561c4e18403be8eced740d6b274dc43f2bc/crates/uv/src/commands/project/mod.rs#L1684-L1693

---

_@samypr100 reviewed on 2025-11-05 02:59_

---

_Review comment by @samypr100 on `docs/guides/tools.md`:148 on 2025-11-05 02:59_

Looking into the one from https://github.com/astral-sh/uv/issues/16539 and although it does rely on LFS, there's no scripts block so it doesn't make a good candidate for tools doc 😢

---

_Review comment by @samypr100 on `crates/uv/tests/it/common/mod.rs`:917 on 2025-11-05 03:01_

Fixed in 2b06aefb25645eebcfa2968105194da3752b98c2

---

_@samypr100 reviewed on 2025-11-05 03:01_

---

_@samypr100 reviewed on 2025-11-05 03:17_

---

_Review comment by @samypr100 on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:1440 on 2025-11-05 03:17_

I think the right long-term approach is likely make it an explicit field in the direct-url spec via an [amendment](https://discuss.python.org/t/draft-pep-amendment-to-the-pep-610-direct-url-json-data-structure/22299), which then makes it easier to incorporate it officially into pylock.toml as it uses direct-url spec. More tooling would benefit by having Git LFS related metadata rather than assuming Git LFS will always run and fetch all objects when the extension is available which is sadly the common behavior I've seen in other tools. Its arguably a similar problem to git submodules exclusions problem (e.g. repo with some git submodules you want to exclude).

---

_@konstin reviewed on 2025-11-10 13:22_

---

_Review comment by @konstin on `crates/uv-git-types/src/lib.rs`:15 on 2025-11-10 13:22_

Thanks for checking!

---

_@konstin reviewed on 2025-11-10 14:16_

---

_Review comment by @konstin on `crates/uv-installer/src/satisfies.rs`:192 on 2025-11-10 14:16_

What I meant specifically is that if I run the command in a loop, it fails this check every time:

```
UV_GIT_LFS=true cargo run sync -v
```

For this case, it looks off cause we're using `GitLfs::from_env()` in the nested parsing routines, so it shouldn't disable the fast path.

---

_@konstin reviewed on 2025-11-10 14:43_

---

_Review comment by @konstin on `crates/uv-git-types/src/lib.rs`:204 on 2025-11-10 14:43_

I've been looking at it some more and I don't have a better alternative to offer, changing all call sites including the serde site would be too much churn, and post-processing all requirements is way more likely to get wrong accidentally.

---

_Review comment by @konstin on `crates/uv-distribution/src/error.rs`:91 on 2025-11-10 15:13_

Can we detect whether `git lfs` is installed? Currently, the error message makes it look like the repo is broken when the local machine doesn't have `git lfs` installed. If we can't easily detect it, we should hint at the possibility in the error message.

```
$ uv pip install "git+https://github.com/samypr100/test-lfs-repo@4e82e85f6a8b8825d614ea23c550af55b2b7738c"
    Updated https://github.com/samypr100/test-lfs-repo (4e82e85f6a8b8825d614ea23c550af55b2b7738c)
  × Failed to download and build `test-lfs-repo @ git+https://github.com/samypr100/test-lfs-repo@4e82e85f6a8b8825d614ea23c550af55b2b7738c#lfs=true`
  ╰─▶ The source distribution `git+https://github.com/samypr100/test-lfs-repo@4e82e85f6a8b8825d614ea23c550af55b2b7738c#lfs=true` is missing Git LFS artifacts

```

---

_Comment by @konstin on 2025-11-10 15:29_

I can't comment where the tests are run, but we should enable the `git-lfs` tests where we run the `git` test features, in `cargo-test-macos`.

---

_@konstin reviewed on 2025-11-10 18:28_

---

_Review comment by @samypr100 on `crates/uv-distribution/src/error.rs`:91 on 2025-11-10 18:53_

Yes, it should show up in the verbose logs as a warning. This failure can occur due to multiple scenarios, not just when Git LFS is not installed. For example, the issue could be:
1. Git LFS may not be installed.
2. There an error fetching the LFS blobs (e.g. network or filter related).
3. The LFS blobs themselves are corrupted due to an upstream issue on the repo (e.g. remote missing object)

---

_@samypr100 reviewed on 2025-11-10 18:53_

---

_Comment by @samypr100 on 2025-11-10 19:03_

> I can't comment where the tests are run, but we should enable the `git-lfs` tests where we run the `git` test features, in `cargo-test-macos`.

Done in e8b48e6

---

_@konstin reviewed on 2025-11-11 12:38_

---

_Review comment by @konstin on `crates/uv-distribution/src/error.rs`:91 on 2025-11-11 12:38_

Is there a way to detect which of those we have (CC @geofft who might know)? If not, we should make the error message more generic:

```
    #[error("The source distribution `{}` is missing Git LFS artifacts. Is Git LFS installed?", _0)]
```

---

_@konstin reviewed on 2025-11-11 12:50_

---

_Review comment by @konstin on `crates/uv-git/src/git.rs`:466 on 2025-11-11 12:50_

I'm afraid I still don't follow. As I understand it, LFS and non-LFS checkouts are always separated (due to `GitRepositoryUrl`, something we should document), so shouldn't `.ok` and `.ok_lfs` carry the same information, because depending on the bucket, we either always create one or always both?

---

_Review comment by @konstin on `crates/uv-git/src/source.rs`:72 on 2025-11-11 12:52_

Does the DB need to be separated by LFS/non-LFS? It seems like we double the storage this way.
CC @geofft

---

_Review comment by @konstin on `docs/guides/tools.md`:148 on 2025-11-11 13:49_

I created one: https://github.com/astral-sh/lfs-cowsay

---

_Review comment by @konstin on `crates/uv/tests/it/sync.rs`:13684 on 2025-11-11 13:51_

Done: https://github.com/astral-sh/test-lfs-repo

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:3967 on 2025-11-11 13:59_

I'm thinking about the case of `--frozen`: With the 2-state in `uv.lock`, `lfs = false` is respected with `UV_GIT_LFS=true` with `uv sync`, but ignore with `uv sync --frozen`.

```toml
[project]
name = "test"
version = "0.0.0"
requires-python = ">=3.13"

dependencies = [
  "test-lfs-repo",
]

[tool.uv.sources]
test-lfs-repo = { git = "https://github.com/samypr100/test-lfs-repo", rev = "657500f0703dc173ac5d68dfa1d7e8c985c84424", lfs = false }
```

`UV_GIT_LFS=true cargo run sync --frozen`: No Git LFS used
`UV_GIT_LFS=true cargo run sync --frozen`: Git LFS used

---

_@konstin reviewed on 2025-11-11 14:01_

---

_@samypr100 reviewed on 2025-11-11 14:54_

---

_Review comment by @samypr100 on `crates/uv-git/src/source.rs`:72 on 2025-11-11 14:54_

For context currently in main when we fetch lfs objects at a revision and then turn off lfs, the git db and the corresponding checkout lfs artifacts may not be updated properly anymore which can affect the caches.

Hence from my perspective since the source trees can be entirely different at every commit, I'd opted for this approach.

Currently we store a git repos inside a git-v0/db/**/.git. Once you initialize LFS objects on a repo there's no going back as far as I know unless you either list all the LFS objects and replace them manually with LFS placeholders (like the one in tests).
The checkouts are just a clone of the corresponding git-v0 db so they follow suit for consistency.

I actually started off this branch using a single cache bucket, but encountered the ["reverting LFS objects" scenario](https://github.com/astral-sh/uv/pull/16143#discussion_r2488481580) and opted for a two-bucket approach instead. Happy to revisit ideas if we'd like to stick with one bucket.

---

_@samypr100 reviewed on 2025-11-11 14:56_

---

_Review comment by @samypr100 on `crates/uv-distribution/src/error.rs`:91 on 2025-11-11 14:56_

I'll change the message. The actual errors are shown as a warning when --verbose is used as they're emitted currently in the uv-git crate.

---

_@samypr100 reviewed on 2025-11-11 15:00_

---

_Review comment by @samypr100 on `crates/uv-installer/src/satisfies.rs`:192 on 2025-11-11 15:00_

~I can reproduce, I'll work towards a fix and add a test for this scenario. I noticed the lock file is returning consistent state sometimes because Git Sources are still treated as always immutable (regardless of fragment changes), which is something I missed.~ 

edit: wrong rabbithole, chasing ghosts...

---

_Review comment by @geofft on `crates/uv-distribution-types/src/requirement.rs`:447 on 2025-11-11 15:50_

Mostly for my own understanding, why 1 and not something else like 3 that wouldn't conflict with the subdirectory case? (I suppose a subdirectory won't be empty, but still)

---

_Review comment by @geofft on `crates/uv-distribution-types/src/specified_requirement.rs`:112 on 2025-11-11 15:53_

Mostly for my own understanding, what is this `into()` and the one below doing? Aren't they both bools?

---

_@samypr100 reviewed on 2025-11-11 15:55_

---

_Review comment by @samypr100 on `crates/uv-git/src/git.rs`:466 on 2025-11-11 15:55_

Thanks, I'll update the comment to be clearer. Is below reframing better?

`.ok` means the checkout was successful
`.ok_lfs` means that the lfs fetch was successful and that objects were fully validated

We can have a successful checkout with invalid objects, hence it's acceptable to have a `.ok` and a missing `.ok_lfs`. 
When `.ok_lfs` is missing, a reset is needed to re-attempt to get a copy of the LFS objects.

Are you seeking to avoid writting `.ok` to begin with when lfs fetch was unsuccessful or when objects were not fully validated?

---

_Review comment by @geofft on `crates/uv-git/src/git.rs`:466 on 2025-11-11 16:28_

I think I agree with @konstin's point here - the purpose of the cache key split is that we will never reuse an _existing_ checkout for a Git LFS clone. I can see the argument if we were adding this onto existing repositories on disk, and an existing `.ok` file only indicates that a git checkout of some sort was done and doesn't indicate anything about LFS. But because we're going to create a new working tree under a new cache directory, we're not starting with any `.ok` files on user machines, and so `.ok` inside that new directory always indicates that an LFS-enabled git checkout happened successfully.

Did you run into cache corruption before splitting the cache keys for LFS and non-LFS? I can't see how you could get an LFS clone with `.ok` but not `.ok_lfs`, especially since you're writing those files out at the same time.

Also, now that I think about it, I wonder what happens if we have a non-LFS clone where the user previously had enabled LFS with the environment variable. In that case, `.ok` indicates that the checkout was done _with_ LFS (i.e. with the files smudged on disk), but no further operations in that clone are going to use LFS because you're setting `GIT_LFS_SKIP_SMUDGE`. That seems like it could lead to some corruption (the sort of corruption you referred to in the PR message, basically) and I wonder if we should do something about that case, i.e., basically detect that LFS was in use previously and consider it not fresh and redo the hard reset. (The implementation might need to be clever to avoid forever paying the cost of looking for LFS in every non-LFS clone.)

---

_Review comment by @geofft on `crates/uv-git/src/source.rs`:72 on 2025-11-11 16:50_

If we want to, we can use the same clone for the `db` directory and split it in the `checkouts` directory, because the `db` directory has no working tree, and so its contents shouldn't differ in the LFS and non-LFS cases and it should be equally suitable for making a clone for LFS and non-LFS use.

The actual LFS large files are cached content-addressable in `.git/lfs/objects/` (https://github.com/git-lfs/git-lfs/blob/main/docs/spec.md). As I understand it, they're only going to get populated when the git-lfs filters are executed, i.e., they'll only be populated inside the `checkouts` directory and won't be in `db`. A good further optimization at some point would be to store them in the `db` directory, so that when you create a branch, the LFS objects that haven't changed on that branch are reused from local disk instead of re-fetched from the network.

(It occurs to me that we'd get that for free if we had all the checkouts be worktrees of the db instead of full clones, because then each checkout would not have its own .git directory. Maybe that's worth doing anyway.)

---

_Review comment by @geofft on `crates/uv-git/src/source.rs`:93 on 2025-11-11 16:51_

I think this doesn't address the case of `lfs = false` + the DB contains LFS artifacts _from a previous version of uv_ because `UV_GIT_LFS` was previously set. See my comment on git.rs.

---

_Review comment by @geofft on `crates/uv-distribution/src/error.rs`:91 on 2025-11-11 17:01_

I think our options are
* we can parse out the error from `git reset` (something like `smudge filter lfs failed` I think, unfortunately this message is localizable and there's no machine-readable status or exit code or anything)
* we can provide our own git-lfs setup where we wrap execution of the filter in our own monitor process that reports how it went
* on error, we can see if (we believe) `git-lfs` is on PATH and display a heuristic error message if it's not asking the user to install it

---

_@geofft reviewed on 2025-11-11 17:02_

---

_@samypr100 reviewed on 2025-11-11 17:41_

---

_Review comment by @samypr100 on `crates/uv-distribution/src/error.rs`:91 on 2025-11-11 17:41_

For context, please see https://github.com/astral-sh/uv/blob/044be175f7504db9c312e9bad633a7950f2c314a/crates/uv-git/src/git.rs#L210 which already does this and shows the warning.

Message has been changed in 5f294d46234c9beb4e9f5a6bbc6ca7b4853df1d2

---

_Review comment by @samypr100 on `crates/uv-git/src/source.rs`:72 on 2025-11-11 17:57_

I think it's worthwhile to re-think the clones vs worktrees, but probably out of scope for this PR? We'd also have to consider git and git-lfs versions on the client from a compatibility perspective. If I'm not mistaken, I think the current git implementation aligns more closer to cargo's implementation.

> The actual LFS large files are cached content-addressable in .git/lfs/objects/ (https://github.com/git-lfs/git-lfs/blob/main/docs/spec.md). As I understand it, they're only going to get populated when the git-lfs filters are executed, i.e., they'll only be populated inside the checkouts directory and won't be in db.

Yes, the `.git/lfs/objects/` will have the objects (either in a successful or failed state), but I don't think this changes things unless I'm missing something based on what I had previously mentioned. My previous comment still holds that you'd end up with two types of checkouts (one with LFS, one without) as they're different source trees. 

> A good further optimization at some point would be to store them in the db directory, so that when you create a branch, the LFS objects that haven't changed on that branch are reused from local disk instead of re-fetched from the network.

The objects are not re-fetched through the network unless they were not initialized (e.g. using Git LFS 2.x) correctly as they're already stored in  `git-v0/db/**/.git/lfs/objects/` .

---

_@samypr100 reviewed on 2025-11-11 17:57_

---

_@geofft reviewed on 2025-11-11 18:08_

---

_Review comment by @geofft on `crates/uv-git/src/source.rs`:72 on 2025-11-11 18:08_

> I think it's worthwhile to re-think the clones vs worktrees, but probably out of scope for this PR?

Yes, I agree that changing this is out of scope for the PR. To be clear I'm fine with merging this PR with the different clones for LFS and non-LFS, and figuring out if we can avoid that later.

> The objects are not re-fetched through the network unless they were not initialized (e.g. using Git LFS 2.x) correctly as they're already stored in `git-v0/db/**/.git/lfs/objects/` .

Aren't they in `git-v0/checkouts/**/.git/lfs/objects/`? (I can go test this and see what happens :) )

---

_@samypr100 reviewed on 2025-11-11 18:14_

---

_Review comment by @samypr100 on `crates/uv-git/src/git.rs`:466 on 2025-11-11 18:14_

> Did you run into cache corruption before splitting the cache keys for LFS and non-LFS?

I'll have to revisit this, the issue was more prominent when a single cache bucket was being used and its when I added it originally. I think we can get rid of the `.ok_lfs` if only if we stay with two cache buckets and change `.ok` to also mean successful LFS validation when LFS is enabled.

> Also, now that I think about it, I wonder what happens if we have a non-LFS clone where the user previously had enabled LFS with the environment variable. In that case, .ok indicates that the checkout was done with LFS (i.e. with the files smudged on disk), but no further operations in that clone are going to use LFS because you're setting GIT_LFS_SKIP_SMUDGE. That seems like it could lead to some corruption (the sort of corruption you referred to in the PR message, basically) and I wonder if we should do something about that case, i.e., basically detect that LFS was in use previously and consider it not fresh and redo the hard reset. (The implementation might need to be clever to avoid forever paying the cost of looking for LFS in every non-LFS clone.)

Yes, I'd expect these would immediatedly switch over to the new LFS cache bucket instead. I don't think we can do anything about the fact that right now you can mess up the git cache when you do operations with UV_GIT_LFS and then removing it. I'd expect this PR would make that behavior deterministic going forward, but an old corrupted cache by mixing UV_GIT_LFS in and out is not something this PR currently fixes... although we can fix it by changing the cache-key for non-lfs buckets (effectively invalidating the old git caches entirely) which I tried to avoid.
e.g. `if git.lfs().enabled() { 1u8.cache_key(state);} else { 0u8.cache_key(state); }`

---

_@samypr100 reviewed on 2025-11-11 18:22_

---

_Review comment by @samypr100 on `crates/uv-resolver/src/lock/mod.rs`:3967 on 2025-11-11 18:22_

I think this is related to the issue identified in https://github.com/astral-sh/uv/pull/16143#discussion_r2514568324 which needs to be fixed.

---

_@samypr100 reviewed on 2025-11-11 18:24_

---

_Review comment by @samypr100 on `crates/uv-distribution-types/src/specified_requirement.rs`:112 on 2025-11-11 18:24_

They're not both bools, `git.with_lfs` takes `GitLfs` and the `.into()` converts it accordingly.

I switched it to use from so it's clearer in b0361afd4c6f2b2c15aaac3bcaf22829ac97f7b7

---

_@samypr100 reviewed on 2025-11-11 18:25_

---

_Review comment by @samypr100 on `crates/uv-distribution-types/src/requirement.rs`:447 on 2025-11-11 18:25_

I don't think it would matter if it's 3? I mainly kept the convention of boolean flags setting 1 or 0 on the cache state.

---

_@samypr100 reviewed on 2025-11-11 18:27_

---

_Review comment by @samypr100 on `crates/uv-git/src/source.rs`:72 on 2025-11-11 18:27_

> Aren't they in git-v0/checkouts/**/.git/lfs/objects/?

They'd be in both as the checkout clones the lfs-enabled git db

---

_@samypr100 reviewed on 2025-11-12 04:13_

---

_Review comment by @samypr100 on `crates/uv/tests/it/sync.rs`:13684 on 2025-11-12 04:13_

The tests have been changed to use this repo, thanks.

---

_@samypr100 reviewed on 2025-11-12 04:13_

---

_Review comment by @samypr100 on `docs/guides/tools.md`:148 on 2025-11-12 04:13_

The docs have been changed to use this repo, thanks.

---

_@samypr100 reviewed on 2025-11-12 06:48_

---

_Review comment by @samypr100 on `crates/uv-git/src/git.rs`:466 on 2025-11-12 06:48_

>  I think we can get rid of the .ok_lfs if only if we stay with two cache buckets and change .ok to also mean successful LFS validation when LFS is enabled.

The `.ok_lfs` file was removed in 7ca27c3960bf7cbc2091c72f862b4ca0ee09a3a1 with this approach. I'll be doing some more stress testing locally as well as a follow up.

---

_@samypr100 reviewed on 2025-11-12 07:00_

---

_Review comment by @samypr100 on `crates/uv-installer/src/satisfies.rs`:192 on 2025-11-12 07:00_

@konstin I believe this should be fixed now. I failed to capture the exact commit for the fix but tl;dr the lowering changes had a bug which explained why we weren't seeing lfs state fragments propagating correctly via the environment variable. The fix helped remove other from_env calls that were no longer needed / not intended to be there. Additional checks in sync.rs tests were added to validate the integrity of the lockfile at different stages.

---

_@samypr100 reviewed on 2025-11-12 07:03_

---

_Review comment by @samypr100 on `crates/uv-resolver/src/lock/mod.rs`:3967 on 2025-11-12 07:03_

I think this is fixed by https://github.com/astral-sh/uv/pull/16143#discussion_r2517137301 (I can't repro anymore)

---

_@konstin reviewed on 2025-11-12 14:02_

---

_Review comment by @konstin on `crates/uv-distribution-types/src/requirement.rs`:447 on 2025-11-12 14:02_

The sequence is hashed in order, so you get like `[2, <url>, 0, 1]` or `[2, <url>, 1, <url>, 1]`, given that there's always a number there can't be a collision.

We should add an else-branch with 0 for future-additions-compat though.

---

_@konstin reviewed on 2025-11-12 14:06_

---

_Review comment by @konstin on `crates/uv-distribution/src/error.rs`:91 on 2025-11-12 14:06_

Can we check for `GIT_LFS` and adjust the error message accordingly?

---

_Review comment by @samypr100 on `crates/uv-distribution-types/src/requirement.rs`:447 on 2025-11-12 14:16_

> We should add an else-branch with 0 for future-additions-compat though.

Added in 0349288d4223f8c9d52412f405f9a7ffaf42f951

---

_@samypr100 reviewed on 2025-11-12 14:16_

---

_@samypr100 reviewed on 2025-11-12 14:18_

---

_Review comment by @samypr100 on `crates/uv-distribution-types/src/requirement.rs`:447 on 2025-11-12 14:18_

Sorry, I had it originally, but misunderstood later on feedback whether we wanted to have a cache churn or not.

---

_@konstin reviewed on 2025-11-12 14:30_

---

_Review comment by @konstin on `crates/uv-distribution-types/src/requirement.rs`:447 on 2025-11-12 14:30_

Right, that's why we didn't add it initially. The cache churn vs. compat is always a tough one; I'd learn towards cache invalidation to avoid problems in the future, given that we make no guarantees about the cache cross version anyway.

---

_Review comment by @konstin on `.github/workflows/ci.yml`:304 on 2025-11-12 14:34_

```suggestion
            --features python,python-managed,pypi,git,git-lfs,performance,crates-io,native-auth,apple-native \
```

---

_@samypr100 reviewed on 2025-11-12 14:39_

---

_Review comment by @samypr100 on `crates/uv-distribution/src/error.rs`:91 on 2025-11-12 14:39_

Do you mean check again if Git LFS is available in the path? Is it not sufficient that we already show this as a warning?

---

_Review comment by @konstin on `crates/uv-cache-key/src/canonical_url.rs`:208 on 2025-11-12 15:45_

nit: It doesn't do that anymore

---

_@samypr100 reviewed on 2025-11-13 05:40_

---

_Review comment by @samypr100 on `crates/uv-distribution/src/error.rs`:91 on 2025-11-13 05:40_

I've improved the error reporting as part of feac4215e1b944a4761957d7cc6d64c0672debea

e.g.
```
  × Failed to download and build `lfs-cowsay @ git+https://github.com/astral-sh/lfs-cowsay@44b8e65c8ecd376e4482a68b29312227879a226d#lfs=true`
  ├─▶ The source distribution `git+https://github.com/astral-sh/lfs-cowsay@44b8e65c8ecd376e4482a68b29312227879a226d#lfs=true` is missing Git LFS artifacts.
  ╰─▶ Is Git LFS configured? You may need to run `git lfs install`.
```

or

```
  × Failed to download and build `lfs-cowsay @ git+https://github.com/astral-sh/lfs-cowsay@44b8e65c8ecd376e4482a68b29312227879a226d#lfs=true`
  ├─▶ The source distribution `git+https://github.com/astral-sh/lfs-cowsay@44b8e65c8ecd376e4482a68b29312227879a226d#lfs=true` is missing Git LFS artifacts.
  ╰─▶ Git LFS extension not found. Ensure that Git LFS is installed and available.
```

---

_@konstin approved on 2025-11-13 09:45_

The only remaining decision is `0u8.cache_key(state);` and probably a final read-through, otherwise it's good to go!

---

_Comment by @geofft on 2025-11-13 12:27_

I'm taking a look at this in a little more detail. I really want to avoid churning the cache because that has an impact on non-LFS git users, and I think we can do that safely.

---

_@samypr100 reviewed on 2025-11-13 13:13_

---

_Review comment by @samypr100 on `crates/uv-cache-key/src/canonical_url.rs`:208 on 2025-11-13 13:13_

Thanks, fixed in [commit](https://github.com/astral-sh/uv/compare/77302b16151a0f5695ef18fe3b74e28534900266..e2b1a8969407f1f7440706be4bcb4d8f79df28d0)

---

_@samypr100 reviewed on 2025-11-13 13:13_

---

_Review comment by @samypr100 on `.github/workflows/ci.yml`:304 on 2025-11-13 13:13_

Thanks, fixed in [commit](https://github.com/astral-sh/uv/compare/77302b16151a0f5695ef18fe3b74e28534900266..e2b1a8969407f1f7440706be4bcb4d8f79df28d0)

---

_Review comment by @geofft on `docs/guides/tools.md`:148 on 2025-11-13 13:19_

nit: the repo name matches the command name so `uvx --lfs git+https://github.com/astral-sh/lfs-cowsay` works too

---

_Review comment by @geofft on `crates/uv-git/src/git.rs`:803 on 2025-11-13 14:12_

I don't understand this - `git lfs fsck` seems to validate LFS objects based on looking at git commit objects (or the index). It doesn't validate files in the working copy, and in any case a GitRepository has no files on disk. So, smudge/clean filters, which only apply to the working copy, should not be relevant here. What does this check do?

Same with the `GIT_LFS_SKIP_SMUDGE` environment variable right above - this shouldn't impact `git lfs fetch`, should it?

---

_@geofft requested changes on 2025-11-13 14:39_

OK, I think I understand the flow a little bit better:

* In `db`, we clone the actual remote repo and also do a `git lfs fetch` of the commit in question, which looks at the git commit object for LFS pointers, pulls down the LFS objects from the remote, and stores them in `.git/lfs/objects/` content-addressed. Files are never checked out (into the working tree) in `db`.
* In `checkouts`, we clone the db (i.e. our origin is the db and there is no reference to the actual remote repo on the internet) and check out the specified commit. When LFS is enabled, this triggers the LFS smudge filters, which internally fetch the LFS objects from the origin, populate `.git/lfs/objects/` in the current clone (using hardlinks, thankfully), and then smudge them onto disk in the working tree.

This means that it is safe to share the db between LFS and non-LFS URLs, and moreover it is safe to share the db between older versions of uv with or without `UV_GIT_LFS` set and current versions of uv. There is no working tree to have inconsistent state, and the `.git/lfs/objects/` storage is content-addressed and additive: if there's stuff there it's correct, and new stuff can be safely added without impacting other use of the repository.

Because of the risk of inconsistency with smudge filters in actual checkouts, I can see the argument for invalidating the cache key for both LFS and non-LFS checkouts. Also, because checkouts are hard-linked from db, their disk usage is smaller, and so I'm less concerned about leaving old/unreachable checkouts on disk than old/unreachable db clones: the wasted disk space is analogous to using another branch/ref.

So I would request:
* In the cache key used for `db`, do not record LFS-ness at all; keep the cache key the same as it was before this PR.
* In the cache key used for `checkouts`, record both LFS-ness and non-LFS-ness (as this PR currently does).

I do think it's possible to avoid changing the cache key for non-LFS checkouts (there are a few ways, to do this my actual preferred option would be to error out if it's an LFS repo as indicated by `.gitattributes` and LFS is not in use, which would force LFS repos to use the new cache key and never the old one), but I'm okay with not making that change.

---

_Comment by @samypr100 on 2025-11-13 16:14_

> 
> So I would request:
> 
> * In the cache key used for `db`, do not record LFS-ness at all; keep the cache key the same as it was before this PR.
> * In the cache key used for `checkouts`, record both LFS-ness and non-LFS-ness (as this PR currently does).
> 

I believe I had this before on one of the original iterations to avoid the cache churn. Are we settled this is what we want?

also, thoughts on this other location per your request?

https://github.com/astral-sh/uv/blob/e2b1a8969407f1f7440706be4bcb4d8f79df28d0/crates/uv-distribution-types/src/requirement.rs#L446-L450

edit: moved discussion to https://github.com/astral-sh/uv/pull/16143#discussion_r2525750175

---

_@samypr100 reviewed on 2025-11-14 01:25_

---

_Review comment by @samypr100 on `crates/uv-git/src/git.rs`:803 on 2025-11-14 01:25_

<img width="609" height="129" alt="lfs_testing" src="https://github.com/user-attachments/assets/613b1b5f-f61e-4e4c-acff-fe91280f8842" />

There was some degree of testing I was doing and found that validating LFS objects this way `fsck --objects` was the proper path forward for both the db and the checkouts after 3.0.2.
In other words the validation is needed to indicate whether the lfs fetch for the db cache is consistent to the oid we're working on. Same for the checkouts.
It's relevant to prevent the rountrip on initialization we may otherwise need.
Thanks for highlighting though, I realized this comment talks about filters which is in the wrong spot as you're right the filters apply on the checkout only. I'll reword for clarity of general intent/reason.

> Same with the GIT_LFS_SKIP_SMUDGE environment variable right above - this shouldn't impact git lfs fetch, should it?

Agreed, I added this mainly as a precaution and future proofing paranoia. I'll adjust the comments to make it more clear.

---

_@samypr100 reviewed on 2025-11-14 02:06_

---

_Review comment by @samypr100 on `docs/guides/tools.md`:148 on 2025-11-14 02:06_

01bb6310ffc28cdc89f8eaf61c264d069a39a592

---

_@samypr100 reviewed on 2025-11-14 02:06_

---

_Review comment by @samypr100 on `crates/uv-git/src/git.rs`:803 on 2025-11-14 02:06_

Comments clarified in c2e08262ce9c0ff554f22bd8b099aed350fe1bc3

---

_@samypr100 reviewed on 2025-11-14 05:02_

---

_Review comment by @samypr100 on `crates/uv-distribution-types/src/requirement.rs`:447 on 2025-11-14 05:02_

> > So I would request:
> > 
> > * In the cache key used for `db`, do not record LFS-ness at all; keep the cache key the same as it was before this PR.
> > * In the cache key used for `checkouts`, record both LFS-ness and non-LFS-ness (as this PR currently does).
> 
> I believe I had this before on one of the original iterations to avoid the cache churn. Are we settled this is what we want?
> 
> also, thoughts on this other location per your request?
> 
> https://github.com/astral-sh/uv/blob/e2b1a8969407f1f7440706be4bcb4d8f79df28d0/crates/uv-distribution-types/src/requirement.rs#L446-L450

@geofft for now, I've added this in 9b06e1c0575e6cc05b99be0e38d93aed491d782e
Let me know if there's any changes you'd like in [uv-distribution-types/src/requirement.rs](https://github.com/astral-sh/uv/blob/e2b1a8969407f1f7440706be4bcb4d8f79df28d0/crates/uv-distribution-types/src/requirement.rs#L446-L450) as shown in the quote.

---

_@samypr100 reviewed on 2025-11-14 05:15_

---

_Review comment by @samypr100 on `docs/guides/tools.md`:148 on 2025-11-14 05:15_

I've reverted this because it didn't work without stating the tool name when I was trying it out.

---

_Label `test:macos` added by @samypr100 on 2025-11-14 05:23_

---

_Review requested from @geofft by @samypr100 on 2025-11-17 13:32_

---

_@geofft approved on 2025-11-18 19:28_

Thanks, and sorry for the delay in the re-review. I'm not totally following some of the code changes here but the behavior does seem to match what we want, i.e., sharing the `db/*` directory and only having separate `checkouts/*/*` directories. I might take a look at that later and send you something for review but that doesn't block merging this.

---

_@messerli-wallace approved on 2025-11-18 19:32_

---

_Comment by @geofft on 2025-11-18 19:33_

There is one theoretical backwards-incompatibility here: with current uv, you only have to specify `UV_GIT_LFS=1` _once_ and the checkout and built wheel gets cached with the LFS artifacts included. This PR (correctly) identifies that as a risk to cache consistency, but I think in practice it may work fine for users with git-lfs installed and configured properly, because the `.ok` file won't get created if the LFS fetch fails or otherwise leaves you in an inconsistent state, and I am worried that people might be relying on that in practice.

With current uv:

```
$ UV_CACHE_DIR=/tmp/cache/q2-old UV_GIT_LFS=1 uvx git+https://github.com/astral-sh/lfs-cowsay -v
    Updated https://github.com/astral-sh/lfs-cowsay (44b8e65c8ecd376e4482a68b29312227879a226d)
      Built lfs-cowsay @ git+https://github.com/astral-sh/lfs-cowsay@44b8e65c8ecd376e4482a68b29312227879a226d
Installed 1 package in 2ms
6.1
$ UV_CACHE_DIR=/tmp/cache/q2-old uvx git+https://github.com/astral-sh/lfs-cowsay -v
6.1
```

With the code from this PR:

```
$ UV_CACHE_DIR=/tmp/cache/q2-new UV_GIT_LFS=1 target/debug/uvx git+https://github.com/astral-sh/lfs-cowsay -v
    Updated https://github.com/astral-sh/lfs-cowsay (44b8e65c8ecd376e4482a68b29312227879a226d)
      Built lfs-cowsay @ git+https://github.com/astral-sh/lfs-cowsay@44b8e65c8ecd376e4482a68b29312227879a226d#lfs=true
Installed 1 package in 19ms
6.1
$ UV_CACHE_DIR=/tmp/cache/q2-new target/debug/uvx git+https://github.com/astral-sh/lfs-cowsay -v
      Built lfs-cowsay @ git+https://github.com/astral-sh/lfs-cowsay@44b8e65c8ecd376e4482a68b29312227879a226d
Installed 1 package in 18ms
Traceback (most recent call last):
  File "/tmp/cache/q2-new/archive-v0/UHbaC9QcSG9WzgquNtN5V/bin/lfs-cowsay", line 6, in <module>
    from lfs_cowsay.__main__ import cli
  File "/tmp/cache/q2-new/archive-v0/UHbaC9QcSG9WzgquNtN5V/lib/python3.14/site-packages/lfs_cowsay/__init__.py", line 4, in <module>
    from .main import (
    ...<13 lines>...
    )
  File "/tmp/cache/q2-new/archive-v0/UHbaC9QcSG9WzgquNtN5V/lib/python3.14/site-packages/lfs_cowsay/main.py", line 4, in <module>
    from .characters import CHARS
  File "/tmp/cache/q2-new/archive-v0/UHbaC9QcSG9WzgquNtN5V/lib/python3.14/site-packages/lfs_cowsay/characters.py", line 2
    oid sha256:4a003717d1e1997c56ef4128f54eb92c72831e44cda4e3d49e93a32ae0cc9222
               ^
SyntaxError: invalid decimal literal
```

(This does not affect `UV_GIT_LFS=1 uv tool install blah` + `uv tool upgrade`, though - we correctly persist the `?lfs=true` in the URL. It only affects two independent parses of the same git repo/commit that do not share persistent state but share the cache.)

Again, I don't think this is a problem in practice / don't think this should block merging.

---

_Referenced in [astral-sh/uv#12156](../../astral-sh/uv/pulls/12156.md) on 2025-11-21 16:50_

---

_Review comment by @konstin on `crates/uv-distribution-types/src/requirement.rs`:449 on 2025-11-28 13:13_

We need to remove the 0 here too, otherwise we invalidate the built wheel cache, through extra-build-requires.

---

_Review comment by @konstin on `crates/uv-git-types/src/lib.rs`:16 on 2025-11-28 13:36_

We need to use something like `parse_boolish_environment_variable` here, otherwise `UV_GIT_LFS=true` and `UV_GIT_LFS=false` do the same thing.

---

_Review comment by @konstin on `crates/uv-installer/src/satisfies.rs`:197 on 2025-11-28 13:37_

nit: We can use a `Display` impl

---

_Review comment by @konstin on `docs/concepts/projects/dependencies.md`:368 on 2025-11-28 14:11_

We need something about the default, probably a different term than "resolved"

> - By default, Git LFS entries are not resolved

---

_@konstin reviewed on 2025-11-28 14:12_

---

_@samypr100 reviewed on 2025-11-28 22:27_

---

_Review comment by @samypr100 on `crates/uv-distribution-types/src/requirement.rs`:449 on 2025-11-28 22:27_

Sounds good, I asked in https://github.com/astral-sh/uv/pull/16143#discussion_r2525750175 if we also wanted to remove this.

---

_Review comment by @samypr100 on `crates/uv-git-types/src/lib.rs`:16 on 2025-11-28 22:30_

Fair. I kept the original behavior in case someone is relying on it with what would be a falsy value.

parse_boolish_environment_variable is currently in uv-settings which is quite high up in the graph, would you copy that helper or move it into some other location?

<img width="2664" height="2536" alt="uv" src="https://github.com/user-attachments/assets/295ff8f4-f61b-4064-a3d7-8626f7dc18ba" />


---

_@samypr100 reviewed on 2025-11-28 22:30_

---

_@samypr100 reviewed on 2025-11-28 22:42_

---

_Review comment by @samypr100 on `docs/concepts/projects/dependencies.md`:368 on 2025-11-28 22:42_

Agreed, 60f0ea8506d61d48fb04a5b089ad9aa552863183

---

_@samypr100 reviewed on 2025-11-28 22:42_

---

_Review comment by @samypr100 on `crates/uv-distribution-types/src/requirement.rs`:449 on 2025-11-28 22:42_

Removed in 200b65ac53a9015ab30ea6e0845317e5bd99f847

---

_@samypr100 reviewed on 2025-11-28 22:42_

---

_Review comment by @samypr100 on `crates/uv-distribution-types/src/requirement.rs`:447 on 2025-11-28 22:42_

https://github.com/astral-sh/uv/pull/16143#discussion_r2572624380

---

_@samypr100 reviewed on 2025-11-28 22:51_

---

_Review comment by @samypr100 on `crates/uv-git-types/src/lib.rs`:16 on 2025-11-28 22:51_

I changed it to at least consider empty values as false in 2377aa3a7aa0794bcd8127f883abfb88db7012b8 for the time being to align on some of the others that don't use `parse_boolish_environment_variable` yet 

---

_@samypr100 reviewed on 2025-11-28 23:14_

---

_Review comment by @samypr100 on `crates/uv-installer/src/satisfies.rs`:197 on 2025-11-28 23:14_

Good point, addc8ed0873968de8d6b0295bc212e498d1d3892

---

_@samypr100 reviewed on 2025-11-29 00:44_

---

_Review comment by @samypr100 on `crates/uv-git-types/src/lib.rs`:16 on 2025-11-29 00:44_

I proceeded with copying the check inline in b88ff97c13a69b8aee20f3fe13d8fd8a84ed7823 as I think this is effectively the same end result

---

_Referenced in [astral-sh/uv#16927](../../astral-sh/uv/pulls/16927.md) on 2025-12-02 11:05_

---

_Renamed from "Conditional Git LFS Support" to "Support Git LFS with opt-in" by @konstin on 2025-12-02 11:44_

---

_@konstin approved on 2025-12-02 12:10_

Thank you!

---

_Merged by @konstin on 2025-12-02 12:23_

---

_Closed by @konstin on 2025-12-02 12:23_

---

_Referenced in [Homebrew/homebrew-core#256853](../../Homebrew/homebrew-core/pulls/256853.md) on 2025-12-03 03:08_

---

_Comment by @Red-Eyed on 2025-12-03 06:17_

Thank you! 
Having worked with LFS, and I wonder how to deal with different LFS endpoints and credentials?
For example, at work we have GitHub enterprise and external git LFS server (not github)

---

_Comment by @samypr100 on 2025-12-03 14:34_

> Thank you! Having worked with LFS, and I wonder how to deal with different LFS endpoints and credentials? For example, at work we have GitHub enterprise and external git LFS server (not github)

@Red-Eyed That would still be configured as part of your git configuration. Since LFS objects have the originating url on them, as long as git itself can authenticate, fetch, and materialize the objects you shouldn't have issues with uv.

---

_Branch deleted on 2025-12-03 14:39_

---

_Comment by @gsemet on 2025-12-06 10:15_

Great ! Is there a way to use it when doing « uv tool install git+https//… »?

---

_Comment by @samypr100 on 2025-12-06 12:08_

> Great ! Is there a way to use it when doing « uv tool install git+https//… »?

uv tool install --lfs

---
