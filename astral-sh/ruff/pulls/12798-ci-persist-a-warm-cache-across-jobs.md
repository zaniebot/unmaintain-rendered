```yaml
number: 12798
title: "ci: persist a warm cache across jobs"
type: pull_request
state: closed
author: DonIsaac
labels:
  - ci
assignees: []
base: main
head: don/ci/shared-warm-caching
created_at: 2024-08-11T02:48:49Z
updated_at: 2024-09-03T07:47:11Z
url: https://github.com/astral-sh/ruff/pull/12798
synced_at: 2026-01-10T21:38:32Z
```

# ci: persist a warm cache across jobs

---

_Pull request opened by @DonIsaac on 2024-08-11 02:48_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

I saw on Twitter that you guys want some help with faster CI.

This PR updates `Swatinem/rust-cache@v2` steps for testing jobs and other long-running tasks (except for release builds) to share cached build artifacts across jobs.

Each platform (linux/windows, didn't see tests for MacOS) gets its own warm cache. Caches are read by all branches, but only saved when CI runs on `main`.

## Test Plan

<!-- How was it tested? -->
This is what Oxc's CI does.

---

_Comment by @codspeed-hq[bot] on 2024-08-11 02:54_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/DonIsaac:don/ci/shared-warm-caching)

### Merging #12798 will **not alter performance**

<sub>Comparing <code>DonIsaac:don/ci/shared-warm-caching</code> (c7279a0) with <code>main</code> (e05953a)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-08-11 03:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_google_drive.ipynb:2:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_redshift.ipynb:13:1:1: Got unexpected token `
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_google_drive.ipynb:2:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_redshift.ipynb:13:1:1: Got unexpected token `
```

</p>
</details>




---

_Comment by @MichaReiser on 2024-08-12 06:44_

Thanks for this PR. Could you tell us a bit more on why this is expected to improve CI times other than OXC is using it?

I think I tried something similar in the past without seeing improved build times

---

_@MichaReiser reviewed on 2024-08-12 06:47_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:172 on 2024-08-12 06:47_

I think we should avoid specying the shard key unless there are multiple jobs that use the same cache.

---

_Comment by @DonIsaac on 2024-08-13 03:27_

It has to do with [this line of code](https://github.com/Swatinem/rust-cache/blob/9bdad043e88c75890e36ad3bbc8d27f0090dd609/src/config.ts#L60). Without a shared key, each job has its own cache. Adding a shared key should reduce the number of duplicate caches created by each job. By only saving on jobs that run on main, we should see less cache invalidation. The hypothesis is that each branch syncs with main and is more likely to have code in common with main than other PRs, so the creation of new PRs shouldn't blow the cache.

---

_Review comment by @DonIsaac on `.github/workflows/ci.yaml`:172 on 2024-08-13 03:28_

Do we know if caches can be shared across builds on different architectures?

---

_@DonIsaac reviewed on 2024-08-13 03:28_

---

_@MichaReiser reviewed on 2024-08-13 11:04_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:172 on 2024-08-13 11:04_

I don't think it makes sense to share caches across architectures because the output is different for each architecture. 

---

_Comment by @MichaReiser on 2024-08-13 11:05_

> It has to do with [this line of code](https://github.com/Swatinem/rust-cache/blob/9bdad043e88c75890e36ad3bbc8d27f0090dd609/src/config.ts#L60). Without a shared key, each job has its own cache. Adding a shared key should reduce the number of duplicate caches created by each job. By only saving on jobs that run on main, we should see less cache invalidation. The hypothesis is that each branch syncs with main and is more likely to have code in common with main than other PRs, so the creation of new PRs shouldn't blow the cache.

That makes sense. But doesn't it mean that we only have to specify the shared key for cached that should be re-used across at least two jobs? For example, there's only a single job using `warm-wasm`. Couldn't we use the default cache key instead?

---

_@MichaReiser reviewed on 2024-08-13 11:05_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:142 on 2024-08-13 11:05_

I would prefer some more specific cache key names. For example: `linux-test` or `linux-debug`

---

_@MichaReiser reviewed on 2024-08-13 11:07_

Should we also change the caching of `clipyy`, `check-formatter-instability-and-black-similarity` etc?

---

_Comment by @DonIsaac on 2024-08-13 12:21_

These are good points, I'll remove and add these respectively. 

---

_Comment by @MichaReiser on 2024-08-13 13:09_

> These are good points, I'll remove and add these respectively.

Thanks. Overall this looks reasonable to me. We can merge it after updating all jobs and removing unnecessary shard-keys to see if it has any positive impact.

---

_Label `ci` added by @MichaReiser on 2024-08-13 13:09_

---

_Comment by @zanieb on 2024-08-13 13:46_

There are a few other things to keep in mind

1. Saving the cache takes time, how does this trade-off with the savings of the shared cache?
2. Is the cache entry going to increase in size now? Presumably if more jobs are sharing it with slightly different builds it's additive? How much does it increase by? How much time does it add to download the larger cache?
3. GitHub imposes a cache size limit, once we reach it entries are evicted. Does this approach help with that or not?

---

_Comment by @MichaReiser on 2024-08-13 13:56_

What I understand is that we would have fewer caches because we only store them on main. The cache size should be unchanged because it's still the same caches. Saving fewer caches should give us some time back on non-main runs because we skip the caching step entirely

---

_Comment by @zanieb on 2024-08-13 14:05_

Ah I misunderstood some of the discussion here and thought we were _previously_ only saving the cache changes for `main` (I made this change over in uv at some point), that changes my caveats. This comes with other trade-offs though, for example, if a pull request add dependencies we have to build them on every commit to the pull request instead of populating a cache on the first run. 

Edit: Not sure how I missed this because [this comment](https://github.com/astral-sh/ruff/pull/12798#issuecomment-2285271968)

> By only saving on jobs that run on main, we should see less cache invalidation. The hypothesis is that each branch syncs with main and is more likely to have code in common with main than other PRs, so the creation of new PRs shouldn't blow the cache.

Clearly addresses my note at (3)

---

_Comment by @zanieb on 2024-08-13 14:11_

Regarding

> If a pull request add dependencies we have to build them on every commit to the pull request instead of populating a cache on the first run.

I think this was a real problem in uv and I reverted the change. Maybe Ruff's dependencies are more stable? Maybe we can toggle the key based on changes to the `Cargo.toml`? Seems kind of tricky.


---

_Comment by @MichaReiser on 2024-08-13 15:16_

> I think this was a real problem in uv and I reverted the change. Maybe Ruff's dependencies are more stable? Maybe we can toggle the key based on changes to the Cargo.toml? Seems kind of tricky.

Interesting. Would love to hear more about the specific problem you ran into. What I understand is that the builds for PRs adding dependencies will take longer because they have to build the added/changed dependencies. But I wouldn't expect that they take that much longer and it's mainly renovate that tinkers with dependencies.

---

_Comment by @zanieb on 2024-08-13 15:19_

I wish I knew which pull request it was, but the pipelines have changed a lot over there it seems hard to track down. More of a note than blocking feedback, we can revisit if it causes a specific problem here.

---

_Comment by @DonIsaac on 2024-08-13 19:51_

> > These are good points, I'll remove and add these respectively.
> 
> Thanks. Overall this looks reasonable to me. We can merge it after updating all jobs and removing unnecessary shard-keys to see if it has any positive impact.

`warm-wasm` is used by two jobs, so I'll leave that one. I've removed `warm-windows` and `warm-msrv`.

---

_@MichaReiser requested changes on 2024-08-14 07:09_

I think there must have been some confusion because using `warm` for all jobs and only saving it in one job will give us worse performance for most jobs.

* We should only specify `shared-key` when two or more jobs use the same cache. In our case that could be `cargo-test-linux` and maybe `mkdocs`? 
* Jobs should only use the same `shared-key` if they use the same target. Clippy, Windows, fuzz, test scripts all use different targets as far as I know
* I would prefer to use more specific names than`warm` 
* There are still multiple jobs that don't use the new optimization (benchmarks, `check-formatter-instability-and-black-similarity`, `pre-commit`, `python-package`, `cargo-build-release`, `cargo-test-windows`). Is there a specific reason for it?

---

_Comment by @DonIsaac on 2024-08-14 11:16_

Today is quite busy for me, I'll get back to this tomorrow. 

---

_Comment by @MichaReiser on 2024-09-03 07:47_

I close this for now. Feel free to re-open if you plan to follow up on the comments.

---

_Closed by @MichaReiser on 2024-09-03 07:47_

---
