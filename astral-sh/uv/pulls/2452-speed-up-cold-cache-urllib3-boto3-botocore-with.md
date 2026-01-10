```yaml
number: 2452
title: Speed up cold cache urllib3/boto3/botocore with batched prefetching
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/fast-boto3
created_at: 2024-03-14T11:01:17Z
updated_at: 2024-04-08T14:28:58Z
url: https://github.com/astral-sh/uv/pull/2452
synced_at: 2026-01-10T14:43:31Z
```

# Speed up cold cache urllib3/boto3/botocore with batched prefetching

---

_Pull request opened by @konstin on 2024-03-14 11:01_

With pubgrub being fast for complex ranges, we can now compute the next n candidates without taking a performance hit. This speeds up cold cache `urllib3<1.25.4` `boto3` from maybe 40s - 50s to ~2s. See docstrings for details on the heuristics.

**Before**

![image](https://github.com/astral-sh/uv/assets/6826232/b7b06950-e45b-4c49-b65e-ae19fe9888cc)

**After**

![image](https://github.com/astral-sh/uv/assets/6826232/1c749248-850e-49c1-9d57-a7d78f87b3aa)

---

We need two parts of the prefetching, first looking for compatible version and then falling back to flat next versions. After we selected a boto3 version, there is only one compatible botocore version remaining, so when won't find other compatible candidates for prefetching. We see this as a pattern where we only prefetch boto3 (stack bars), but not botocore (sequential requests between the stacked bars).

![image](https://github.com/astral-sh/uv/assets/6826232/e5186800-23ac-4ed1-99b9-4d1046fbd03a)

The risk is that we're completely wrong with the guess and cause a lot of useless network requests. I think this is acceptable since this mechanism only triggers when we're already on the bad path and we should simply have fetched all versions after some seconds (assuming a fast index like pypi).

---

It would be even better if the pubgrub state was copy-on-write so we could simulate more progress than we actually have; currently we're guessing what the next version is which could be completely wrong, but i think this is still a valuable heuristic.

Fixes #170.

---

_Label `performance` added by @konstin on 2024-03-14 11:01_

---

_Comment by @albertferras-vrf on 2024-03-21 18:31_

thanks for working on this @konstin , looking forward for this massive speed improvement :) . We're currently waiting for `uv` to be able to resolve boto3 faster before being able to use it

---

_Comment by @charliermarsh on 2024-03-21 18:40_

You might want to consider pinning or bounding boto3 or urllib3, regardless of what you use to resolve / install.

---

_@konstin reviewed on 2024-04-05 14:45_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:225 on 2024-04-05 14:45_

Once again making windows happy

---

_Marked ready for review by @konstin on 2024-04-05 14:49_

---

_Review requested from @charliermarsh by @konstin on 2024-04-05 14:50_

---

_@charliermarsh reviewed on 2024-04-08 13:47_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/batch_prefetch.rs`:97 on 2024-04-08 13:47_

I think we should probably _only_ do this prefetching when the candidate has a wheel. Otherwise, we risk building tons and tons of source distributions.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/batch_prefetch.rs`:112 on 2024-04-08 13:48_

I feel like this risks being quadratic, because we iterate over `total_prefetch`, and then `select_no_preference` is also linear, right? Is there any way to improve that?

---

_@charliermarsh reviewed on 2024-04-08 13:48_

---

_@konstin reviewed on 2024-04-08 13:58_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/batch_prefetch.rs`:112 on 2024-04-08 13:58_

I did an attempt turning `select_candidate` into an iterator but then looked at the profiles for urllib3/boto3/botocore and i think it's not worth it.

---

_@konstin reviewed on 2024-04-08 14:14_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/batch_prefetch.rs`:97 on 2024-04-08 14:14_

Good catch

---

_@charliermarsh approved on 2024-04-08 14:17_

---

_Renamed from "POC: Speed up cold cache boto3/urllib3 with batched prefetching" to "Speed up cold cache urllib3/boto3/botocore with batched prefetching" by @konstin on 2024-04-08 14:27_

---

_Merged by @konstin on 2024-04-08 14:28_

---

_Closed by @konstin on 2024-04-08 14:28_

---

_Branch deleted on 2024-04-08 14:28_

---
