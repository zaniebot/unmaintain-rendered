```yaml
number: 283
title: Add support for Git dependencies
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/vcs
created_at: 2023-11-02T01:29:17Z
updated_at: 2023-11-02T15:14:56Z
url: https://github.com/astral-sh/uv/pull/283
synced_at: 2026-01-12T16:03:51Z
```

# Add support for Git dependencies

---

_@charliermarsh_

## Summary

This PR adds support for Git dependencies, like:

```
flask @ git+https://github.com/pallets/flask.git
```

Right now, they're only supported in the resolver (and not the installer), since the installer doesn't yet support source distributions at all.

The general approach here is based on Cargo's Git implementation. Specifically, I adapted Cargo's [`git`](https://github.com/rust-lang/cargo/blob/23eb492cf920ce051abfc56bbaf838514dc8365c/src/cargo/sources/git/mod.rs) module to perform the cloning, which is based on `libgit2`.

As compared to Cargo's implementation, I made the following changes:

- Removed any unnecessary code.
- Fixed any Clippy errors for our stricter ruleset.
- Removed the dependency on `curl`, in favor of `reqwest` which we use elsewhere.
- Removed the ability to use `gix`. Cargo allows the use of `gix` as an experimental flag, but it only supports a small subset of the operations. When Cargo fully adopts `gix`, we should plan to do the same.
- Removed Cargo's host key checking. We need to re-add this! I'll do it shortly.
- Removed Cargo's progress bars. We should re-add this too, but we use `indicatif` and Cargo had their own thing.

There are a few follow-ups to consider:

- Adding support in the installer.
- When we lock, we should write out the Git URL that includes the exact SHA. This lets us cache in perpetuity and avoids dependencies changing without re-locking.
- When we resolve, we should _always_ try to refresh Git dependencies. (Right now, we skip if the wheel was already built.)

I'll work on the latter two in follow-up PRs.

Closes #202.


---

_@charliermarsh reviewed on 2023-11-02 01:31_

---

_Review comment by @charliermarsh on `crates/puffin-vcs/src/git.rs`:1 on 2023-11-02 01:31_

This file does the actual Git operations. It's largely derived from Cargo.

---

_@charliermarsh reviewed on 2023-11-02 01:35_

---

_Review comment by @charliermarsh on `crates/puffin-vcs/src/source.rs`:89 on 2023-11-02 01:35_

The cool thing about Cargo's implementation is that they maintain a "database" of repositories. So every repository that you ever check out is included in there. Then, peer to it, they have a directory of checkouts, which are created by checking out the commit in the database and hardlinking from the database to the checkout. So it's fast to switch between commits of the same repository.

---

_Marked ready for review by @charliermarsh on 2023-11-02 04:09_

---

_@charliermarsh reviewed on 2023-11-02 04:10_

---

_Review comment by @charliermarsh on `crates/puffin-git/src/git.rs`:892 on 2023-11-02 04:10_

There are so many issues about Cargo not working as expected with SSH dependencies that I'm wondering if we should just make this the default (https://github.com/rust-lang/cargo/issues/2078, https://github.com/rust-lang/cargo/issues/3381, etc.).

---

_Review requested from @zanieb by @charliermarsh on 2023-11-02 04:44_

---

_Review requested from @konstin by @charliermarsh on 2023-11-02 04:44_

---

_@charliermarsh reviewed on 2023-11-02 04:44_

---

_Review comment by @charliermarsh on `crates/puffin-git/src/git.rs`:892 on 2023-11-02 04:44_

(This would not remove our dependency on `libgit2` since this is _just_ the fetch.)

---

_Review comment by @konstin on `crates/puffin-build/src/lib.rs`:127 on 2023-11-02 12:41_

The method docstring needs updating (outside of comment range)

---

_Review comment by @konstin on `crates/puffin-build/src/lib.rs`:136 on 2023-11-02 12:42_

```suggestion
        let source_tree = if fs::metadata(sdist)?.is_dir() {
            sdist.to_path_buf()
        } else {
             debug!("Unpacking for build: {}", sdist.display());
            let extracted = temp_dir.path().join("extracted");
            extract_archive(sdist, &extracted)?
        };
```

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver.rs`:663 on 2023-11-02 12:48_

nit: Either remove the "from" or show the url

---

_@konstin approved on 2023-11-02 12:56_

I didn't review the parts copied from cargo.

It's a pity that we copy the code and there's no reusable crate.

---

_Comment by @charliermarsh on 2023-11-02 12:58_

@konstin - I agree although I'm actually happy with how little code we had to copy in the end. It may not feel that way from the review, but I was able to pare it down a lot. Now, we're really only borrowing the actual Git operations and logic around when to refresh, when to reset, recursively cloning submodules, etc., which at least feels somewhat domain agnostic.

---

_Merged by @charliermarsh on 2023-11-02 15:14_

---

_Closed by @charliermarsh on 2023-11-02 15:14_

---

_Branch deleted on 2023-11-02 15:14_

---
