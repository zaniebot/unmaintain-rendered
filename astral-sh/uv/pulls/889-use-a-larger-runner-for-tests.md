```yaml
number: 889
title: Use a larger runner for tests
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/larger-runner
created_at: 2024-01-11T19:18:18Z
updated_at: 2024-01-12T09:17:03Z
url: https://github.com/astral-sh/uv/pull/889
synced_at: 2026-01-12T16:04:15Z
```

# Use a larger runner for tests

---

_@zanieb_

Alternative to #875. Instead of partitioning tests across multiple runners via nextest, we use a larger GitHub Actions runner. Additionally, we explore using nextest to take advantage of the increased number of cores.

On the 8-core machine, nextest is 22% faster than insta. In combination with the vastly more readable output, I think this means we should switch over. As noted in #875 we lose the ability to detect unreferenced snapshot files but since we inline all of our snapshots this shouldn't matter. 

### Benchmarks

The following are the runtime of _just_ the test portion of the test job in GitHub Actions except the partitioned case from #875 which requires a separate build step making runner overhead relevant.

The compile times are noted as a reference as a possible lower bound of test times. The compile time **is included** in all of the test times shown.

Where the nextest thread count is not noted, it is inferred from the CPU count.

```
test                                  time        diff
------------------------------------------------------
2-core (main)                         4m 53s
2-core-nextest-partioned (#875)       3m 56s      -19%
4-core-compilation                       32s      
4-core-insta                          1m 47s      -63%
4-core-nextest                        1m 40s      -66%
8-core-compilation                       18s      
8-core-insta                          1m  9s      -76%
8-core-nextest                        1m  5s      -78%
8-core-nextest-12-threads                54s      -82%
8-core-nextest-16-threads                55s      -82%
```


### Cost

We must pay per-minute costs for these runners:

> Larger runners are not eligible for the use of included minutes on private repositories. For both private and public repositories, when larger runners are in use, they will always be billed at the per-minute rate.
>
> Compared to standard GitHub-hosted runners, larger runners are billed differently. Larger runners are only billed at the per-minute rate for the amount of time workflows are executed on them. 

[[source]](https://docs.github.com/en/actions/using-github-hosted-runners/about-larger-runners/about-larger-runners#understanding-billing)

The per-minute rates are as follows:

> Linux	2	$0.008  (main)
> Linux	4	$0.016
> Linux	8	$0.032  (pull request)

[[source]](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions#per-minute-rates)

The per-minute cost increases by 4x but the workflow is 5.2x faster since we are making use of the extra compute. We will not get any free minutes executing these runners once the repository is public. Additionally, we will not make use of our [3,000 minutes / month](https://docs.github.com/en/billing/managing-billing-for-github-actions/about-billing-for-github-actions#included-storage-and-minutes) of included minutes. Using the 8-core machines, the included 3,000 minutes should account for approximately ~$100.

Here's a brief analysis of costs from the last few
```

Minutes used
------------
November 1090 + 3000 = 4090
December 1357 + 3000 = 4357
January  2655 in 7 days
         ~3x more expected
                     = 11000 estimated

Costs
-----
November  1090 * 0.008   = $ 8.72
December  1357 * 0.008   = $10.86
January   8000 * 0.008   = $64 projected using 3000 included minutes and 2-core machines
          (11000 - (0.82 * 11000)) * 0.032
                         = $63 projected without included minutes and 4-core machines with perf improvement
          (11000 - (0.70 * 11000)) * 0.032
                         = $100 projected with a more conservative 70% reduction in total runtime

```

We can reduce costs (once public) by disabling larger runners for non-organization users e.g. https://github.com/PrefectHQ/prefect/pull/9519 

---

_Renamed from "Use a 4-core larger runner for tests" to "Use a larger runner for tests" by @zanieb on 2024-01-11 19:37_

---

_@zanieb reviewed on 2024-01-11 20:08_

---

_Review comment by @zanieb on `.github/workflows/ci.yaml`:53 on 2024-01-11 20:08_

Before merge, an organization admin (@charliermarsh) needs to change the name of this runner to `ubuntu-latest-large-8` or similar. 

The [macOS runners](https://docs.github.com/en/actions/using-github-hosted-runners/about-larger-runners/about-larger-runners) are named "macos-latest-large" and "macos-latest-xlarge". We could consider just using "ubuntu-latest-large" for consistency in our workflow and change the number of cores in the admin panel as needed. A consistent name would make it really easy for us to have just the OS in the matrix.

---

_Marked ready for review by @zanieb on 2024-01-11 20:14_

---

_Comment by @charliermarsh on 2024-01-11 21:03_

Since this only includes the test runtime, and not the compile time, we wouldn't expect an 82% speed _overall_ right? Just looking at the cost analysis, which seems to assume one. (I still think it's fine.)

---

_@charliermarsh approved on 2024-01-11 21:03_

What would you like the name change to be precisely

---

_Comment by @zanieb on 2024-01-11 21:09_

@charliermarsh sorry that's not clear. All of the test run numbers _include_ the compile time. I only listed the compile time separately to help see how much of the change was related to that. We should expect about an 82% improvement for compilation and testing. If we look at the total runtime of the job it's probably actually more like 70% overall runtime  improvement (since setup isn't significantly faster) which comes out to ~$100 projected for January.

---

_Comment by @zanieb on 2024-01-11 21:11_

`ubuntu-latest-large` please. Then I can match it to the macOS runner if we add tests for macOS.

---

_Comment by @charliermarsh on 2024-01-11 22:06_

I renamed `astral-8-core` to `ubuntu-latest-large`

---

_Comment by @zanieb on 2024-01-11 22:14_

Omg it's so fast. Note for posterity, there are larger runners e.g. 16, 32, and 64 core machines that I didn't bother testing. 1m seems good enough.

---

_Merged by @zanieb on 2024-01-11 22:14_

---

_Closed by @zanieb on 2024-01-11 22:14_

---

_Branch deleted on 2024-01-11 22:14_

---

_Comment by @konstin on 2024-01-12 09:17_

:heart_eyes: 

---
