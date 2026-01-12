```yaml
number: 16447
title: "Test registry_client::tests::test_redirect_to_server_with_credentials is flaky"
type: issue
state: closed
author: musicinmybrain
labels:
  - bug
  - ci-flake
assignees: []
created_at: 2025-10-25T09:37:59Z
updated_at: 2025-12-03T13:51:26Z
url: https://github.com/astral-sh/uv/issues/16447
synced_at: 2026-01-12T16:02:32Z
```

# Test registry_client::tests::test_redirect_to_server_with_credentials is flaky

---

_@musicinmybrain_

### Summary

In ten “scratch builds” of [Fedora’s `uv` package](https://src.fedoraproject.org/rpms/uv), the test `registry_client::tests::test_redirect_to_server_with_credentials` failed once on `ppc64le`:

```
failures:
---- registry_client::tests::test_redirect_to_server_with_credentials stdout ----
thread 'registry_client::tests::test_redirect_to_server_with_credentials' panicked at crates/uv-client/src/registry_client.rs:1425:9:
assertion `left == right` failed: Requests should fail if credentials are missing
  left: 200
 right: 401
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

This seems to be a relatively new issue in the 0.9.x series. I have seen it a couple of times, always on `ppc64le`. I don’t know if the problem is arch-specific, or if there is just a race condition that happens to be easier to hit on that architecture. I am temporarily without access to a computer with enough RAM to easily build `uv` locally, so I didn’t try to investigate this further. I’m just skipping the test for now.

### Platform

Fedora Rawhide, ppc64le

### Version

uv 0.9.5

### Python version

Python 3.14.0

---

_Label `bug` added by @musicinmybrain on 2025-10-25 09:38_

---

_Comment by @musicinmybrain on 2025-10-25 12:46_

Looking more closely, this seems to have been already failing flakily on `s390x` in the same way since at least 0.7.19, and I just hadn’t gotten around to reporting it. So what’s new is just that the failure seems to now be more frequent on s390x than it once was.

---

_Renamed from "Test registry_client::tests::test_redirect_to_server_with_credentials is flaky on ppc64le" to "Test registry_client::tests::test_redirect_to_server_with_credentials is flaky on ppc64le, s390x" by @musicinmybrain on 2025-10-25 12:46_

---

_Comment by @zanieb on 2025-10-25 14:17_

Huh that's weird. It's just a local test server, I can't imagine where there'd be a race condition.

---

_Label `ci-flake` added by @zanieb on 2025-10-25 14:17_

---

_Comment by @konstin on 2025-10-27 19:17_

The problem is that all tests use the same credentials, and we have a global static credentials cache. When using `cargo test` instead of `cargo nextest run`, the credentials get reused between tests if the test server URL is the same. I can reproduce this locally by inserting a sleep statement at the top of `test_redirect_to_server_with_credentials`: `tokio::time::sleep(std::time::Duration::from_millis(500)).await;`

---

_Comment by @zanieb on 2025-10-27 19:32_

Ah good catch. I would recommend using nextest if you can, since it provides process-level isolation per test. Other test runners will only be supported on a best-effort basis.

---

_Comment by @musicinmybrain on 2025-10-28 00:40_

> Ah good catch. I would recommend using nextest if you can, since it provides process-level isolation per test. Other test runners will only be supported on a best-effort basis.

Hmm, this is good to know. It’s likely that trying to package nextest in Fedora specifically for the purpose of testing uv will be too much effort, since nextest has many dependencies, aggressive dependency version bound updates, and frequent releases. It is easy enough to skip this particular test for now. I will have to hope that there is not too much divergence between the results of cargo test and the results of cargo nextest in the future.

---

_Renamed from "Test registry_client::tests::test_redirect_to_server_with_credentials is flaky on ppc64le, s390x" to "Test registry_client::tests::test_redirect_to_server_with_credentials is flaky" by @musicinmybrain on 2025-12-03 10:32_

---

_Comment by @musicinmybrain on 2025-12-03 10:33_

Changing the title since I’ve now observed this on `x86_64`.

---

_Comment by @konstin on 2025-12-03 12:47_

This will be fixed by https://github.com/astral-sh/uv/pull/16768 (which is otherwise a refactoring).

---

_Closed by @konstin on 2025-12-03 13:51_

---
