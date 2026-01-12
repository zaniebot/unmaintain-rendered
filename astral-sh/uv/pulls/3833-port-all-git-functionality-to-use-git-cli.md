```yaml
number: 3833
title: Port all git functionality to use git CLI
type: pull_request
state: merged
author: ibraheemdev
labels: []
assignees: []
merged: true
base: main
head: git-cli
created_at: 2024-05-24T21:48:10Z
updated_at: 2024-05-30T19:28:48Z
url: https://github.com/astral-sh/uv/pull/3833
synced_at: 2026-01-12T16:05:53Z
```

# Port all git functionality to use git CLI

---

_@ibraheemdev_

## Summary

We currently rely on libgit2 for most git-related functionality. However, libgit2 has long-standing performance issues, as well as lags significantly behind git in terms of new features. For these reasons we now use the git CLI by default for fetching repositories (https://github.com/astral-sh/uv/pull/1781). This PR completely drops libgit2 in favor of the git CLI for all git-related functionality, which should allow us to use features such as partial clones and sparse checkouts in the future for performance.

There is also a lot of technical debt in the current git code as it's mostly taken from Cargo. Switching to the git CLI *vastly* simplifies the `uv-git` codebase.

Eventually we might want to look into switching to [`gitoxide`](https://github.com/Byron/gitoxide), but it's currently too immature for our use case.

## Test Plan

<!-- How was it tested? -->


---

_@zanieb reviewed on 2024-05-24 23:15_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:161 on 2024-05-24 23:15_

```suggestion
    /// Initializes a Git repository
```

---

_Comment by @konstin on 2024-05-25 10:05_

Do we introduce a specific git cli minimum version requirement this way? I remember graphite breaking because my linux distro git was to old for their wrapper.

---

_Comment by @codspeed-hq[bot] on 2024-05-28 01:08_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/ibraheemdev:git-cli)

### Merging #3833 will **degrade performances by 9.61%**

<sub>Comparing <code>ibraheemdev:git-cli</code> (915d8d8) with <code>main</code> (502e042)</sub>



### Summary

`⚡ 1` improvements
`❌ 1` regressions
`✅ 11` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/ibraheemdev:git-cli)._

### Benchmarks breakdown

|     | Benchmark | `main` | `ibraheemdev:git-cli` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `resolve_warm_airflow` | 1.5 s | 1.4 s | +6.39% |
| ❌ | `resolve_warm_jupyter` | 80.2 ms | 88.8 ms | -9.61% |


---

_Marked ready for review by @ibraheemdev on 2024-05-28 16:59_

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-05-28 17:08_

---

_Review requested from @zanieb by @ibraheemdev on 2024-05-28 17:08_

---

_Comment by @ibraheemdev on 2024-05-28 17:10_

@konstin I don't think this relies on anything even relatively new, but I can audit the commands and see the minimum version we require.

---

_Comment by @ibraheemdev on 2024-05-28 17:39_

Benchmarks are pretty inconsistent but it seems like overall this change introduces a ~5% regression, which is expected because we have to shell out to Git quite a bit. We may be able to improve this somewhat taking into account the new cost (e.g. some of the `rev-parse` calls). However, the features that this enables us to use have a lot more potential for performance improvements, so, along with the reduced maintenance burden, it seems worth it.

---

_Review comment by @ibraheemdev on `crates/uv-git/src/sha.rs`:49 on 2024-05-28 17:42_

`libgit2` hex encodes object IDs to reduce their size (by half). However we can't use the `hex` crate directly because we would need to support odd-length strings, so I left it out for now. We could also just store the IDs as `String`s and do a bit of extra cloning.

---

_@ibraheemdev reviewed on 2024-05-28 17:42_

---

_@ibraheemdev reviewed on 2024-05-28 17:43_

---

_Review comment by @ibraheemdev on `crates/uv-git/src/git.rs`:165 on 2024-05-28 17:43_

I can't seem to find a way to do this with the CLI...

---

_@ibraheemdev reviewed on 2024-05-28 17:45_

---

_Review comment by @ibraheemdev on `crates/uv-git/src/git.rs`:409 on 2024-05-28 17:45_

Also unsure if we need to do this.

---

_@charliermarsh reviewed on 2024-05-30 01:17_

---

_Review comment by @charliermarsh on `crates/uv-git/src/sha.rs`:80 on 2024-05-30 01:17_

Do we need to validate that this a valid hex?

---

_@charliermarsh reviewed on 2024-05-30 01:22_

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:161 on 2024-05-30 01:22_

Nit: can you write out your TODOs as `TODO(ibraheem): ...` or `TODO(ibraheemdev): ...` or whatever you prefer? I like having an author annotation on TODOs -- it doesn't necessarily mean that you're responsible for _solving_ it, but it makes it immediately clear who has context on it.

---

_@charliermarsh reviewed on 2024-05-30 01:25_

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:367 on 2024-05-30 01:25_

I don't know if we should use `portable_display` here -- it will use a relative path in some cases. Should we use `simplified_display`, to get an absolute path?

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:409 on 2024-05-30 01:26_

I think we can just remove this. We don't support vendoring right? Unless I'm misunderstanding.

---

_@charliermarsh reviewed on 2024-05-30 01:26_

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:975 on 2024-05-30 01:27_

Why remove these two?

---

_@charliermarsh reviewed on 2024-05-30 01:27_

---

_@charliermarsh reviewed on 2024-05-30 01:28_

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:314 on 2024-05-30 01:28_

What does the `^0` signify here?

---

_Comment by @charliermarsh on 2024-05-30 01:29_

So just confirming -- the overall scheme here is the same, right? We have one checkout for Git repo, and then we fetch individual commits and clone them into separate directories to build?

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:171 on 2024-05-30 01:32_

Is there any risk of dropping important output here? (What did we do with Git CLI output before, when fetching?)

---

_@charliermarsh reviewed on 2024-05-30 01:32_

---

_@ibraheemdev reviewed on 2024-05-30 15:20_

---

_Review comment by @ibraheemdev on `crates/uv-git/src/sha.rs`:80 on 2024-05-30 15:20_

I don't think so, git will end up failing if we try to identify a commit using a malformed/broken hash anyways. I guess we could for better error messages, but it seems unlikely anyways?

---

_@ibraheemdev reviewed on 2024-05-30 15:26_

---

_Review comment by @ibraheemdev on `crates/uv-git/src/git.rs`:975 on 2024-05-30 15:26_

Both of those were linked to libgit2 issues. The git cli runs garbage collection automatically on `git fetch`, `git commit`, and other commands, so it shouldn't be a problem now.

---

_@ibraheemdev reviewed on 2024-05-30 15:28_

---

_Review comment by @ibraheemdev on `crates/uv-git/src/git.rs`:314 on 2024-05-30 15:28_

`^0` is short for `^{commit}`, which recursively "peels" to the underlying commit object. e.g. if the tag points to another tag, which then points to a commit. I'm not 100% sure if we need to do this, but the previous code had calls to `peel(ObjectType::Commit)`, so this retains the original behavior. I'll add a more detailed comment explaining this.

---

_@ibraheemdev reviewed on 2024-05-30 15:31_

---

_Review comment by @ibraheemdev on `crates/uv-git/src/git.rs`:171 on 2024-05-30 15:31_

This is the same thing we've always done in `fetch_with_cli`. The `true` means that stderr output will still be captured in the error message if the command fails, the `silent` callbacks just make sure that the stdout/stderr output is not piped to the user's terminal (`Command::{stdout,stderr}` are overridden).

---

_@ibraheemdev reviewed on 2024-05-30 15:32_

---

_Review comment by @ibraheemdev on `crates/uv-git/src/git.rs`:409 on 2024-05-30 15:32_

What sort of vendoring are you thinking of? `core.autocrlf` configures whether git automatically converts line endings (CRLF to LF) for checked out files. I think this can still affect us, but I'm not sure whether it matters (or why it matters for Cargo)?

---

_Comment by @ibraheemdev on 2024-05-30 15:33_

> So just confirming -- the overall scheme here is the same, right? We have one checkout for Git repo, and then we fetch individual commits and clone them into separate directories to build?

Yes, there should be no change in behavior.

---

_@ibraheemdev reviewed on 2024-05-30 15:38_

---

_Review comment by @ibraheemdev on `crates/uv-git/src/git.rs`:409 on 2024-05-30 15:38_

Ah you mean cargo vendoring. Yeah that looks to be why they needed this, it shouldn't affect us. I'll remove it.

---

_@ibraheemdev reviewed on 2024-05-30 16:26_

---

_Review comment by @ibraheemdev on `crates/uv-git/src/git.rs`:171 on 2024-05-30 16:26_

Turns out we can use `exec_with_output` here instead, which is a bit clearer.

---

_@charliermarsh approved on 2024-05-30 17:25_

---

_Merged by @ibraheemdev on 2024-05-30 19:28_

---

_Closed by @ibraheemdev on 2024-05-30 19:28_

---
