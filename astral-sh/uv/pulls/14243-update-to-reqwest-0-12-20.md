```yaml
number: 14243
title: Update to reqwest 0.12.20
type: pull_request
state: closed
author: musicinmybrain
labels:
  - enhancement
assignees: []
base: main
head: unpin-reqwest
created_at: 2025-06-24T15:47:43Z
updated_at: 2025-07-07T11:43:47Z
url: https://github.com/astral-sh/uv/pull/14243
synced_at: 2026-01-12T16:11:06Z
```

# Update to reqwest 0.12.20

---

_@musicinmybrain_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The `reqwest` dependency was pinned to `=0.12.15` in https://github.com/astral-sh/uv/commit/794b3ec7506e0430b088a24d2e680ed7ac8d14b4 as a workaround for https://github.com/astral-sh/uv/issues/13831.

Since the [fix is in `hyper-util` 0.1.14](https://github.com/hyperium/hyper/issues/3900), and `Cargo.lock` has that version, we *should* be able to unpin `reqwest` now.

(While we’re at it, it would seem to make sense to update the dev-dependency on `hyper-util` in `uv-client` from 0.1.8 to 0.1.14, although this does not affect `Cargo.lock` in practice.)

See also the declined PR https://github.com/seanmonstar/reqwest/pull/2743.

When I unpinned `reqwest`, I had to update four snapshots due to a reduced level of detail in certain error messages, which looks innocuous.

The tests still don’t pass due to the following:

```
---- registry_client::tests::test_redirect_preserve_fragment stdout ----

thread 'registry_client::tests::test_redirect_preserve_fragment' panicked at crates/uv-client/src/registry_client.rs:1442:9:
assertion `left == right` failed: Requests should preserve fragment
  left: "http://127.0.0.1:40859/foo"
 right: "http://127.0.0.1:40859/foo#fragment"
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

This seems to be a regression associated with updating `reqwest` from 0.12.15 to 0.12.16 (https://github.com/seanmonstar/reqwest/compare/v0.12.15...v0.12.16), since I can reproduce it after `cargo update reqwest --precise 0.12.16`.

I’m not sure exactly what should be done about the above test failure. Obviously, `uv` will need to unpin `reqwest` at some point. I would appreciate advice.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
```
$ cargo run python install
$ cargo test
```


---

_Renamed from "[DRAFT] Unpin reqwest" to "Update to reqwest 0.12.20" by @konstin on 2025-06-24 19:05_

---

_Label `enhancement` added by @konstin on 2025-06-24 19:05_

---

_@konstin approved on 2025-06-24 19:06_

Thanks!

---

_Comment by @konstin on 2025-06-24 20:36_

CC @jtfmumm regarding the regression of `test_redirect_preserve_fragment`, i assume this changed in https://github.com/seanmonstar/reqwest/pull/2617?

---

_Closed by @jtfmumm on 2025-07-07 11:39_

---
