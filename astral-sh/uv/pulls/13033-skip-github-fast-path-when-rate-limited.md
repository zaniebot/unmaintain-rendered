```yaml
number: 13033
title: Skip GitHub fast path when rate-limited
type: pull_request
state: merged
author: christeefy
labels:
  - enhancement
assignees: []
merged: true
base: main
head: gh-fast-path/429
created_at: 2025-04-21T23:19:11Z
updated_at: 2025-06-24T19:20:48Z
url: https://github.com/astral-sh/uv/pull/13033
synced_at: 2026-01-12T16:10:31Z
```

# Skip GitHub fast path when rate-limited

---

_@christeefy_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Fixes #12761 and #12746.

I'm interpreting the "special handling" required to mean erroring so that we skip the subsequent Git fetch operation in [`.git_metadata`](https://github.com/astral-sh/uv/blob/12bfbed0ecf29e7d11cb200d0d3ede9adc0da98d/crates/uv-distribution/src/source/mod.rs#L1636-L1653) and [`.resolve_revision`](https://github.com/astral-sh/uv/blob/12bfbed0ecf29e7d11cb200d0d3ede9adc0da98d/crates/uv-distribution/src/source/mod.rs#L1878-L1891) of `SourceDistributionBuilder`. Please advise if you meant something else instead.  

Also let me know if you'd like me to generalize this to other HTTP status codes (i.e. 400 and 422, as mentioned in the [comments](https://github.com/astral-sh/uv/blob/12bfbed0ecf29e7d11cb200d0d3ede9adc0da98d/crates/uv-git/src/resolver.rs#L90-L91)).

## Test Plan

<!-- How was it tested? -->
None so far. 

- What's the best way to create new (snapshot?) test cases for this? 
- Should I use `wiremock` to mock a 429 response?

---

_Comment by @christeefy on 2025-05-02 17:49_

Hi @zanieb, I like to follow up on this PR as it has been awaiting review for a while. Do you have any feedback on this work? Let me know if this is still relevant. Thanks!

---

_Comment by @charliermarsh on 2025-05-04 17:05_

I think this might not quite be the right approach. I _think_ what we want is: if we detect a rate limit, skip these "fast-path" attempts in any subsequent operations. So we might need to track some kind of "Are we rate-limited?" global state, and read-from + update it in these locations.

---

_Comment by @zanieb on 2025-05-06 16:04_

Indeed, that's what I had in mind. GitHub also returns a time to wait until attempting another request, so we can stash that in the global state and invalidate it after that much time (though I think in practice, skipping it until the process exits would be fine).

---

_Comment by @christeefy on 2025-05-09 17:07_

Thank you both for the feedback; I understand things better now after some studying.

I'm assuming that we want the global state to apply to everything within `uv-git`, including `GitResolver` and `git.rs` which both have similar `fast-path` logic.

Just to ensure I'm interpreting things correctly, here's the steps I'll implement:
1. Keep track of an "Are we rate-limited?" global state
    - If we get a HTTP-429, mark this state to true
    - The state can revert to false based on a "time to wait" derived from GitHub's response header, and may be a nice-to-have assuming the process runtime is typically much shorter than this "time to wait"
2. Subsequent calls to `github_fast_path` would avoid pinging the GitHub API if/while this global state is true, returning the appropriate value:
    - `GitResolver::github_fast_path` returns `Ok(None)`, leading to a git fetch
    - `git.rs::github_fast_path` returns `FastPathRev::Indeterminate` leading to all branches and tags potentially being fetched in `git.rs::fetch` based on `RefspecStrategy` 🤔 

Please let me know if I've misinterpreted anything here.

Thanks!

---

_Comment by @christeefy on 2025-05-20 14:44_

Hi @zanieb, I've updated the PR based on your feedback. 
Appreciate you taking a look at it!

I found adding integration tests to not be easy. The base urls for the GitHub fast-paths are hardcoded and not exposed via the CLI / env vars, so I don't know how to mock their response via `wiremock`. I can go down the route of exposing them, unless you have better suggestions. 

Thanks!

---

_Comment by @zanieb on 2025-05-20 15:07_

>  The base urls for the GitHub fast-paths are hardcoded and not exposed via the CLI / env vars,

It'd be fine to add this as an environment variable, imo.

---

_Comment by @christeefy on 2025-05-20 15:11_

I can add that env var (tentatively `UV_GITHUB_FAST_PATH_URL`). 
In the meantime, let me know if I'm missing anything major in this PR.

---

_Comment by @christeefy on 2025-05-20 21:08_

I've made the changes and included integration tests. 

I manually did some mutation testing (changing the status code from the mock response, and commenting out this PR's functionalities) to verify the tests aren't dummy tests. 

---

_Review comment by @christeefy on `crates/uv/tests/it/edit.rs`:510 on 2025-05-20 21:09_

`uv` makes 4 attempts (first try + 3 retries) because a HTTP 429 is considered transient in `uv-client`. 

If there are other concurrent fast paths attempt before the first rate-limited request finishes retrying, the skip functionality won't kick in yet. I'm testing the implications of changing the retry semantics for HTTP 429 to `Retryable::Fatal`. 

---

_@christeefy reviewed on 2025-05-20 21:09_

---

_@christeefy reviewed on 2025-05-20 21:18_

---

_Review comment by @christeefy on `crates/uv/tests/it/edit.rs`:510 on 2025-05-20 21:18_

Local test suite is passing, so I can update these tests to expect _zero_ retries.

---

_@zanieb reviewed on 2025-05-21 01:53_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:522 on 2025-05-21 01:53_

I think this makes a 429 fatal everywhere? We probably don't want that.

---

_@christeefy reviewed on 2025-05-21 02:01_

---

_Review comment by @christeefy on `crates/uv-client/src/base_client.rs`:522 on 2025-05-21 02:01_

Sounds good. Reverting.

---

_@christeefy reviewed on 2025-05-21 02:12_

---

_Review comment by @christeefy on `crates/uv/tests/it/edit.rs`:510 on 2025-05-21 02:12_

Full test suite fails, so not going through with that. See comment below as well.

---

_Comment by @christeefy on 2025-05-21 14:28_

Good morning @zanieb, the PR is ready for review. 
Please take a look when you get the chance, thanks 😊

---

_Assigned to @oconnor663 by @zanieb on 2025-06-09 21:23_

---

_Review comment by @oconnor663 on `crates/uv-git/src/rate_limit.rs`:7 on 2025-06-09 23:09_

I don't think `LazyLock` is needed here, since `AtomicBool` can be const-initialized.

---

_Review comment by @oconnor663 on `crates/uv-git/src/git.rs`:820 on 2025-06-09 23:15_

[The GitHub API docs](https://docs.github.com/en/rest/using-the-rest-api/troubleshooting-the-rest-api?apiVersion=2022-11-28#rate-limit-errors) say that both 403 and 429 are possible here. The only reason I went and found that is that I think the 403's I was seeing here are also rate-limit-related: https://github.com/astral-sh/uv/pull/13748#issuecomment-2957204584. Seems like we should handle both?

---

_Review comment by @oconnor663 on `crates/uv-git/src/rate_limit.rs`:20 on 2025-06-09 23:17_

I don't think this matters for performance in practice, but my usual default with atomics is to make them `Relaxed` when they're "just data", as opposed to some sort of synchronization tool that `unsafe` code might interact with. Curious what other folks think.

---

_@oconnor663 requested changes on 2025-06-09 23:41_

How does this PR relate to #12746? (Incidentally that's about to be fixed by #13748.)

---

_@konstin reviewed on 2025-06-10 09:11_

---

_Review comment by @konstin on `crates/uv-git/src/rate_limit.rs`:20 on 2025-06-10 09:11_

I'm no expert on atomics to weigh in here, but we should make it consistent with (which may include changing the latter in an earlier PR) https://github.com/astral-sh/uv/blob/24859bd3eebfe8fe50b187391e72dab91afb7093/crates/uv-warnings/src/lib.rs#L11-L22. We can also use a mutex given that this is not performance critical, though `AtomicBool` is simple enough.

---

_@konstin reviewed on 2025-06-10 09:13_

---

_Review comment by @konstin on `crates/uv-static/src/env_vars.rs`:670 on 2025-06-10 09:13_

This should document what shape of URL is expected, is there something that GitHub considers as API root that users could exchange?

---

_Unassigned @oconnor663 by @oconnor663 on 2025-06-18 15:16_

---

_@christeefy reviewed on 2025-06-21 13:02_

---

_Review comment by @christeefy on `crates/uv-static/src/env_vars.rs`:670 on 2025-06-21 13:02_

This env var's intended use is to mock the GitHub API root during tests. It's not meant for users to override, and is hidden from [user-facing docs](https://docs.astral.sh/uv/reference/environment/) with `#[attr_hidden]`. 

I can rename this to `UV_TEST_GITHUB_FAST_PATH_URL` to be even more explicit, assuming we are okay with `UV_TEST_*` appearing in non-test files.

Please let me know what you think. 

---

_@christeefy reviewed on 2025-06-21 13:07_

---

_Review comment by @christeefy on `crates/uv-git/src/rate_limit.rs`:20 on 2025-06-21 13:07_

I did some [studying](https://www.youtube.com/watch?v=ZQFzMfHIxng) into Atomics and you're right. It's more appropriate to use `Ordering::Relaxed` here since the atomic isn't interacting with other data structures (e.g. as an index to a distributed vec). 

@konstin I will update those in `uv-warning` consistent in a separate PR as that work is distinct from this PR's. 

---

_@christeefy reviewed on 2025-06-21 13:08_

---

_Review comment by @christeefy on `crates/uv-git/src/rate_limit.rs`:7 on 2025-06-21 13:08_

You're right, thanks for the suggestion. I'll remove the `LazyLock`. 

---

_Review comment by @christeefy on `crates/uv-git/src/git.rs`:820 on 2025-06-21 13:10_

Good point! That said, I feel more comfortable relying on `x-ratelimit-remaining` in the response header as 403 could imply failures beyond just rate-limiting. 

~~I'll update the implementation accordingly to improve its clarity.~~

[Update] `x-ratelimit-remaining` applies only to primary rate limits; secondary rate limits do not have such headers. So we have to rely on 403 and 429 as indicators.

---

_@christeefy reviewed on 2025-06-21 13:10_

---

_Review requested from @oconnor663 by @christeefy on 2025-06-21 14:44_

---

_@christeefy reviewed on 2025-06-21 15:16_

---

_Review comment by @christeefy on `crates/uv-git/src/rate_limit.rs`:20 on 2025-06-21 15:16_

#14190 created for this work thread.

---

_@oconnor663 approved on 2025-06-23 17:57_

---

_Assigned to @oconnor663 by @oconnor663 on 2025-06-23 17:57_

---

_Assigned to @konstin by @oconnor663 on 2025-06-23 17:57_

---

_Unassigned @oconnor663 by @oconnor663 on 2025-06-23 17:57_

---

_Review comment by @konstin on `crates/uv-git/src/rate_limit.rs`:18 on 2025-06-24 09:42_

Does this method need to be `pub(crate)`?

---

_Review comment by @konstin on `crates/uv/tests/it/pip_install.rs`:2083 on 2025-06-24 10:01_

For consistency, can we use https://github.com/astral-test/uv-public-pypackage here too?

---

_Review comment by @konstin on `crates/uv/tests/it/pip_install.rs`:2109 on 2025-06-24 10:01_

Nice work on the testing strategy here!


---

_@konstin approved on 2025-06-24 10:07_

Two things that should be easy to fix, otherwise it looks good!

---

_Renamed from "Skip git fetch when rate-limited by GitHub" to "Skip GitHub fast path when rate-limited" by @konstin on 2025-06-24 10:32_

---

_Label `enhancement` added by @konstin on 2025-06-24 10:32_

---

_@christeefy reviewed on 2025-06-24 13:29_

---

_Review comment by @christeefy on `crates/uv-git/src/rate_limit.rs`:18 on 2025-06-24 13:29_

No it doesn't. Updated to just `const`. 

---

_Review requested from @konstin by @christeefy on 2025-06-24 13:29_

---

_Comment by @christeefy on 2025-06-24 13:30_

@konstin changes have been made and CI is passing. Thanks for your quick PR review 😊

---

_Merged by @oconnor663 on 2025-06-24 19:11_

---

_Closed by @oconnor663 on 2025-06-24 19:11_

---

_Branch deleted on 2025-06-24 19:20_

---
