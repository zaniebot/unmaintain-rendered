```yaml
number: 3480
title: "ci: Benchmark CI Step"
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: ci/benchmark-step
created_at: 2023-03-13T12:51:42Z
updated_at: 2023-03-16T08:05:11Z
url: https://github.com/astral-sh/ruff/pull/3480
synced_at: 2026-01-12T15:55:13Z
```

# ci: Benchmark CI Step

---

_@MichaReiser_

This PR adds a new benchmark step that runs for every pull request and compares the PR performance against main. The action writes a comment (or updates an existing one) with the results. 


This is inspired and partially based on [Rome](https://github.com/rome/tools/)'s and [OXC](https://github.com/Boshen/oxc)'s benchmarking setup that @Boshen created.

## Test Plan

See [this comment](https://github.com/charliermarsh/ruff/pull/3480#issuecomment-1469583939)

---

_Comment by @MichaReiser on 2023-03-14 08:56_

@Boshen It's not fully functional yet nor have I tested the caching but I think you could find some of it interesting ;) 



---

_Comment by @MichaReiser on 2023-03-14 09:57_

Okay, caching and running PR/main on different runners is a bad idea. The runtime difference is too big even if there are no changes.

---

_Marked ready for review by @MichaReiser on 2023-03-14 10:22_

---

_Comment by @sciyoshi on 2023-03-15 01:58_

This is great! Two quick comments since I was working recently on the [ecosystem CI check](https://github.com/charliermarsh/ruff/issues/2677) which also comments on the PR:
- It would probably make sense to combine the commenting piece of this workflow with `ecosystem-comment.yaml` (and probably rename it to e.g. `pr-comment.yaml`). That way there will be a single comment rather than two which would probably make for less noise, and also the separate workflow means that the PR comment can also work for PRs from forks of this repo (which is basically every PR except yours and @charliermarsh's 😄)
- I'm not sure what artifacts `cargo bench` requires, but assuming it only needs the debug binary, you could use the artifacts that are [now uploaded by the `cargo test` step](https://github.com/charliermarsh/ruff/blob/57796c5e59e3e0950d4409d3e54e7ba5a04bdda1/.github/workflows/ci.yaml#L82-L85). That would save on the re-build time (at least for the main branch), and means that the benchmark only runs if the remaining tests pass.

---

_Comment by @Boshen on 2023-03-15 02:40_

Very clever approaches for speeding up CI times! Thank you @MichaReiser, I stole this faster than you could merge this PR ;-)

I wonder if we can also build the binary on main and then just download it on the PR 🤔 

Uploading the binary from the test step doesn't work I think, it it seems to be uploading a debug build? `path: target/debug/ruff`. 

---

_Comment by @MichaReiser on 2023-03-15 06:58_

> This is great! Two quick comments since I was working recently on the https://github.com/charliermarsh/ruff/issues/2677 which also comments on the PR:

Thanks for that work. I used your job as inspiration on how we can improve the benchmarking, and it seems there's more for me to learn :smiley: 

* Comments: That's clever. Let me give this a try. 
* Re-using the binary: The benchmark uses the release build of `ruff_benchmark` which we aren't building as part of the pull_request workflow. However, we could do something good for the environment (it won't speed up the overall time) and build the binary for main once and then re-use when running the baseline benchmarks on PRs. 

> I wonder if we can also build the binary on main and then just download it on the PR thinking

Yes, I'll give that a try

> Very clever approaches for speeding up CI times! Thank you @MichaReiser, I stole this faster than you could merge this PR ;-)

 :laughing:  Thank you! I saw your PR yesterday and was, wow, that was quick. I should update the PR summary to clarify that this workflow is heavily inspired by your work on Rome's and OXC's benchmarks!

---

_Assigned to @MichaReiser by @MichaReiser on 2023-03-15 07:00_

---

_Comment by @github-actions[bot] on 2023-03-15 08:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.00      9.1±0.09ms     4.5 MB/sec    1.01      9.2±0.04ms     4.4 MB/sec
linter/numpy/ctypeslib.py    1.00      3.0±0.04ms   111.8 MB/sec    1.01      3.0±0.00ms   111.2 MB/sec
linter/numpy/globals.py      1.00  1569.6±16.01µs   113.6 MB/sec    1.01   1590.0±4.44µs   112.1 MB/sec
linter/pydantic/types.py     1.00      4.3±0.04ms     6.0 MB/sec    1.02      4.3±0.01ms     5.9 MB/sec
```

#### Windows
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.00      9.2±0.15ms     4.4 MB/sec    1.00      9.2±0.45ms     4.4 MB/sec
linter/numpy/ctypeslib.py    1.00      3.0±0.07ms   113.8 MB/sec    1.00      2.9±0.06ms   114.3 MB/sec
linter/numpy/globals.py      1.00  1548.2±55.86µs   115.1 MB/sec    1.01  1567.2±63.16µs   113.7 MB/sec
linter/pydantic/types.py     1.00      4.3±0.16ms     6.0 MB/sec    1.01      4.4±0.17ms     5.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @MichaReiser on 2023-03-15 09:30_

---

_Comment by @MichaReiser on 2023-03-15 09:31_

I implemented the recommended changes. 

I haven't been able to test the caching because it requires triggering a build on main.... which I can only do after this PR lands. 

EDIT: Caching on main isn't as straightforward as I have thought with our approach where we use `cargo bench`. `cargo bench` doesn't create a single binary but one binary for each benchmark. Each binary has a unique name `linter-<hash>`, that makes it difficult to extract. Building `main` should also take less time than building the PR branch because it is only necessary to build the difference. 

One thing we could try is to setup a release build for main that warms the cache but I would prefer to leave this for another PR. Mainly because we should potentially re-visit our caching overall to avoid running into GitHubs 10GB cache limit, which results in cache-pruning (e.g. warm the cache on main for test, and release build, don't populate the cache from PR branches)

---

_@MichaReiser reviewed on 2023-03-15 09:32_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:164 on 2023-03-15 09:32_

This adds the ecosystem output to the CI job summary

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:83 on 2023-03-15 09:33_

We only need the linux binary. Trying to run this on windows creates a warning because the file is named `ruff.exe` and not `ruff`

---

_@MichaReiser reviewed on 2023-03-15 09:33_

---

_Review comment by @charliermarsh on `.github/workflows/pr-comment.yaml`:67 on 2023-03-15 23:46_

(Should we put this under its own header for consistency? It looks like there's a header for `PR Check Results` and `Benchmark Results`, but not `Ecosystem Results` or `Ecosystem`.)

---

_@charliermarsh approved on 2023-03-15 23:47_

I love this

---

_Merged by @MichaReiser on 2023-03-16 08:05_

---

_Closed by @MichaReiser on 2023-03-16 08:05_

---

_Branch deleted on 2023-03-16 08:05_

---
