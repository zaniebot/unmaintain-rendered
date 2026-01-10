```yaml
number: 5122
title: Use global cache directory
type: pull_request
state: closed
author: Thomasdezeeuw
labels:
  - breaking
assignees: []
base: main
head: thomas/global_cache
created_at: 2023-06-15T14:31:13Z
updated_at: 2025-02-20T08:59:40Z
url: https://github.com/astral-sh/ruff/pull/5122
synced_at: 2026-01-10T19:57:22Z
```

# Use global cache directory

---

_Pull request opened by @Thomasdezeeuw on 2023-06-15 14:31_

## Summary

This is based on https://github.com/astral-sh/ruff/pull/5117.

Updates #1292.

Falling back to the old .ruff_cache directory in the current directory
if the global cache directory can't be determined.

For reference a number of other Python tools also use global caches (by
default):

 * Black: https://github.com/psf/black/blob/35722dff623f3cdf5018e3f1183cd4e02e91caa8/src/black/cache.py#L21-L34.
 * Pip: https://github.com/pypa/pip/blob/8a1eea4aaedb1fb1c6b4c652cd0c43502f05ff37/src/pip/_internal/locations/base.py#L13, https://github.com/pypa/pip/blob/8a1eea4aaedb1fb1c6b4c652cd0c43502f05ff37/src/pip/_internal/utils/appdirs.py#L16.
 * Pylint: https://github.com/pylint-dev/pylint/blob/507dfc5ebd0cd9712ac4840cae3bea8623206554/doc/faq.rst#where-is-the-persistent-data-stored-to-compare-between-successive-runs.
* Poetry: https://github.com/python-poetry/poetry/blob/6d57e843b4a709f5a5156c9cc5661cdd0f718979/src/poetry/locations.py#LL18C30-L18C30,
* Pycharm: https://intellij-support.jetbrains.com/hc/en-us/articles/206544519-Directories-used-by-the-IDE-to-store-settings-caches-plugins-and-logs.

Python projects that don't:

* CPython uses a local dir (`__pycache__`).
* MyPy uses a local dir: https://github.com/python/mypy/blob/66b96ed54b255495288ba539e3fd1f54f21d4abe/mypy/defaults.py#L17.
* Pytest uses a local dir: https://github.com/pytest-dev/pytest/blob/b55c02c3c9914a48e00fe8ab1cd7d903ab3b08c0/src/_pytest/cacheprovider.py#LL105C21-L105C21.

Other tools:

* Pyright doesn't seem to do any caching on-disk (?) https://github.com/microsoft/pyright/discussions/4809.

## Test Plan

Existing tests should keep working.


---

_Review requested from @charliermarsh by @Thomasdezeeuw on 2023-06-15 14:31_

---

_Review requested from @konstin by @Thomasdezeeuw on 2023-06-15 14:31_

---

_Comment by @github-actions[bot] on 2023-06-15 14:46_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      6.4±0.02ms     6.4 MB/sec    1.02      6.5±0.01ms     6.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1309.8±6.23µs    12.7 MB/sec    1.02   1338.3±2.93µs    12.4 MB/sec
formatter/numpy/globals.py                 1.00    127.6±0.28µs    23.1 MB/sec    1.03    131.2±0.90µs    22.5 MB/sec
formatter/pydantic/types.py                1.00      2.6±0.01ms     9.8 MB/sec    1.01      2.6±0.01ms     9.7 MB/sec
linter/all-rules/large/dataset.py          1.11     15.0±2.04ms     2.7 MB/sec    1.00     13.5±0.08ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.02ms     5.1 MB/sec    1.00      3.3±0.01ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    416.9±0.88µs     7.1 MB/sec    1.01    420.6±0.64µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.05ms     4.3 MB/sec    1.00      5.9±0.04ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.03ms     6.1 MB/sec    1.00      6.6±0.03ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1430.1±3.72µs    11.6 MB/sec    1.00   1436.1±4.77µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    161.9±0.26µs    18.2 MB/sec    1.00    161.6±0.18µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.5 MB/sec    1.01      3.0±0.01ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.7±0.11ms     5.3 MB/sec    1.03      8.0±0.23ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1603.3±26.62µs    10.4 MB/sec    1.02  1632.0±28.39µs    10.2 MB/sec
formatter/numpy/globals.py                 1.00    157.6±3.63µs    18.7 MB/sec    1.04    164.5±7.44µs    17.9 MB/sec
formatter/pydantic/types.py                1.00      3.2±0.05ms     7.9 MB/sec    1.02      3.3±0.10ms     7.8 MB/sec
linter/all-rules/large/dataset.py          1.00     15.9±0.24ms     2.6 MB/sec    1.03     16.4±0.21ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.08ms     4.0 MB/sec    1.05      4.4±0.15ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    506.2±9.03µs     5.8 MB/sec    1.02    517.9±8.60µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.11ms     3.6 MB/sec    1.06      7.5±0.13ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.10ms     5.0 MB/sec    1.05      8.5±0.14ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1771.2±26.41µs     9.4 MB/sec    1.01  1795.7±24.22µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    204.3±4.23µs    14.4 MB/sec    1.02    208.0±4.08µs    14.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.06ms     6.8 MB/sec    1.02      3.8±0.06ms     6.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-06-15 22:24_

---

_Review comment by @charliermarsh on `crates/ruff_cache/Cargo.toml`:19 on 2023-06-15 22:24_

We also use this in the `ruff` crate, can we make this a workspace dependency?

---

_@charliermarsh approved on 2023-06-15 22:26_

---

_Review comment by @konstin on `crates/ruff_cli/src/commands/clean.rs`:26 on 2023-06-16 08:16_

do we have any safeguard that we don't [accidentally remove user data](https://www.theregister.com/2015/01/17/scary_code_of_the_week_steam_cleans_linux_pcs/), e.g. checking for `CACHEDIR.TAG`?

---

_@konstin approved on 2023-06-16 08:17_

---

_Comment by @Thomasdezeeuw on 2023-06-16 12:25_

I forgot to remove the (old) cache initialization: https://github.com/astral-sh/ruff/blob/5526699535dea230f873687494f75bfd288da168/crates/ruff_cli/src/commands/run.rs#L47-L65

---

_Review requested from @MichaReiser by @Thomasdezeeuw on 2023-06-19 09:26_

---

_Comment by @MichaReiser on 2023-06-19 17:15_

Can you add an example on how to use Ruff in CI, e.g. together with the github cache action? Would it make sense to automatically detect CI and then use a different directory?

---

_Marked ready for review by @Thomasdezeeuw on 2023-06-20 12:48_

---

_Comment by @Thomasdezeeuw on 2023-06-20 12:58_

> Can you add an example on how to use Ruff in CI, e.g. together with the github cache action?

When running on the CI you probably want to se the `cache-dir` argument to use a fixed cache.

>  Would it make sense to automatically detect CI and then use a different directory?

I'm not sure if we can consistently detect this, but even if we could than we have the problem what directory to use. I don't think GitHub Actions has a default directory that is always cache, I think you need to explicitly set this.

Maybe an example GitHub Actions config file can tackle both issues.

---

_Comment by @Thomasdezeeuw on 2023-06-28 09:03_

@MichaReiser I believe you were still on the fence on whether or not a global cache directory would be an improvement, have you made a decision on this?

---

_Comment by @MichaReiser on 2023-06-28 12:19_

> @MichaReiser I believe you were still on the fence on whether or not a global cache directory would be an improvement, have you made a decision on this?

Thanks for pinging me. Not really. My concern is that this change introduces additional complexity and potentially breaks the workflows of users and, thus, the cost of the change may be too height.

My concerns are:

* `ruff clean` now needs an additional option to limit the scope to a single project
* A user could unintentionally nuke all caches by simply running `ruff clean` because they assume that the command is scoped to the current project (as are all? other commands). There's no real harm by this, but may be surprising. 
* I would be interested in seeing an example on how to setup the github action (with the cache action) when using the global cache directory (or would we recommend using a local directory?) for a task that runs cross-platform.



---

_Comment by @MichaReiser on 2023-06-28 12:27_

But... this is not a *me* decision. If the team is in favor then go for it.

---

_Comment by @Thomasdezeeuw on 2023-06-28 12:54_

> Thanks for pinging me. Not really. My concern is that this change introduces additional complexity and potentially breaks the workflows of users and, thus, the cost of the change may be too height.
> 
> My concerns are:
> 
>     * `ruff clean` now needs an additional option to limit the scope to a single project
> 
>     * A user could unintentionally nuke all caches by simply running `ruff clean` because they assume that the command is scoped to the current project (as are all? other commands). There's no real harm by this, but may be surprising.

The above two concerns are valid. Don't really have a good suggestion to resolve these. 

>     * I would be interested in seeing an example on how to setup the github action (with the cache action) when using the global cache directory (or would we recommend using a local directory?) for a task that runs cross-platform.

We could recommend to set `--cache-dir` flag and cache that directory in GitHub Action.

> But... this is not a _me_ decision. If the team is in favor then go for it.

Maybe you can discuss this within the team? I think I can still merge this but, I think consensus is important.

---

_Comment by @MichaReiser on 2023-06-28 12:58_

> Maybe you can discuss this within the team? I think I can still merge this but, I think consensus is important.

Will do. I'll bring it up in our next standup.

---

_Comment by @zanieb on 2023-06-28 13:30_

> We could recommend to set --cache-dir flag and cache that directory in GitHub Action.

We should probably just provide a GitHub Action with reasonable defaults anyway.

---

_Assigned to @MichaReiser by @MichaReiser on 2023-07-03 08:30_

---

_Label `breaking` added by @MichaReiser on 2023-07-03 08:33_

---

_Comment by @MichaReiser on 2023-07-04 14:31_

I brought this PR up in today's standup, and we decided not to merge this as of today because moving to a global cache directory only provides limited benefits in our view and comes with a few downsides. This isn't saying that we don't want to move to a global cache directory eventually. It's just that we want to have more time to consider all the tradeoffs to decide this for good. 

Thanks again @Thomasdezeeuw for working on this and all your caching improvements.

---

_Closed by @MichaReiser on 2023-07-04 14:31_

---

_Branch deleted on 2025-02-20 08:59_

---
