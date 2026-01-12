```yaml
number: 518
title: bump encoding_rs to 0.6
type: pull_request
state: merged
author: igor-raits
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2017-06-15T16:32:41Z
updated_at: 2017-11-13T18:04:46Z
url: https://github.com/BurntSushi/ripgrep/pull/518
synced_at: 2026-01-12T18:23:13Z
```

# bump encoding_rs to 0.6

---

_@igor-raits_

_No description provided._

---

_Comment by @igor-raits on 2017-06-15 16:42_

Looks like encoding_rs is not compatible with rust 1.12.... Not sure what you would like to do about that.

---

_Comment by @BurntSushi on 2017-06-15 16:52_

Right. Could you:

1. Bump `1.12.0` in `.travis.yml` to `1.16.0`?
2. Bumping just the `Cargo.toml` isn't sufficient. You'll also want to update `Cargo.lock`. You can do that by running `cargo update -p encoding_rs`.

Let's hope `encoding_rs` compiles on `1.16.0`, but unfortunately, `encoding_rs`'s CI doesn't seem to test on a specific Rust version. I'll file an issue.

---

_Comment by @BurntSushi on 2017-06-15 16:54_

issue filed: https://github.com/hsivonen/encoding_rs/issues/18#issue-236253315

---

_Comment by @BurntSushi on 2017-06-15 17:02_

@ignatenkobrain We should probably have a discussion at some point regarding how we'll deal with these bumps going forward. This happened to be timed pretty well, since I've been wanting to bump the minimum Rust version for ripgrep for a while now. But I don't think I'll always be so accommodating? For example, when the time comes for ripgrep 1.0 to be released, it's not clear to me whether that means "ripgrep 1.x will forever work on Rust 1.y, where Rust 1.y is the minimum Rust version supported at the time ripgrep 1.x was released." If I choose that policy, then it seems like ripgrep will gets its major version bump at least somewhat semi-frequently. But what if I didn't want to bump the major version that frequently and instead stuck with Rust 1.y? Well, invariably, that would mean that I won't be able to update some of my dependencies. How much of a problem will that be for you? And more broadly, what are your thoughts on this type of policy?

---

_Comment by @igor-raits on 2017-06-15 17:53_

@BurntSushi Speaking on behalf of Fedora Rust SIG:
1. We are going to follow latest stable releases of rust for all version of Fedora.
2. We prefer to have only one version of crate (we can have multiple versions at the same time) and that should be latest one.
3. Because of 2, we bump version of dependencies so it will use latest version and write patch if it is not building. For non-trivial cases where we can't port ourselves (time or lazyness) we create compat package with old crate which means that it also needs some patching for its dependencies to use latest version and so on. So far in repo we have ripgrep, rustfmt, rustfilt tools and only one compatibility package is there (toml-0.2, because there were a lot of changes to 0.3 and I was not able to port rustfmt myself).

To summarize: We don't care about minimal rust version, it should be just some stable version. We would prefer if upstream will use latest-shiny version of crates so we will not do patching here and there.

I think it definitely makes sense to support some old compilers, but I think it makes to support 2-3 last versions so people have time to migrate.

---

_Comment by @BurntSushi on 2017-06-15 18:14_

@ignatenkobrain Thank you. That is an extremely helpful data point.

---

_Comment by @Object905 on 2017-07-11 21:40_

Also now `encoding_rs` has `simd-accel` feature, so it would be nice to add it in `simd-accel` feature as well.

---

_Merged by @BurntSushi on 2017-07-30 22:00_

---

_Closed by @BurntSushi on 2017-07-30 22:00_

---

_Branch deleted on 2017-11-13 18:04_

---
