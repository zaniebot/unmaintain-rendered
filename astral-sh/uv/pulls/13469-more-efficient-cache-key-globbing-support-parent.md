```yaml
number: 13469
title: More efficient cache-key globbing + support parent paths in globs
type: pull_request
state: merged
author: aldanor
labels: []
assignees: []
merged: true
base: main
head: feat/resolve-cache-key-globs
created_at: 2025-05-15T14:07:29Z
updated_at: 2025-07-11T15:35:59Z
url: https://github.com/astral-sh/uv/pull/13469
synced_at: 2026-01-12T16:10:42Z
```

# More efficient cache-key globbing + support parent paths in globs

---

_@aldanor_

## Summary

(Related PR: #13438 - would be nice to have it merged as well since it touches on the same globwalker code)

There's a few issues with `cache-key` globs, which this PR attempts to address:

- As of the current state, parent or absolute paths are not allowed, which is not obvious and is not documented. E.g., cache-key paths of the form `{file = "../dep/**"}` will be essentially ignored.
- Absolute glob patterns also don't work (funnily enough, there's logic in `globwalk` itself that attempts to address it in [`globwalk::glob_builder()`](https://github.com/Gilnaa/globwalk/blob/8973fa2bc560be54c91448131238fa50d56ee121/src/lib.rs#L415), which serves as inspiration to some parts of this PR).
- The reason for parent paths being ignored is the way globwalker is currently being triggered in `uv-cache-info`: the base directory is being walked over completely and each entry is then being matched to one of the provided match patterns.
- This may also end up being very inefficient if you have a huge root folder with thousands of files: if your match patterns are `a/b/*.rs` and `a/c/*.py` then instead of walking over the root directory, you can just walk over `a/b` and `a/c` and match the relevant patterns there.
- Why supporting parent paths may be important to the point of being a blocker: in large codebases with python projects depending on other local non-python projects (e.g. rust crates), cache-keys can be very useful to track dependency on the source code of the latter (e.g. `cache-keys = [{ file = "../../crates/some-dep/**" }]`.
- TLDR: parent/absolute cache-key globs don't work, glob walk can be slow. 

## Solution

- In this PR, user-provided glob patterns are first clustered (LCP-style) into pattern groups with longest common path prefix; each of these groups can then be walked over separately.
- Pattern groups do not overlap, so we would never walk over the same directory twice (unless there's symlinks pointing to same folders).
- Paths are not canonicalized nor virtually normalized (which is impossible on Unix without FS access), so the method is symlink-safe (i.e. we don't treat `a/b/..` as `a`) and should work fine with #13438.
- Because of LCP logic, the minimal amount of directory space will be traversed to cover all patterns.
- Absolute glob patterns will now work.
- Parent-relative glob patterns will now work.
- Glob walking will be more efficient in some cases.

## Possible improvements

- Efficiency can be further greatly improved if we limit max depth for globwalk. Currently, a simple ".toml" will deep-traverse the whole folder. Essentially, max depth can be always set to either N or infinity. If a pattern at a pivot node contains `**`, we collect all children nodes from the subtree into the same group and don't limit max depth; otherwise, we set max depth to the length of the glob pattern. This wouldn't change correctness though and can we done separately if needed.
- If this is considered important enough, docs can be updated to indicate that parent and absolute globs are supported (and symlinks are resolved, if the relevant PR is algo merged in).

## Test Plan

- Glob splitting and clustering tests are included in the PR.
- Relative and absolute glob cache-keys were tested in an actual codebase.

---

_Review requested from @BurntSushi by @konstin on 2025-05-15 14:08_

---

_Review comment by @BurntSushi on `crates/uv-cache-info/src/glob.rs`:39 on 2025-05-15 17:57_

Maybe `if last.map_or(false, |last| last != Component::CurDir) {` to collapse these `if` expressions?

---

_Review comment by @BurntSushi on `crates/uv-cache-info/src/glob.rs`:11 on 2025-05-15 18:00_

I think this needs to check for `[` and `]` too. I think [that's it](https://docs.rs/globset/latest/src/globset/lib.rs.html#944-961).

---

_Review comment by @BurntSushi on `crates/uv-cache-info/src/glob.rs`:14 on 2025-05-15 18:04_

Can you document the contract for this function? In particular, I think it is necessary that this always return true if the given part returns something different if it's interpreted as a glob versus if it was interpreted as a literal. But that it can also return true in cases even when the result is identical. That is, this routine allows false positives but not false negatives.

I think this is how it's used too. That is, if you interpret a path component as a glob when it isn't, then I think the worst thing that happens is that you traverse more of the directory tree? (i.e., You lose on perf but not correctness.)

I'm thinking about components like `foo{bar` or `foo[bar`.

---

_Review comment by @BurntSushi on `crates/uv-cache-info/src/glob.rs`:25 on 2025-05-15 18:06_

This might be vestigial? It's pushed to below, but then appears unused.

---

_Review comment by @BurntSushi on `crates/uv-cache-info/src/glob.rs`:25 on 2025-05-15 18:08_

I do wonder if this code can be simplified by doing `let mut current = &mut glob.base;`. And then in the loop below:

```rust
if !globbing && is_glob_like(part) {
    current = &mut glob.pattern;
}
```

Then you can just do `current.push(last);` in both places and avoid the extra case analysis on `globbing`.

(I'm wondering if this is what you had and it didn't work for some reason.)

---

_@BurntSushi reviewed on 2025-05-15 18:13_

I started reviewing this, but realized that before I got too much into the weeds, it would be good to check the motivation first: do you have any benchmarks for this? The more realistic, the better. I do grant that there is likely some improvement here, but it's important to contextualize it with 1) how common that case is and 2) if the improvement is worth the extra implementation cost here.

---

_Comment by @aldanor on 2025-05-15 18:47_

> I started reviewing this, but realized that before I got too much into the weeds, it would be good to check the motivation first: do you have any benchmarks for this? The more realistic, the better. I do grant that there is likely some improvement here, but it's important to contextualize it with 1) how common that case is and 2) if the improvement is worth the extra implementation cost here.

@BurntSushi - thanks again for taking a look.

I think PR title/description might be a bit confusing - sorry about that and let me try to explain:
- The main point (and the blocker we are currently facing in a production codebase) is being able to use relative/parent paths in cache-key globs, to define dependencies on local folders that are not descendants of the current python project.
- The way globwalk is currently being used, it's being called with project directory as the base, and simply traverses all of it. Of course, this will not catch any entries outside of CWD.
- In order to cover patterns that may contain (or start with) `..`, we would have to:
    1. do multiple globwalks
    2. figure out which base directories to use for those globwalks
- Point (ii) is the reason for `split_glob()` to exist (which is somewhat similar to `glob_builder()` in globwalk crate).
- As for (i), if we simply kick off a globwalk for each glob entry, we will traverse the same folders multiple times. In order to not do that, we need some LCP logic - which is the reason for `cluster_globs()` to exist.
- Finally, - and it just happens so, coincidentally and automatically, - we also happen to make the general use case more efficient in case your CWD has a deep tree and many entries but globs only point to particular parts of it. This is not the main intent of this PR, but more of a side bonus. There should be no perf regressions but sometimes there will be random perf wins.
  - As mentioned above, if we really want to make it as efficient as possible, we can also set max depth based on glob patterns and not always walk through the entire directory tree with infinite depth (this will also change the way globs are grouped into clusters). If need be, it can be implemented as a separate PR.

---

_@aldanor reviewed on 2025-05-15 19:05_

---

_Review comment by @aldanor on `crates/uv-cache-info/src/glob.rs`:25 on 2025-05-15 19:05_

Good catch, this is a leftover from a previous version that wasn't cleaned up after a refactor.

---

_@aldanor reviewed on 2025-05-15 19:09_

---

_Review comment by @aldanor on `crates/uv-cache-info/src/glob.rs`:39 on 2025-05-15 19:09_

Don't think it would work, because inside this scope we need `last` to be `Component`.

---

_@aldanor reviewed on 2025-05-15 19:09_

---

_Review comment by @aldanor on `crates/uv-cache-info/src/glob.rs`:11 on 2025-05-15 19:09_

Done.

---

_Review comment by @aldanor on `crates/uv-cache-info/src/glob.rs`:14 on 2025-05-15 19:14_

Good point - and hence the `_like` suffix in the function name. Makes sense to add more explicit wording around it.

---

_@aldanor reviewed on 2025-05-15 19:14_

---

_Comment by @konstin on 2025-05-15 19:23_

The PR looks great algorithmically, but I'm wondering if we can have less of that complexity in uv itself? We would be adding a custom trie having to maintain and explain it for something that's not a core algorithm, are there maybe off the shelve components we could use? Could these optimizations be implemented in one of the ecosystem globbing/walkdir crates which use?

Can you describe how the real-world project looks like that motivated these changes, and how uv is currently failing on it?

---

_Comment by @aldanor on 2025-05-15 20:26_

@konstin 
> Can you describe how the real-world project looks like that motivated these changes, and how uv is currently failing on it?

As mentioned above, we're currently facing a pretty heavy blocker because of the lack of relative/parent glob support in a big production codebase, where we have lots of pyo3/maturin python packages that depend on both pure-rust crates and/or static content that may affect the builds. In order to track freshness, we'd like to use relative globs like
```toml
[tool.uv]
cache-keys = [
    { file = "../../crates/foo/**" },
    { file = "../some/files/**.txt" },
]
```
which uv currently doesn't support because of the way globwalk is being used.

> I'm wondering if we can have less of that complexity in uv itself? We would be adding a custom trie having to maintain and explain it for something that's not a core algorithm, are there maybe off the shelve components we could use? Could these optimizations be implemented in one of the ecosystem globbing/walkdir crates which use?

I couldn't find any glob-specific LCPs/tries... there's some generic trie crates but that sounds like an overkill and they provide too much functionality for what's needed here (also LCP grouping here is a bit special to globs - i.e. handling normal/non-normal edges differently).

Technically, it could be either
- slammed into the globwalk crate itself, although it wouldn't be my preference since (a) it would change its core logic in an incompatible way and/or complicate its api and (b) it's marked by author as 'in perpetual maintenance, use other crates'.
- made into a globwalk2 crate with default support for trie-based glob clustering, base dir inference, relative/absolute globs, max depth limiting, skipping irrelevant dirs, etc... although then it would have to copy/paste lots of code from walkdir/ignore to be self-sufficient.
- a separate crate that takes (base, patterns) and returns vec<(base, patterns)>.
- integrated into `ignore` crate?.. but it's used by ripgrep, plus globs outside of base dirs don't really match what it's doing.


I guess one of the latter is doable and I can take on that but is quite a lot more work, and if you have a strong preference to not have this code inside uv, wonder if it makes sense to get this merged into uv first to unblock and pull it out next? Btw globwalk is currently only being used in uv-workspace and uv-cache-info.

---

_Comment by @aldanor on 2025-05-27 15:29_

Hey folks - I went with the 3rd option above and took some time to clean it up, add tests, fix edge cases and make it into a standalone crate (which may perhaps be useful to the general public since it fixes most reported globwalk issues, errors and performance problems).

Would that be acceptable here? (cc @konstin) If yes, I can open a separate PR replacing globwalk. Feature-wise, I think it covers all the bases except ignore files (e.g. .gitignore), is it needed as of right now? (in which case I can try to quickly add it as well)

@BurntSushi would really appreciate if you could take a brief look, because it basically joins two crates of your authorship, in a somewhat obscure way 🙂 https://github.com/aldanor/multiglob/pull/1

---

_Assigned to @konstin by @zanieb on 2025-05-28 18:32_

---

_Comment by @konstin on 2025-06-02 11:07_

My main concern is that we want the same glob implementation across uv; this is already non-ideal as we're using both `glob` and `globset`, but we shouldn't add yet another implementation, rather reduce the differences. Specifically, have you seen the `uv-globfilter` crate? It implements optimizations very similar to the ones you describe.

I'm not sure if we want parent references and absolute paths for globs. Projects are usually intended to be self-contained so they can be built into source dists and wheels, and this breaks this isolation.

For Rust projects specifically, have you seen https://github.com/PyO3/maturin-import-hook?

> As mentioned above, we're currently facing a pretty heavy blocker because of the lack of relative/parent glob support in a big production codebase, where we have lots of pyo3/maturin python packages that depend on both pure-rust crates and/or static content that may affect the builds. In order to track freshness, we'd like to use relative globs like
> 
> ```toml
> [tool.uv]
> cache-keys = [
>     { file = "../../crates/foo/**" },
>     { file = "../some/files/**.txt" },
> ]
> ```
> 
> which uv currently doesn't support because of the way globwalk is being used.

Is the blocker specifically that the lack relative references requires manual rebuilds, or is there also a performance problem?

---

_Comment by @aldanor on 2025-06-02 23:01_

@konstin Let me try to clarify some of the points:
- > Is the blocker specifically that the lack relative references requires manual rebuilds, or is there also a performance problem?

  Primarily the lack of relative references. In large codebases I think disallowing relative references in cache keys is a restriction way too harsh.

  Performance-wise - it's just if you allow relative/absolute references and you have multiple base folders, so if you have 10 glob patterns and if you don't want to kick off 10 full glob walks, you might have to think of how to do it efficiently, see similar problems in e.g. https://github.com/Gilnaa/globwalk/issues/29, https://github.com/Gilnaa/globwalk/pull/31, etc). These problems are addressed in uv-globfilter but will pop back up if you allow multiple bases / rel paths.

- > Specifically, have you seen the uv-globfilter crate? It implements optimizations very similar to the ones you describe.

  Yea, I've seen it; it's somewhat close but would not work out of the box with relative paths because again, essentially there will be many overlapping bases.

- > For Rust projects specifically, have you seen https://github.com/PyO3/maturin-import-hook?

  Yea I'm aware of that, but doesn't quite suit our use case (> 100 crates, many dozen pyo3 projects, cross compilation required, not for 'local development'). We are looking for a clean way for uv to figure when particular packages need to get rebuilt if their dependencies changes (whether rust dependencies of pyo3 crates like `../dep-1/**` or static files like protobuf schemas).

- > as we're using both glob and globset
- > I'm wondering if we can have less of that complexity in uv itself

  The initial reason I spent considerable amount of time pulling it into a separate crate (multiglob) was you suggesting uv shouldn't have this complexity internally :) Given that there's no working options supporting relpaths neither in uv nor in crates.io, I went with a new standalone crate which should now be pretty well tested; it's based on walkdir+globset, somewhat similar to uv-globfilter (but can be further aligned to uv-globfilter behaviour if need be, if there's any particular points).

- > this is already non-ideal as we're using both glob and globset, but we shouldn't add yet another implementation, rather reduce the differences

  The suggestion here is to remove one dependency (globwalk) and replace it with another (multiglob) which is also based on walkdir+globset.

Thanks!

---

_Review comment by @BurntSushi on `crates/uv-cache-info/src/glob.rs`:72 on 2025-06-03 12:26_

It looks like this `Trie` implementation makes pretty heavy use of recursion. And the call depth is in turn proportional to the length of data controlled by end users. That's generally something I think we should try to avoid. Although, in this case, paths are likely limited in length in most cases to a point where the call depth isn't going to get deep enough to overflow the stack. But I'm a little uneasy about relying on such things since that's more of a platform restriction as opposed to something we check and enforce ourselves.

---

_Review comment by @BurntSushi on `crates/uv-cache-info/src/glob.rs`:317 on 2025-06-03 12:29_

Can you add tests with [Windows path prefixes](https://doc.rust-lang.org/std/path/enum.Component.html#variant.Prefix)?

---

_@BurntSushi reviewed on 2025-06-03 12:29_

---

_Review comment by @aldanor on `crates/uv-cache-info/src/glob.rs`:72 on 2025-06-04 20:12_

https://github.com/aldanor/multiglob/pull/1/commits/bbd43be1c63e7c31d78dbad49b01b11d415a9b7c

(fixed in that [PR](https://github.com/aldanor/multiglob/pull/1) because there's some edge cases fixed there and lots of tests, including multi-platform ones)

// Apologies for all the confusion, I decided to pull it out into a standalone crate after @konstin asked if we can have less complexity in uv itself; but this also allowed to thoroughly go through all the multi-platform edge cases and write tons of tests. If uv requires this crate under its control I'm ok with passing ownership or whatever else you guys want (although it may be pretty useful for general public).

---

_@aldanor reviewed on 2025-06-04 20:12_

---

_Review comment by @aldanor on `crates/uv-cache-info/src/glob.rs`:317 on 2025-06-04 20:14_

There were already windows path prefix tests for pretty much all functions except this one in the other [PR](https://github.com/aldanor/multiglob/pull/1), but added them for this test [as well](https://github.com/aldanor/multiglob/pull/1/commits/d2667ea8cbf2876141c8fe5a57412ad01a42e470).

---

_@aldanor reviewed on 2025-06-04 20:14_

---

_@BurntSushi reviewed on 2025-06-05 12:30_

---

_Review comment by @BurntSushi on `crates/uv-cache-info/src/glob.rs`:72 on 2025-06-05 12:30_

Personally, I'd rather the small targeted change you have in this PR. Maintaining a whole other crate for this is a lot to ask.

I'm not keen on depending on a crate made _just_ so that the code doesn't live in uv proper. Its long term maintenance story isn't clear to me. I can't speak for @konstin, but "off the shelf components" to me means _existing_ components. Sorry about the confusion here.

---

_@konstin reviewed on 2025-06-05 12:40_

---

_Review comment by @konstin on `crates/uv-cache-info/src/glob.rs`:72 on 2025-06-05 12:40_

I'm different to @BurntSushi here for maintainability, he has more experience with glob implementations than me

---

_@BurntSushi reviewed on 2025-07-10 22:18_

---

_Review comment by @BurntSushi on `crates/uv-cache-info/src/glob.rs`:317 on 2025-07-10 22:18_

Can you add them in this PR? I'd rather own this very small amount of logic than depend on an entirely new crate personally.

---

_Comment by @BurntSushi on 2025-07-10 22:39_

I'm personally in favor of bringing this in without adding a dependency on a new crate. I think I grok the main logic here decently enough and the trie here looks pretty simple. Are you okay with that @konstin?

---

_Comment by @BurntSushi on 2025-07-10 23:59_

@aldanor Is there anything that your `multiglob` PR does that isn't addressed by the smaller code change here?

---

_Comment by @aldanor on 2025-07-11 00:13_

> @aldanor Is there anything that your `multiglob` PR does that isn't addressed by the smaller code change here?

@BurntSushi I think it would be pretty close to being identical, except in multiglob there's no recursion (that you mentioned above in a comment), a few parts are cleaned up and lots more tests for the whole thing. But functionally, from uv's standpoint, there's not going to be much of a difference I guess. There may appear to be lots of code in multiglob PR because it has to replicate all the outer walkdir API plus there's tests boilerplate.

If this PR is looking more or less ok, my personal suggestion would be to:
- try to get this pr over the line (unless there's any other feedback?), it's more self-contained
- I should probably just publish multiglob separately, may be useful to other people in the ecosystem? (as an alternative to globwalk)
- if you guys ever feel like switching off from globwalk that would be always doable

---

_@aldanor reviewed on 2025-07-11 00:13_

---

_Review comment by @aldanor on `crates/uv-cache-info/src/glob.rs`:317 on 2025-07-11 00:13_

Yea, added.

---

_@BurntSushi approved on 2025-07-11 14:54_

I think this LGTM. I feel comfortable that if we find a bug in this code, I could dive into it and fix it without too much trouble.

---

_Merged by @zanieb on 2025-07-11 15:01_

---

_Closed by @zanieb on 2025-07-11 15:01_

---

_Comment by @aldanor on 2025-07-11 15:35_

Thanks @BurntSushi and @konstin for your time reviewing and getting this through, really appreciated!

---
