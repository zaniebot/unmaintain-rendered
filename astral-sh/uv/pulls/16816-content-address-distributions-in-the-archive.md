```yaml
number: 16816
title: Content-address distributions in the archive
type: pull_request
state: open
author: charliermarsh
labels:
  - enhancement
  - performance
assignees: []
base: main
head: charlie/content
created_at: 2025-11-22T03:05:15Z
updated_at: 2025-12-02T12:41:26Z
url: https://github.com/astral-sh/uv/pull/16816
synced_at: 2026-01-12T16:12:27Z
```

# Content-address distributions in the archive

---

_@charliermarsh_

## Summary

The idea here is to _always_ compute at least a SHA256 hash for all wheels, then store unzipped wheels in a content-address location in the archive directory. This will help with disk space (since we'll avoid storing multiple copies of the same wheel contents) and cache reuse, since we can now reuse unzipped distributions from `uv pip install` in `uv sync` commands (which always require hashes _already_).

Closes #1061.

Closes #13995.

Closes #16786.


---

_Comment by @charliermarsh on 2025-11-22 03:55_

Still a few things I want to improve here.

---

_@charliermarsh reviewed on 2025-11-22 14:38_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:1086 on 2025-11-22 14:38_

This change I am a little worried about, because it will be a regression to move away from our parallel synchronous zip reader (https://github.com/GoogleChrome/ripunzip) to streaming. On the other hand, it means we'll no longer have two zip implementations.

---

_Review requested from @zanieb by @charliermarsh on 2025-11-22 14:45_

---

_Review requested from @konstin by @charliermarsh on 2025-11-22 14:45_

---

_Marked ready for review by @charliermarsh on 2025-11-22 14:46_

---

_@charliermarsh reviewed on 2025-11-22 15:09_

---

_Review comment by @charliermarsh on `docs/reference/troubleshooting/build-failures.md`:59 on 2025-11-22 15:09_

Another risk here is that this is significantly longer which hurts path length.

---

_@charliermarsh reviewed on 2025-11-22 15:11_

---

_Review comment by @charliermarsh on `docs/reference/troubleshooting/build-failures.md`:59 on 2025-11-22 15:11_

We could `base64.urlsafe_b64encode` it which would be ~43 characters (less than the 64 here, but more than the 21 we used before).

---

_@zanieb reviewed on 2025-11-22 17:10_

---

_Review comment by @zanieb on `docs/reference/troubleshooting/build-failures.md`:59 on 2025-11-22 17:10_

A few ideas...

1. base64 encoding seems reasonable
2. we might want to store it as `{:2}/{2:}`? git and npm do this to shard directories. I guess we don't have that problem today but if we're changing it maybe we should consider it? It looks like you did in https://github.com/astral-sh/uv/pull/16816/commits/3bf79e2adab015b9fd81b148b03d0c72064f9576 ?
3. We could do a truncated hash with a package id for collisions? `{:8}/{package-id}` (I guess the `package-id` could come first?). We'd could persist the full hash to a file for a safety check too.

---

_@charliermarsh reviewed on 2025-11-22 17:31_

---

_Review comment by @charliermarsh on `docs/reference/troubleshooting/build-failures.md`:59 on 2025-11-22 17:31_

Yes. I did it as `{:2}/{2:4}/{4:}` in an earlier commit then rolled it back because it makes various things more complicated (e.g., for cache prune we have to figure out if we can prune the directories recursively). I can re-add it if it seems compelling.


---

_Review comment by @charliermarsh on `docs/reference/troubleshooting/build-failures.md`:59 on 2025-11-22 17:33_

> We could do a truncated hash with a package id for collisions?

I'd prefer not to couple the content-addressed storage to a concept like "package names" if possible. It's meant to be more general (e.g., we also use it for cached environments).

---

_@charliermarsh reviewed on 2025-11-22 17:33_

---

_Review comment by @charliermarsh on `docs/reference/troubleshooting/build-failures.md`:59 on 2025-11-22 17:34_

(`{:2}/{2:4}/{4:}` is what PyPI uses; it looks like pip does `{:2}/{2:4}/{4:6}/{6:}`?)

---

_@charliermarsh reviewed on 2025-11-22 17:34_

---

_@zanieb reviewed on 2025-11-22 17:40_

---

_Review comment by @zanieb on `docs/reference/troubleshooting/build-failures.md`:59 on 2025-11-22 17:40_

> then rolled it back because it makes various things more complicated 

Fair enough. I think people do it to avoid directory size limits (i.e., the number of items allowed in a single directory). I think we'd have had this problem already though if it was a concern for us? It seems fairly trivial to check both locations in the future if we determine we need it.

> I'd prefer not to couple the content-addressed storage to a concept like "package names" if possible.

I think the idea that there's a "disambiguating" component for collisions if we truncate the hash doesn't need to be tied to "package names" specifically. The most generic way to do it would be to have `/0`, `/1`, ... directories with `/{id}/HASH` files and iterate over them? I sort of don't like that though :)

It's broadly unclear to me how much engineering we should do to avoid a long path length.

---

_@charliermarsh reviewed on 2025-11-22 17:49_

---

_Review comment by @charliermarsh on `docs/reference/troubleshooting/build-failures.md`:59 on 2025-11-22 17:49_

It may not really matter. I can't remember the specifics but what ends up happening here is: we create a temp dir, unzip it, then we move the temp dir into this location and hardlink from this location. So I don't think we end up referencing paths within these archives?

---

_@charliermarsh reviewed on 2025-11-22 19:47_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:1086 on 2025-11-22 19:47_

Unfortunately I probably need to benchmark this.

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:1086 on 2025-11-22 20:08_

A huge performance degradation for large files (300ms to 1.3s):
```
unzip_sync_small        time:   [5.4237 ms 5.5621 ms 5.7425 ms]
                        change: [-13.499% -7.9934% -2.6150%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 6 outliers among 100 measurements (6.00%)
  1 (1.00%) high mild
  5 (5.00%) high severe

unzip_sync_medium       time:   [12.587 ms 13.109 ms 13.788 ms]
                        change: [+3.3102% +8.3958% +15.174%] (p = 0.00 < 0.05)
                        Performance has regressed.
Found 8 outliers among 100 measurements (8.00%)
  2 (2.00%) high mild
  6 (6.00%) high severe

Benchmarking unzip_sync_large: Warming up for 3.0000 s
Warning: Unable to complete 100 samples in 5.0s. You may wish to increase target time to 34.9s, or reduce sample count to 10.
unzip_sync_large        time:   [328.01 ms 331.20 ms 334.39 ms]
                        change: [-2.9970% +0.4267% +3.5014%] (p = 0.80 > 0.05)
                        No change in performance detected.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high mild

unzip_stream_small      time:   [5.5436 ms 5.6239 ms 5.7148 ms]
                        change: [-9.3797% -6.8046% -4.0946%] (p = 0.00 < 0.05)
                        Performance has improved.
Found 2 outliers among 100 measurements (2.00%)
  1 (1.00%) high mild
  1 (1.00%) high severe

unzip_stream_medium     time:   [16.696 ms 17.381 ms 18.161 ms]
                        change: [+16.309% +21.820% +27.116%] (p = 0.00 < 0.05)
                        Performance has regressed.
Found 14 outliers among 100 measurements (14.00%)
  4 (4.00%) high mild
  10 (10.00%) high severe

Benchmarking unzip_stream_large: Warming up for 3.0000 s
Warning: Unable to complete 100 samples in 5.0s. You may wish to increase target time to 118.7s, or reduce sample count to 10.
unzip_stream_large      time:   [1.2877 s 1.3015 s 1.3146 s]
                        change: [+12.395% +14.194% +15.823%] (p = 0.00 < 0.05)
                        Performance has regressed.
```

---

_@charliermarsh reviewed on 2025-11-22 20:08_

---

_@charliermarsh reviewed on 2025-11-22 20:17_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:1086 on 2025-11-22 20:17_

What we could do instead is: keep our parallel unzip, then use blake3's parallelized mmap hash for files that we have on-disk (at least for wheels build ourselves, since we never validate hashes for those).

---

_@charliermarsh reviewed on 2025-11-22 20:23_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:1086 on 2025-11-22 20:23_

I guess this wouldn't work for path-based wheels that are provided in `uv add` though. (Although in that case, we already do hash them for `uv add` even if not in `uv pip install`, so the regression only affects `uv pip`.)

---

_@charliermarsh reviewed on 2025-11-22 20:24_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:1086 on 2025-11-22 20:24_

We could consider always computing the blake3 instead of always computing the sha256 (so, compute _both_ the blake3 and the sha256 if the sha256 is needed).

---

_Review comment by @zanieb on `crates/uv-distribution/src/distribution_database.rs`:1086 on 2025-11-22 22:23_

Ah that's pretty unfortunate. I'm curious about content addressing by blake3 and just computing the sha256 where needed, that idea sounds compelling.

---

_@zanieb reviewed on 2025-11-22 22:23_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:1086 on 2025-11-24 00:31_

We'd still have to compute the SHA256 for any local wheels used in `uv add` or similar (unless we changed the lockfile to also use Blake3, which could be a good idea?). The only benefit would be for wheels we build ourselves (since we'd no longer need to hash those).


---

_@charliermarsh reviewed on 2025-11-24 00:31_

---

_@konstin reviewed on 2025-11-28 09:48_

---

_Review comment by @konstin on `crates/uv-distribution/src/distribution_database.rs`:1086 on 2025-11-28 09:48_

Trying to understand the impact of this regression vs the gains of content-addressable caching and one unzip implementation less, there are three cases where we have local wheels:

* A wheel comes from a build: The build an building the wheel should already be much slower than our hashing and unpacking.
* A local path dependency as an index override: There is a regression, but it only applies to one or few wheels. If it's the torch wheel, it becomes noticeable.
* Vendoring with a large find-links directory: This is the case where we lose most compared to parallel unzipping.

---

_@charliermarsh reviewed on 2025-11-28 13:28_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:1086 on 2025-11-28 13:28_

I think this is correct. However, the latter two only apply to `uv pip`, since we _already_ pay that cost in `uv sync` et al (to include a hash in the lockfile).

---

_Label `enhancement` added by @konstin on 2025-12-01 08:34_

---

_Label `performance` added by @konstin on 2025-12-01 08:34_

---

_Comment by @zanieb on 2025-12-02 10:02_

How does this relate to https://github.com/astral-sh/uv/issues/888?

---

_Comment by @konstin on 2025-12-02 12:37_

iirc The hash checking ideas of RECORD never materialized, pip doesn't check the RECORD and neither does uv, and there's plans to remove it (https://discuss.python.org/t/discouraging-deprecating-pep-427-style-signatures/94968). The consensus has shifted to using hashes and signature for the entire archive that are presented outside of the archive, on the index page, instead of being shipped with the archive.

---

_@konstin reviewed on 2025-12-02 12:41_

---

_Review comment by @konstin on `crates/uv-distribution/src/distribution_database.rs`:1086 on 2025-12-02 12:41_

With increased focus on this project API, this only being a slowdown for `uv pip` makes the tradeoff sound even better.

---
