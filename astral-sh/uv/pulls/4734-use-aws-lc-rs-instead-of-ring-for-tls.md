```yaml
number: 4734
title: Use aws-lc-rs instead of ring for TLS
type: pull_request
state: closed
author: kcon-stackav
labels:
  - needs-decision
  - network
assignees: []
base: main
head: kcon-stackav/use-aws-lc-instead-of-ring
created_at: 2024-07-02T18:39:32Z
updated_at: 2026-01-02T15:05:26Z
url: https://github.com/astral-sh/uv/pull/4734
synced_at: 2026-01-12T16:06:25Z
```

# Use aws-lc-rs instead of ring for TLS

---

_@kcon-stackav_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR switches the TLS backend used by `reqwest` from `ring` to `aws-lc` to support more SSL certificate signature algorithms (especially P521 algorithms which aren't yet supported by `ring`: https://github.com/briansmith/ring/pull/1631).

Fixes #4534 

## Test Plan

I used `uv pip install` to try installing a package from a private PyPI server whose SSL certificate was signed using the ECDSA SHA-512 certificate signature algorithm and, using the Rust debugger, observed that `uv` did not fail to install the package due to not supporting the ECDSA SHA-512 certificate signature algorithm.




---

_Renamed from "Explore using aws-lc-rs instead of ring for TLS" to "Use aws-lc-rs instead of ring for TLS" by @kcon-stackav on 2024-07-02 18:42_

---

_Assigned to @zanieb by @zanieb on 2024-07-02 18:46_

---

_Label `network` added by @zanieb on 2024-07-02 18:47_

---

_@zanieb reviewed on 2024-07-02 20:20_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:141 on 2024-07-02 20:20_

Can we avoid panicking here? How can this fail? Can we get away with just warning if it fails?

---

_Comment by @zanieb on 2024-07-02 20:20_

Thanks for the pull request!

---

_Comment by @zanieb on 2024-07-02 20:36_

This [Windows failure](https://github.com/astral-sh/uv/actions/runs/9766298659/job/26958922044?pr=4734) also looks somewhat problematic (see https://github.com/aws/aws-lc/issues/1477 and [example fix](https://github.com/rustls/rustls/blob/75edb20a1e6a894089516053348b6137a425b9b4/.github/workflows/build.yml#L42-L44))

---

_Review comment by @kcon-stackav on `.github/workflows/ci.yml`:86 on 2024-07-02 22:32_

@zanieb I'm not very familiar with maturin, do you think installing NASM is needed for the `build-binaries` Windows job too?

https://github.com/astral-sh/uv/blob/66a4b8e6b7737b6dca78f6b60720488d0bc0889f/.github/workflows/build-binaries.yml#L153-L162

---

_Review comment by @kcon-stackav on `.github/workflows/ci.yml`:420 on 2024-07-02 22:37_

I tweaked this to match the size used in the `build binary | windows` job to try to help resolve this failure: https://github.com/actions/runner/issues/2491

Now we're hitting this other error which I'm not sure how to resolve: https://github.com/astral-sh/uv/actions/runs/9768734370/job/26966858668?pr=4734

```
  = note:    Creating library E:\uv\target\debug\deps\uv.lib and object E:\uv\target\debug\deps\uv.exp

          LINK : fatal error LNK1318: Unexpected PDB error; LIMIT (12) 'E:\uv\target\debug\deps\uv.pdb'
```

---

_@kcon-stackav reviewed on 2024-07-02 22:38_

---

_Marked ready for review by @kcon-stackav on 2024-07-02 22:38_

---

_@zanieb reviewed on 2024-07-02 22:41_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:420 on 2024-07-02 22:41_

That insane error is also an indication that the disk is full afaik, you can make it even bigger.

---

_@zanieb reviewed on 2024-07-02 22:41_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:86 on 2024-07-02 22:41_

cc @konstin 

---

_@zanieb reviewed on 2024-07-02 22:42_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:222 on 2024-07-02 22:42_

Note to self we should audit this action and consider just implementing it ourself if the install is straightforward

---

_@zanieb reviewed on 2024-07-02 22:44_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:144 on 2024-07-02 22:44_

Is this always safe due to the above `if`? 

---

_@kcon-stackav reviewed on 2024-07-02 22:48_

---

_Review comment by @kcon-stackav on `crates/uv-client/src/base_client.rs`:144 on 2024-07-02 22:48_

Yes I think so, but when I tested removing this `expect()` I saw that if `install_default()` did fail then no error would be reported and the TLS would fall back to `ring` which I wasn't sure was desired. Is there a different mechanism than `expect()` we should use here to avoid that?

---

_@zanieb reviewed on 2024-07-02 22:54_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:144 on 2024-07-02 22:54_

There are a couple options, like ignoring the result:

```rust
let _ = aws_lc_rs::default_provider().install_default();`
```

or warning the user on failure:

```rust
if let Err(err) = aws_lc_rs::default_provider().install_default() {
    warn_user_once!(...)
}
```

`expect` is okay if it should _never_ fail like... an assertion of an invariant. I don't understand when this would fail though, I'd need to poke around its implementation / documentation.

---

_@zanieb reviewed on 2024-07-02 23:25_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:144 on 2024-07-02 23:25_

It looks like the error attached to the result is just `Self` and has no information, right?

---

_@kcon-stackav reviewed on 2024-07-02 23:28_

---

_Review comment by @kcon-stackav on `crates/uv-client/src/base_client.rs`:144 on 2024-07-02 23:28_

Yes I understand the error holds the `CryptoProvider` that was previously installed as the default in that case.

---

_@zanieb reviewed on 2024-07-02 23:31_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:144 on 2024-07-02 23:31_

I guess we could capture it to say what we're using instead but it doesn't seem critical.

---

_Review comment by @kcon-stackav on `crates/uv-client/src/base_client.rs`:144 on 2024-07-02 23:52_

Ah yeah I thought about doing that but I wasn't sure the `CryptoProvider` had a nice human-readable name field we could use: https://docs.rs/rustls/latest/rustls/crypto/struct.CryptoProvider.html


---

_@kcon-stackav reviewed on 2024-07-02 23:52_

---

_@samypr100 reviewed on 2024-07-02 23:57_

---

_Review comment by @samypr100 on `.github/workflows/ci.yml`:222 on 2024-07-02 23:57_

ilammy also known for https://github.com/ilammy/msvc-dev-cmd

---

_Comment by @zanieb on 2024-07-10 14:59_

Sorry this is lingering, I'm just not sure of the trade-offs here since it changes our release pipeline and hasn't been requested by many people.

---

_@konstin reviewed on 2024-07-10 15:02_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:86 on 2024-07-10 15:02_

maturin calls `cargo build`, so they same rules should apply whether it's maturin or not.

---

_Comment by @kcon-stackav on 2024-07-15 17:47_

> Sorry this is lingering, I'm just not sure of the trade-offs here since it changes our release pipeline and hasn't been requested by many people.

No worries, and feel free to close this PR if not enough people are wanting it since I learned that switching from `ring` to `aws-lc-rs` doesn't solve my particular problem after all (https://github.com/astral-sh/uv/issues/4534#issuecomment-2204597948), but I believe https://github.com/astral-sh/uv/issues/1339 should once it becomes available.

---

_Label `needs-decision` added by @zanieb on 2024-07-15 20:29_

---

_Comment by @zanieb on 2024-07-19 17:03_

I think I'll close for now since this changes our build dependencies and there isn't a compelling reason to switch over at this time. Happy to reconsider in the future.

---

_Closed by @zanieb on 2024-07-19 17:03_

---

_Comment by @rami3l on 2024-08-01 10:55_

@zanieb As a part of Rustup's plan for the v1.28.0 release cycle, I've migrated Rustup to `aws-lc-rs` in https://github.com/rust-lang/rustup/pull/3898 for the very same reason (https://github.com/rust-lang/rustup/issues/3820), and I've been contacting `aws-lc`'s side, providing useful information to help address various issues regarding the cross compilation of this library. I'd love to share more experience with you when we're able to actually ship the build, if you're interested.

---

_Comment by @zanieb on 2024-08-01 12:24_

Thanks @rami3l, I appreciate the heads up and am definitely interested.

---

_Comment by @cyberksh on 2026-01-02 14:54_

Hello are we planning to add support for this? 
If this could be an optional feature for Linux only or we could use the prebuilt NASM modules that are provided by aws-lc-rs

---

_Comment by @rami3l on 2026-01-02 15:05_

> Thanks @rami3l, I appreciate the heads up and am definitely interested.

@zanieb Sorry for the long wait, but as you might have noticed, we've been using `aws-lc-rs` in production for about a year now. The team has been quite responsive in terms of aligning our supported targets so it might be worth it to make the move.

---
