```yaml
number: 2945
title: "Add hash-checking support to `install` and `sync`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/check-hashes-iii
created_at: 2024-04-09T20:15:42Z
updated_at: 2024-04-10T19:09:05Z
url: https://github.com/astral-sh/uv/pull/2945
synced_at: 2026-01-12T16:05:19Z
```

# Add hash-checking support to `install` and `sync`

---

_@charliermarsh_

## Summary

This PR adds support for hash-checking mode in `pip install` and `pip sync`. It's a large change, both in terms of the size of the diff and the modifications in behavior, but it's also one that's hard to merge in pieces (at least, with any test coverage) since it needs to work end-to-end to be useful and testable.

Here are some of the most important highlights:

- We store hashes in the cache. Where we previously stored pointers to unzipped wheels in the `archives` directory, we now store pointers with a set of known hashes. So every pointer to an unzipped wheel also includes its known hashes.
- By default, we don't compute any hashes. If the user runs with `--require-hashes`, and the cache doesn't contain those hashes, we invalidate the cache, redownload the wheel, and compute the hashes as we go. For users that don't run with `--require-hashes`, there will be no change in performance. For users that _do_, the only change will be if they don't run with `--generate-hashes` -- then they may see some repeated work between resolution and installation, if they use `pip compile` then `pip sync`.
- Many of the distribution types now include a `hashes` field, like `CachedDist` and `LocalWheel`.
- Our behavior is similar to pip, in that we enforce hashes when pulling any remote distributions, and when pulling from our own cache. Like pip, though, we _don't_ enforce hashes if a distribution is _already_ installed.
- Hash validity is enforced in a few different places:
  1. During resolution, we enforce hash validity based on the hashes reported by the registry. If we need to access a source distribution, though, we then enforce hash validity at that point too, prior to running any untrusted code. (This is enforced in the distribution database.)
  2. In the install plan, we _only_ add cached distributions that have matching hashes. If a cached distribution is missing any hashes, or the hashes don't match, we don't return them from the install plan.
  3. In the downloader, we _only_ return distributions with matching hashes.
  4. The final combination of "things we install" are: (1) the wheels from the cache, and (2) the downloaded wheels. So this ensures that we never install any mismatching distributions.
- Like pip, if `--require-hashes` is provided, we require that _all_ distributions are pinned with either `==` or a direct URL. We also require that _all_ distributions have hashes.

There are a few notable TODOs:

- We don't support hash-checking mode for unnamed requirements. These should be _somewhat_ rare, though? Since `pip compile` never outputs unnamed requirements. I can fix this, it's just some additional work.
- We don't automatically enable `--require-hashes` with a hash exists in the requirements file. We require `--require-hashes`.

Closes #474.

## Test Plan

I'd like to add some tests for registries that report incorrect hashes, but otherwise: `cargo test`


---

_Label `enhancement` added by @charliermarsh on 2024-04-09 20:15_

---

_Marked ready for review by @charliermarsh on 2024-04-09 20:15_

---

_Converted to draft by @charliermarsh on 2024-04-09 20:16_

---

_@charliermarsh reviewed on 2024-04-09 20:28_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/prioritized_distribution.rs`:459 on 2024-04-09 20:28_

@zanieb - I could use your help with this part. I'm not sure if I did the comparisons correctly here.

---

_@charliermarsh reviewed on 2024-04-09 20:29_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/hash.rs`:146 on 2024-04-09 20:29_

@BurntSushi - Do you mind reviewing this component? The basic idea is a wrapper around any reader that can compute a set of hashes as we go. It's then used to wrap the async streams as we unzip.

---

_Marked ready for review by @charliermarsh on 2024-04-09 21:18_

---

_Comment by @charliermarsh on 2024-04-09 21:27_

Open to advice on how best to test that if a registry tells us the _wrong_ hashes, we still validate them at install time. I need a registry or `--find-links` entry that reports the wrong hashes for a given distribution.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-04-09 21:27_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-09 21:27_

---

_Review requested from @konstin by @charliermarsh on 2024-04-09 21:27_

---

_Review request for @BurntSushi removed by @charliermarsh on 2024-04-09 21:27_

---

_Comment by @charliermarsh on 2024-04-09 21:47_

A few TODOs:

- [x] Add some tests for invalid indexes.
- [x] Run a basic benchmark (especially, to ensure that there's no regression for non-`--require-hashes`).

I'd also like to refactor the dataflow from requirements file to `RequireHashes`, but this isn't required to merge.


---

_@charliermarsh reviewed on 2024-04-09 22:03_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:4226 on 2024-04-09 22:03_

This is a TODO. We reject distributions without hashes when `--require-hashes` is provided, whereas in reality we should compute them.

---

_@zanieb reviewed on 2024-04-09 22:29_

---

_Review comment by @zanieb on `crates/distribution-types/src/prioritized_distribution.rs`:459 on 2024-04-09 22:29_

I think this is actually wrong, although I'm not sure if it matters. You're supposed to be enforcing an ordering but here you're saying that `MissingHash` and `MismatchedHash` are _never_ more compatible than the other value. This means that if we see both `MissingHash` and `MismatchedHash` we would arbitrarily display the first one we saw instead of preferring to present one of them to the user.

---

_@charliermarsh reviewed on 2024-04-09 23:12_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/prioritized_distribution.rs`:459 on 2024-04-09 23:12_

I decided to change up the strategy in https://github.com/astral-sh/uv/pull/2949, such that we treat distributions without hashes as compatible (but lower-priority). I can merge that into this PR if you agree with the change.

---

_@charliermarsh reviewed on 2024-04-09 23:25_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:4226 on 2024-04-09 23:25_

I solved this in #2949.

---

_Review comment by @konstin on `Cargo.toml`:97 on 2024-04-10 12:03_

Do we want to support md5? It's cryptographically broken

---

_Review comment by @konstin on `crates/uv-client/src/cached_client.rs`:303 on 2024-04-10 12:14_

I don't think we need to instrument function

```suggestion
```

---

_Review comment by @konstin on `crates/uv-distribution/src/archive.rs`:12 on 2024-04-10 12:17_

nit: we have a constructor and getters

```suggestion
    /// The path to the archive entry in the wheel's archive bucket.
    path: PathBuf,
    /// The computed hashes of the archive.
    hashes: Vec<HashDigest>,
```

---

_Review comment by @konstin on `crates/uv-distribution/src/distribution_database.rs`:545 on 2024-04-10 12:23_

Why don't we spawn blocking here too?

---

_Review comment by @konstin on `crates/uv-distribution/src/error.rs`:87 on 2024-04-10 12:24_

```suggestion
    #[error("Failed to finish hashing file")]
```

---

_Comment by @konstin on 2024-04-10 12:28_

Not exactly this PR (more #2909), but do we have an explanation for .http/.rev files in the docstrings?

---

_Review comment by @konstin on `crates/uv-extract/src/hash.rs`:35 on 2024-04-10 12:34_

Just to double check, these are all run with buffered readers so we don't do those context switches too often, right?

---

_@konstin approved on 2024-04-10 12:45_

Nice work!

---

_@charliermarsh reviewed on 2024-04-10 13:53_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/hash.rs`:35 on 2024-04-10 13:53_

Yes!

---

_@charliermarsh reviewed on 2024-04-10 13:54_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:545 on 2024-04-10 13:54_

Because it's async whereas the other version is sync. Does that make sense?

---

_Review comment by @BurntSushi on `crates/uv-extract/src/hash.rs`:120 on 2024-04-10 15:09_

This is potentially putting 8KB on the stack. Maybe not an issue, but it's big enough to stand out to me.

I think I'd probably just use `vec![0; 8192]` here instead. You could put it in a `thread_local!` to amortize the alloc if you're concerned about perf.

---

_Review comment by @BurntSushi on `crates/uv-extract/src/hash.rs`:146 on 2024-04-10 15:10_

Aye. I think I have just one concern but otherwise this looks pretty reasonable to me.

---

_@BurntSushi reviewed on 2024-04-10 15:10_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/hash.rs`:146 on 2024-04-10 15:11_

Tyvm!

---

_@charliermarsh reviewed on 2024-04-10 15:11_

---

_@konstin reviewed on 2024-04-10 17:31_

---

_Review comment by @konstin on `crates/uv-distribution/src/distribution_database.rs`:545 on 2024-04-10 17:31_

yeah

---

_Merged by @charliermarsh on 2024-04-10 19:09_

---

_Closed by @charliermarsh on 2024-04-10 19:09_

---

_Branch deleted on 2024-04-10 19:09_

---
