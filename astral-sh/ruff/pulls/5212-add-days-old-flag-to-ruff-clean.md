```yaml
number: 5212
title: Add days-old flag to ruff clean
type: pull_request
state: closed
author: Thomasdezeeuw
labels: []
assignees: []
draft: true
base: thomas/global_cache
head: thomas/clean_days-old_flag
created_at: 2023-06-20T15:45:11Z
updated_at: 2025-02-20T08:59:43Z
url: https://github.com/astral-sh/ruff/pull/5212
synced_at: 2026-01-10T19:57:22Z
```

# Add days-old flag to ruff clean

---

_Pull request opened by @Thomasdezeeuw on 2023-06-20 15:45_

## Summary

Change the cleaning to only include files that are of a certain age.

Based on https://github.com/astral-sh/ruff/pull/5122.

## Test Plan

Manually tested.


---

_Comment by @MichaReiser on 2023-06-20 16:13_

What's the use case for this flag? It seems that building the cache is pretty cheap. A user can, therefore, just clean the whole cache and rebuild it. I think what would be more useful (but probably still too rare to be useful) is to allow cleaning by package. However, that starts exposing implementation details.

---

_Comment by @github-actions[bot] on 2023-06-20 16:16_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.4±0.01ms     6.4 MB/sec    1.00      6.4±0.01ms     6.4 MB/sec
formatter/numpy/ctypeslib.py               1.01   1358.4±1.69µs    12.3 MB/sec    1.00   1340.1±7.19µs    12.4 MB/sec
formatter/numpy/globals.py                 1.01    133.5±0.18µs    22.1 MB/sec    1.00    132.4±0.42µs    22.3 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms     9.7 MB/sec    1.00      2.6±0.01ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.1±0.02ms     3.1 MB/sec    1.00     13.1±0.03ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.00ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    435.2±0.30µs     6.8 MB/sec    1.00    429.3±0.42µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.01ms     6.1 MB/sec    1.00      6.6±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1490.1±7.84µs    11.2 MB/sec    1.00   1472.1±2.21µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.02    172.2±1.88µs    17.1 MB/sec    1.00   169.3±10.51µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.00ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.11      8.6±0.08ms     4.7 MB/sec    1.00      7.8±0.07ms     5.2 MB/sec
formatter/numpy/ctypeslib.py               1.08  1737.9±25.51µs     9.6 MB/sec    1.00  1602.1±21.56µs    10.4 MB/sec
formatter/numpy/globals.py                 1.07    168.5±3.03µs    17.5 MB/sec    1.00    158.0±4.92µs    18.7 MB/sec
formatter/pydantic/types.py                1.09      3.5±0.04ms     7.2 MB/sec    1.00      3.2±0.05ms     7.9 MB/sec
linter/all-rules/large/dataset.py          1.00     15.3±0.24ms     2.7 MB/sec    1.01     15.5±0.18ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.05ms     4.2 MB/sec    1.02      4.0±0.08ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    491.5±5.90µs     6.0 MB/sec    1.01    497.6±8.03µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.7±0.08ms     3.8 MB/sec    1.02      6.9±0.13ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      7.9±0.07ms     5.1 MB/sec    1.02      8.1±0.09ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1691.2±17.47µs     9.8 MB/sec    1.03  1741.8±29.70µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.5±2.91µs    15.3 MB/sec    1.04   200.1±11.16µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.04ms     7.1 MB/sec    1.02      3.6±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @Thomasdezeeuw on 2023-06-21 08:10_

> What's the use case for this flag? It seems that building the cache is pretty cheap. A user can, therefore, just clean the whole cache and rebuild it. I think what would be more useful (but probably still too rare to be useful) is to allow cleaning by package. However, that starts exposing implementation details.

This assumes that we switch to a global cache (https://github.com/astral-sh/ruff/pull/5122), which changes `ruff clean` to remove all caches (for all packages). For users working with multiple packages they might not want to clean the entire cache, only the unused cache file. This is to combat the general problem of the "every growing cache".

---

_Comment by @MichaReiser on 2023-06-21 09:24_

I continue to be undecided whether we should move to a global cache. It seems to introduce more complexity. Anyway:

* I think we should change the documentation of `clean` to make it clear that, by default, removes **all** caches, globally. This should probably go into #5122 
* Would it make sense to instead add a flag (similar to `cargo -p`) to specify which packages to clean? 

> to combat the general problem of the "every growing cache".

My understanding was that your PR adding the *last seen* already removes old files. Does this PR address the case where a cache keeps growing after changing the settings?

---

_Comment by @Thomasdezeeuw on 2023-06-23 09:26_

(I completely missed your comment)

> I continue to be undecided whether we should move to a global cache. It seems to introduce more complexity. Anyway:
> 
>     * I think we should change the documentation of `clean` to make it clear that, by default, removes **all** caches, globally. This should probably go into [Use global cache directory #5122](https://github.com/astral-sh/ruff/pull/5122)

:+1: 

>     * Would it make sense to instead add a flag (similar to `cargo -p`) to specify which packages to clean?
> 
> 
> > to combat the general problem of the "every growing cache".
> 
> My understanding was that your PR adding the _last seen_ already removes old files. Does this PR address the case where a cache keeps growing after changing the settings?

Yes, for caches that are actually used. When/if we switch to a global cache dir removing a Python project will no longer remove Ruff's cache for it (as it's stored outside of the project's directory that is removed). This also true, to a lesser extend, for "packages" within the same repo, e.g. one-off files, examples and often tests. Using the current solver these are stored separately from the "main" source code. For example the FastAPI repo has 79 "packages" and thus 79 different cache files, moving the source file (e.g. an example or test) will cause the cache to be now longer used, but it won't be removed as we simple don't even look at the old cache files again.

A regular `cargo clean` will take of the above, but it will also wipe out still used caches. A `days-old` flag like this adding keep the useful caches, while removing only the unused cache files.

---

_Comment by @Thomasdezeeuw on 2023-07-04 15:17_

Since https://github.com/astral-sh/ruff/pull/5122 wasn't merged this still has some benefit, but less so. If "packages", including tests, docs, scripts, etc., move within a repo this can remove the old cache where the current `clean` command simply removes everything (this assumes a single cache dir is used for the entire repo).

---

_Assigned to @MichaReiser by @MichaReiser on 2023-07-04 15:38_

---

_Comment by @MichaReiser on 2023-07-10 06:47_

> Since #5122 wasn't merged this still has some benefit, but less so. If "packages", including tests, docs, scripts, etc., move within a repo this can remove the old cache where the current `clean` command simply removes everything (this assumes a single cache dir is used for the entire repo).

I'll close this for now based on the decision of #5122. The new flag is less needed now that we keep the local cache and cleaning the whole cache of a single project and rebuilding it should only take a few seconds. Not having the flag keeps Ruff's complexity and API smaller. 



---

_Closed by @MichaReiser on 2023-07-10 06:47_

---

_Branch deleted on 2025-02-20 08:59_

---
