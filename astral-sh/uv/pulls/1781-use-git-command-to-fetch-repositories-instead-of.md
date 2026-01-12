```yaml
number: 1781
title: "Use `git` command to fetch repositories instead of `libgit2` for robust SSH support"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/git-auth-ssh
created_at: 2024-02-20T20:06:12Z
updated_at: 2024-02-21T18:44:33Z
url: https://github.com/astral-sh/uv/pull/1781
synced_at: 2026-01-12T16:04:44Z
```

# Use `git` command to fetch repositories instead of `libgit2` for robust SSH support

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/1775
Closes https://github.com/astral-sh/uv/issues/1452
Closes https://github.com/astral-sh/uv/issues/1514
Follows https://github.com/astral-sh/uv/pull/1717

libgit2 does not support host names with extra identifiers during SSH lookup (e.g. [`github.com-some_identifier`](
https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys#using-multiple-repositories-on-one-server)) so we use the `git` command instead for fetching. This is required for `pip` parity.

See the [Cargo documentation](https://doc.rust-lang.org/nightly/cargo/reference/config.html#netgit-fetch-with-cli) for more details on using the `git` CLI instead of libgit2. We may want to try to use libgit2 first in the future, as it is more performant (#1786).

We now support authentication with:

```
git+ssh://git@<hostname>/...
git+ssh://git@<hostname>-<identifier>/...
```

Tested with a deploy key e.g.

```
cargo run -- \
    pip install uv-private-pypackage@git+ssh://git@github.com-test-uv-private-pypackage/astral-test/uv-private-pypackage.git \
    --reinstall --no-cache -v
```

and

```
cargo run -- \
    pip install uv-private-pypackage@git+ssh://git@github.com/astral-test/uv-private-pypackage.git \
    --reinstall --no-cache -v     
```

with a ssh config like

```
Host github.com
        Hostname github.com
        IdentityFile=/Users/mz/.ssh/id_ed25519

Host github.com-test-uv-private-pypackage
        Hostname github.com
        IdentityFile=/Users/mz/.ssh/id_ed25519
```

It seems quite hard to add test coverage for this to the test suite, as we'd need to add the SSH key and I don't know how to isolate that from affecting other developer's machines.

---

_@zanieb reviewed on 2024-02-20 23:44_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:1014 on 2024-02-20 23:44_

I think the approach at https://github.com/astral-sh/uv/pull/1786/commits/51b38f9a871c42ac6638328718c04630b216e56f#diff-e0d9d19e8dbea69ecd5354dcbec7d87e259e801687037360fac463c3143c16c8R1004-R1011 is probably a better way to handle the "branch or tag" refspecs.

---

_@zanieb reviewed on 2024-02-20 23:54_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:1014 on 2024-02-20 23:54_

Picked this out of https://github.com/astral-sh/uv/pull/1786 in https://github.com/astral-sh/uv/pull/1781/commits/afafb66b34c604c3ba274ee402083973a953d77a

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:1013 on 2024-02-20 23:55_

Is there a better way to combine these?

---

_@zanieb reviewed on 2024-02-20 23:55_

---

_@zanieb reviewed on 2024-02-20 23:57_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:977 on 2024-02-20 23:57_

This could hide errors. It's unclear to me what happens inside libgit2 but it definitely succeeds with missing refspecs.

Alternatively, we could special case the `BranchOrTag` case and take the first matching refspec in that case but continue passing them all in at once for other cases?

---

_Marked ready for review by @zanieb on 2024-02-21 00:02_

---

_Review requested from @charliermarsh by @zanieb on 2024-02-21 00:03_

---

_Review requested from @snowsignal by @zanieb on 2024-02-21 00:03_

---

_Label `bug` added by @zanieb on 2024-02-21 00:18_

---

_@charliermarsh reviewed on 2024-02-21 00:20_

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:977 on 2024-02-21 00:20_

Do we "typically" have multiple refspecs here? Like would we expect this to cause a noticeable slowdown, doing them by-one-one?

---

_@zanieb reviewed on 2024-02-21 00:23_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:977 on 2024-02-21 00:23_

It's just two cases afaik

- We have a short commit and fetch _everything_ via two refspecs
- We have a branch or tag and fetch each of those (we only want the first one that is present though)

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:1013 on 2024-02-21 00:31_

Does the repeated `.context` lead them to be rendered one-by-one, in a vertical list?

I think this is reasonable...

---

_@charliermarsh reviewed on 2024-02-21 00:32_

---

_@charliermarsh approved on 2024-02-21 00:32_

---

_Review comment by @charliermarsh on `crates/uv-git/src/git.rs`:1013 on 2024-02-21 00:32_

@BurntSushi would have good intuition here (but doesn't need to block the PR).

---

_@charliermarsh reviewed on 2024-02-21 00:32_

---

_Review comment by @snowsignal on `crates/uv-git/src/git.rs`:981 on 2024-02-21 00:54_

Small performance nit: `if !results.iter().any(Result::is_ok)` would be slightly faster as a check since it would return early.

---

_@snowsignal approved on 2024-02-21 00:58_

---

_@zanieb reviewed on 2024-02-21 01:13_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:1013 on 2024-02-21 01:13_

per https://github.com/astral-sh/uv/pull/1786/commits/acc4ff3b7f8abf5ca26a1944deb831fc3fa07612 I don't know if these are surfaced _at all_?

```bash
$ cargo run -- pip install "uv-public-pypackage @ git+https://github.com/astral-test/uv-public-pypackage@2.0.0" -v --reinstall --no-cache
...
    0.373747s DEBUG uv_git::source Updating git source `Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/astral-test/uv-public-pypackage", query: None, fragment: None }`
    0.377127s DEBUG uv_git::git Attempting GitHub fast path for: https://api.github.com/repos/astral-test/uv-public-pypackage/commits/2.0.0
    0.624614s DEBUG uv_git::git failed to check github HTTP status client error (422 Unprocessable Entity) for url (https://api.github.com/repos/astral-test/uv-public-pypackage/commits/2.0.0)
    0.625373s DEBUG uv_git::git skipping gc as there's only 0 pack files
    0.626108s DEBUG uv_git::git Performing a Git fetch for: https://github.com/astral-test/uv-public-pypackage
    0.627057s DEBUG uv_git::git initiating fetch of ["+refs/heads/2.0.0:refs/remotes/origin/2.0.0", "+refs/tags/2.0.0:refs/remotes/origin/tags/2.0.0"] from https://github.com/astral-test/uv-public-pypackage
error: Failed to download and build: uv-public-pypackage @ git+https://github.com/astral-test/uv-public-pypackage@2.0.0
  Caused by: Git operation failed
  Caused by: failed to find branch or tag `2.0.0`
```

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:981 on 2024-02-21 01:17_

Won't `all` exit early on the first `Result::Ok` too?

---

_@zanieb reviewed on 2024-02-21 01:17_

---

_@snowsignal reviewed on 2024-02-21 04:10_

---

_Review comment by @snowsignal on `crates/uv-git/src/git.rs`:981 on 2024-02-21 04:10_

Right, sorry. My bad 🤦‍♀️ 

---

_@BurntSushi reviewed on 2024-02-21 12:24_

---

_Review comment by @BurntSushi on `crates/uv-git/src/git.rs`:1013 on 2024-02-21 12:24_

Yeah this _should_ create a causal chain with each link being an error in `results`. For example:

```rust
fn main() -> anyhow::Result<()> {
    let mut err = anyhow::anyhow!("error #1");
    for i in 2..=10 {
        err = err.context(format!("error #{i}"));
    }
    Err(err)
}
```

Has this output:

```
[andrew@duff anyhow-err-combine]$ cargo run -q
Error: error #10

Caused by:
    0: error #9
    1: error #8
    2: error #7
    3: error #6
    4: error #5
    5: error #4
    6: error #3
    7: error #2
    8: error #1
```

I'm not sure if this is the right presentation though. Because these are more like sibling errors than errors in a causal chain. Perhaps if we "picked" on error and added a `tracing::warn!` for the others? Or alternatively, smushed all of the errors into one, without creating a new link in the causal chain for each one.

---

_@zanieb reviewed on 2024-02-21 15:26_

---

_Review comment by @zanieb on `crates/uv-git/src/git.rs`:1013 on 2024-02-21 15:26_

I don't think these are actually displayed right now so I'd like to do the simplest thing. I'll log the errors and raise a new one.

---

_@zanieb reviewed on 2024-02-21 16:12_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:135 on 2024-02-21 16:12_

I want to return `Vec<(&str, &str)>` here where the lifetime is of the `TestContext` but can't because `cache_dir.canonicalize()` is owned by the function. Suggestions?

---

_@zanieb reviewed on 2024-02-21 16:14_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:135 on 2024-02-21 16:14_

Do I need to store the canonicalized path on the struct too?

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:135 on 2024-02-21 16:15_

The reason this is a problem is e.g.

https://github.com/astral-sh/uv/pull/1781/files#diff-10e2684d108814c5407ca429afe35ac9916b5577d17b45afc4e800f6582f9ce7R949-R954

---

_@zanieb reviewed on 2024-02-21 16:15_

---

_@BurntSushi reviewed on 2024-02-21 16:16_

---

_Review comment by @BurntSushi on `crates/uv/tests/common/mod.rs`:135 on 2024-02-21 16:16_

If you can store the canonicalized path on `TestContext`, then that would do the trick I think?

But also, `Vec<(String, String)>` seems fine to me unless there's a reason not to do that?

---

_@BurntSushi reviewed on 2024-02-21 16:18_

---

_Review comment by @BurntSushi on `crates/uv/tests/common/mod.rs`:135 on 2024-02-21 16:18_

OK, so the reason why you want this is that `uv_snapshot!` wants a `Vec<(&str, &str)>`, is that right? I'm not familiar with `uv_snapshot!`, but if you can change it to accept `Vec<(impl AsRef<str>, impl AsRef<str>)>`, then that should make it work for both `Vec<(&str, &str)>` and `Vec<(String, String)>`.

---

_@zanieb reviewed on 2024-02-21 16:21_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:135 on 2024-02-21 16:21_

Just going to add it to the struct

---

_Comment by @MichaReiser on 2024-02-21 16:32_

Do we have any benchmarks that use git dependencies? Does it change the performance in significant ways?

---

_Comment by @zanieb on 2024-02-21 16:53_

> Do we have any benchmarks that use git dependencies? Does it change the performance in significant ways?

I don't think we do. Honestly I'm not sure it makes sense to take the time to measure right now because we _need_ to support authentication. I think benchmarking should be done when considering #1786 instead.

---

_Merged by @zanieb on 2024-02-21 18:44_

---

_Closed by @zanieb on 2024-02-21 18:44_

---

_Branch deleted on 2024-02-21 18:44_

---
