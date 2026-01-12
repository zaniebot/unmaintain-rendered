```yaml
number: 5120
title: Open cache files in parallel
type: pull_request
state: merged
author: Thomasdezeeuw
labels: []
assignees: []
merged: true
base: main
head: thomas/open_cache_in_parallel
created_at: 2023-06-15T13:35:49Z
updated_at: 2023-06-20T15:43:11Z
url: https://github.com/astral-sh/ruff/pull/5120
synced_at: 2026-01-12T03:43:30Z
```

# Open cache files in parallel

---

_Pull request opened by @Thomasdezeeuw on 2023-06-15 13:35_

## Summary

This is based on https://github.com/astral-sh/ruff/pull/5117.

Open cache files in parallel (again).

## Test Plan

Existing tests should keep working.

---

_Review requested from @charliermarsh by @Thomasdezeeuw on 2023-06-15 13:35_

---

_Review requested from @konstin by @Thomasdezeeuw on 2023-06-15 13:35_

---

_Comment by @github-actions[bot] on 2023-06-15 13:49_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.1±0.30ms     5.8 MB/sec    1.01      7.1±0.29ms     5.7 MB/sec
formatter/numpy/ctypeslib.py               1.01  1499.9±79.92µs    11.1 MB/sec    1.00  1484.0±67.92µs    11.2 MB/sec
formatter/numpy/globals.py                 1.00   149.8±10.72µs    19.7 MB/sec    1.04   155.9±15.25µs    18.9 MB/sec
formatter/pydantic/types.py                1.00      3.0±0.15ms     8.6 MB/sec    1.02      3.0±0.19ms     8.4 MB/sec
linter/all-rules/large/dataset.py          1.01     14.7±0.48ms     2.8 MB/sec    1.00     14.6±0.50ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.14ms     4.5 MB/sec    1.01      3.7±0.21ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.02   497.9±30.36µs     5.9 MB/sec    1.00   487.2±22.89µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.25ms     3.9 MB/sec    1.01      6.6±0.23ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.4±0.31ms     5.5 MB/sec    1.01      7.5±0.44ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1639.6±84.02µs    10.2 MB/sec    1.00  1608.2±90.52µs    10.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   190.9±10.24µs    15.5 MB/sec    1.02   194.6±14.40µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.4±0.13ms     7.5 MB/sec    1.00      3.4±0.15ms     7.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.13     11.5±0.36ms     3.6 MB/sec    1.00     10.2±0.32ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.16      2.3±0.09ms     7.1 MB/sec    1.00      2.0±0.07ms     8.3 MB/sec
formatter/numpy/globals.py                 1.14   238.8±10.23µs    12.4 MB/sec    1.00   209.8±11.26µs    14.1 MB/sec
formatter/pydantic/types.py                1.16      4.9±0.18ms     5.2 MB/sec    1.00      4.3±0.18ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.04     19.8±0.61ms     2.1 MB/sec    1.00     19.1±0.71ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.2±0.13ms     3.2 MB/sec    1.04      5.4±0.26ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   666.7±27.83µs     4.4 MB/sec    1.01   676.0±58.82µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.28ms     2.9 MB/sec    1.03      9.2±0.28ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.5±0.36ms     3.9 MB/sec    1.00     10.6±0.41ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03      2.3±0.10ms     7.3 MB/sec    1.00      2.2±0.06ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.04   276.8±11.48µs    10.7 MB/sec    1.00   264.9±21.50µs    11.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.9±0.18ms     5.2 MB/sec    1.00      4.8±0.22ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @Thomasdezeeuw on 2023-06-19 09:26_

---

_Marked ready for review by @Thomasdezeeuw on 2023-06-19 14:26_

---

_Comment by @MichaReiser on 2023-06-19 15:28_

Do I understand this change correctly that it mainly opens the cache "lazily" and it's our parallel traversal of the files that make the cache loading *parallel*?

---

_Comment by @Thomasdezeeuw on 2023-06-19 15:53_

> Do I understand this change correctly that it mainly opens the cache "lazily" and it's our parallel traversal of the files that make the cache loading _parallel_?

Initially this was true. But with the last commit I've simplified this because I didn't really like the code. Now the caches are opened in parallel, but no longer lazily. This allows us to avoid atomic/locks in reading from the cache (to synchronize the reading of the cache).

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:318 on 2023-06-20 06:58_

Have you considered using https://crates.io/crates/tempdir to ensure we always clean up the temp directory, including when the test succeeds?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:360 on 2023-06-20 06:59_

This is a fairly heavy integration test that can break for many reasons. Would it make sense to:
* use a smaller fixture directory
* Test the cache in isolation rather than depending on `lint_path` (calling the cache API directly)

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:352 on 2023-06-20 07:00_

Could we store the values of saved files in the cache and use it in the assertion on line 357?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:426 on 2023-06-20 07:03_

Nit: It could make sense to move some of the shared setup code into a helper function that returns the created `Cache` or a wrapper object. This can help with readability

Example: 

https://github.com/astral-sh/ruff/blob/4cc3cdba16846955a1b7a9f4abf7f83fddd26359/crates/ruff_python_formatter/src/comments/mod.rs#L434-L438

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:447 on 2023-06-20 07:05_

Can we set the last modification time manually instead of falling back to a sleep here?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:26 on 2023-06-20 07:07_

Nit: Would it make sense to create newtype wrappers instead that implement `Deref` to enforce the difference statically?

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:31 on 2023-06-20 07:07_

```suggestion
/// package. The on-disk representation is represented in [`PackageCache`] (and
```

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:44 on 2023-06-20 07:08_

```suggestion
    /// source files that still exist.
```

What's the meaning of *still exist*? That have not been deleted or that have been seen?

---

_@MichaReiser approved on 2023-06-20 07:13_

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:26 on 2023-06-20 08:27_

We could I'm not sure it it's worth the maintenance burden though.

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:44 on 2023-06-20 08:32_

I've improved the description, let me know if the new one is clear.

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:318 on 2023-06-20 08:33_

I usually try to minimize the dependencies used, but I'm perfectly happy switching to it.

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:360 on 2023-06-20 08:37_

I understand this test is a bit heavy compared to some of the other tests in the ruff_cli crate, but I would be hesitant to make it any weaker.  This is not only testing the caching stuff it's also testing the recreation of the diagnostics, which will be more complex if we switch to storing only a partial source for example.

So I rather keep this integration test, but perhaps we can move it somewhere else (which would mean we need to make some of these functions public).

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:352 on 2023-06-20 09:47_

I've added commit 42f2bf17421d0086bc86e84c7c0ec562b232ce15 that checks the cached files a little more strictly, though it's a little brittle around dealing with parse errors (which we don't cache).

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:426 on 2023-06-20 09:52_

Agreed, I didn't bother with it because it's just four calls to `Cache::open`. Do you mind if I leave it for now?

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:447 on 2023-06-20 09:54_

It's not about setting the last modified time. For some reason that tmpfs on my system didn't sync/write/whatever the timestamp correctly. So when the loop below started and the file was rewritten (the "File change" test case below) the timestamp was the same as in the cache (and thus the cache was reused causing an error). I didn't really bother investigating it more than that though.

---

_@Thomasdezeeuw reviewed on 2023-06-20 09:54_

---

_Comment by @Thomasdezeeuw on 2023-06-20 10:18_

I'm getting an error on the CI that the `jupyter/R.ipynb` is missing from the cache, but I can't reproduce it locally...

---

_Review comment by @MichaReiser on `crates/ruff_cli/src/cache.rs`:372 on 2023-06-20 12:45_

Nit. Can we use `assert_neq!(paths, [])`. Same for asserting the diagnostics, or the length

---

_@MichaReiser reviewed on 2023-06-20 12:45_

---

_@Thomasdezeeuw reviewed on 2023-06-20 14:03_

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:372 on 2023-06-20 14:03_

Not sure if they're is much added value, but I've made the change.

---

_Review comment by @Thomasdezeeuw on `crates/ruff_cli/src/cache.rs`:447 on 2023-06-20 14:36_

I've ended up removing this code.

---

_@Thomasdezeeuw reviewed on 2023-06-20 14:36_

---

_Closed by @konstin on 2023-06-20 14:43_

---

_Reopened by @konstin on 2023-06-20 14:43_

---

_Review request for @konstin removed by @konstin on 2023-06-20 15:21_

---

_Merged by @Thomasdezeeuw on 2023-06-20 15:43_

---

_Closed by @Thomasdezeeuw on 2023-06-20 15:43_

---

_Branch deleted on 2023-06-20 15:43_

---
