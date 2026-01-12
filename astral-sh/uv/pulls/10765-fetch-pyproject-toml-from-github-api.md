```yaml
number: 10765
title: "Fetch `pyproject.toml` from GitHub API"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
  - no-build
assignees: []
merged: true
base: main
head: charlie/git-fetch
created_at: 2025-01-20T01:26:16Z
updated_at: 2025-01-20T17:50:41Z
url: https://github.com/astral-sh/uv/pull/10765
synced_at: 2026-01-12T16:09:28Z
```

# Fetch `pyproject.toml` from GitHub API

---

_@charliermarsh_

## Summary

When resolving Git metadata, we may be able to fetch the metadata from GitHub directly in some cases. This is _way_ faster, since we don't need to perform many Git operations and, in particular, don't need to clone the repo.

This only works in the following cases:

- The Git repository is public. Otherwise, I believe you need an access token, which we don't have.
- The `pyproject.toml` has static metadata.
- The `pyproject.toml` has no `tool.uv.sources`. Otherwise, we need to lower them... And, if there are any paths or workspace sources, that requires an install path (i.e., we need the content on-disk).
- The project is in the repo root. If it's in a subdirectory, it could be a workspace member. And if it's a workspace member, there could be sources defined in the workspace root. But we can't know without fetching the workspace root -- and we need the workspace in order to find the root...

Closes #10568.


---

_Review requested from @zanieb by @charliermarsh on 2025-01-20 01:26_

---

_Review requested from @konstin by @charliermarsh on 2025-01-20 01:26_

---

_Label `performance` added by @charliermarsh on 2025-01-20 01:26_

---

_Label `no-build` added by @charliermarsh on 2025-01-20 01:28_

---

_Comment by @zanieb on 2025-01-20 03:07_

Did you do any benches?

---

_Comment by @charliermarsh on 2025-01-20 03:35_

Yeah, I think it's just whatever the cost of cloning the repo is. For example, >20x faster for uv:

```
❯ hyperfine 'echo "uv @ git+https://github.com/astral-sh/uv" | ./target/release/uv pip compile - -n' 'echo "uv @ git+https://github.com/astral-sh/uv" | uv pip compile - -n' --warmup 5 --runs 10
Benchmark 1: echo "uv @ git+https://github.com/astral-sh/uv" | ./target/release/uv pip compile - -n
  Time (mean ± σ):     126.7 ms ±  13.6 ms    [User: 27.9 ms, System: 12.3 ms]
  Range (min … max):   113.7 ms … 162.6 ms    10 runs

Benchmark 2: echo "uv @ git+https://github.com/astral-sh/uv" | uv pip compile - -n
  Time (mean ± σ):      2.892 s ±  0.035 s    [User: 1.220 s, System: 0.389 s]
  Range (min … max):    2.840 s …  2.942 s    10 runs

Summary
  echo "uv @ git+https://github.com/astral-sh/uv" | ./target/release/uv pip compile - -n ran
   22.83 ± 2.46 times faster than echo "uv @ git+https://github.com/astral-sh/uv" | uv pip compile - -n
```

(Python interpreter querying is like 40ms out of that 120ms. The GitHub API call is like 50ms. The GitHub file fetch is like 30ms.)


---

_Review comment by @konstin on `crates/uv-git/src/resolver.rs`:76 on 2025-01-20 08:53_

We should set a good example and add contact information and version too: `uv/{uv_version} (+https://github.com/astral-sh/uv)`

---

_@konstin reviewed on 2025-01-20 08:53_

---

_@konstin reviewed on 2025-01-20 08:54_

---

_Review comment by @konstin on `crates/uv-git/src/resolver.rs`:86 on 2025-01-20 08:54_

We need a log statement with the error here

---

_Comment by @konstin on 2025-01-20 08:58_

How do we handle github api limits? If I'm reading the header correctly we get 60 requests per hour. Do we just rely on github to send us 403 when we've exhausted the rate limit?

---

_Comment by @charliermarsh on 2025-01-20 13:28_

Yes. It’s no different than the existing fast-path code we have in the Git crate.

---

_@konstin approved on 2025-01-20 16:17_

---

_@charliermarsh reviewed on 2025-01-20 17:40_

---

_Review comment by @charliermarsh on `crates/uv-git/src/resolver.rs`:76 on 2025-01-20 17:40_

I'll change this separately, since we already do this in `uv-git`.

---

_Merged by @charliermarsh on 2025-01-20 17:50_

---

_Closed by @charliermarsh on 2025-01-20 17:50_

---

_Branch deleted on 2025-01-20 17:50_

---
