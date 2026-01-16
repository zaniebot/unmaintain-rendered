```yaml
number: 16772
title: "Ignore `SSL_CERT_FILE` with empty value"
type: pull_request
state: open
author: taearls
labels:
  - bug
assignees: []
base: main
head: 16712/ssl-cert-file-ignores-empty-value
created_at: 2025-11-18T18:19:49Z
updated_at: 2026-01-16T19:37:00Z
url: https://github.com/astral-sh/uv/pull/16772
synced_at: 2026-01-16T20:03:55Z
```

# Ignore `SSL_CERT_FILE` with empty value

---

_@taearls_

## Summary

Fixes #16712.

This PR adds empty value filtering to `SSL_CERT_FILE` environment variable handling, matching the existing behavior of `SSL_CERT_DIR`.

Previously, setting `SSL_CERT_FILE=""` would cause uv to attempt to use an empty string as a file path, resulting in a "file does not exist" warning. With this change, an empty `SSL_CERT_FILE` is treated as unset, falling back to default certificate handling.

The fix adds `.filter(|v| !v.is_empty())` to the `SSL_CERT_FILE` processing in `crates/uv-client/src/base_client.rs`, consistent with the pattern used for `SSL_CERT_DIR` and other environment variables like `NO_COLOR` and `FORCE_COLOR`.

## Test Plan

Added a test case to `crates/uv-client/tests/it/ssl_certs.rs` that:
1. Sets `SSL_CERT_FILE` to an empty string
2. Verifies the connection fails with `UnknownCA` (indicating default certificate handling was used, not a path-related error)

Run the test with:
```bash
cargo test --package uv-client --test it -- ssl_env_vars --exact
```


---

_Review requested from @samypr100 by @konstin on 2025-11-19 09:43_

---

_Assigned to @zanieb by @konstin on 2025-11-19 09:43_

---

_Label `bug` added by @konstin on 2025-11-19 09:43_

---

_Unassigned @zanieb by @konstin on 2025-11-19 09:43_

---

_Review requested from @zanieb by @konstin on 2025-11-19 09:43_

---

_Renamed from "fix: SSL_CERT_FILE ignores empty value" to "Ignore `SSL_CERT_FILE` with empty value" by @konstin on 2025-11-19 09:44_

---

_Review comment by @samypr100 on `crates/uv-client/tests/it/ssl_certs.rs`:216 on 2025-11-20 02:57_

Thanks for adding a test!

In this case the test with `SSL_CERT_FILE=""` would behave the same way before and after the code changes given this is a server using a self-signed certificate, hence I don't think its actually verifying the changes.

Having said that, I'm not quite sure if we should have a test for this change. If we'd like to have a test I believe we can add one to the bottom of this file that specifically tests the warning scenarios for these env vars, e.g. something like

```rust
// SAFETY: This test is meant to run in isolation
#[tokio::test]
#[allow(unsafe_code)]
async fn ssl_env_vars_warnings() -> Result<()> {
    // Enable user-facing warnings
    uv_warnings::enable();

    unsafe {
        std::env::set_var(EnvVars::SSL_CERT_FILE, "");
    }
    let (server_task, addr) = start_http_user_agent_server().await?;
    let url = DisplaySafeUrl::from_str(&format!("http://{addr}"))?;
    let cache = Cache::temp()?.init()?;
    let client = RegistryClientBuilder::new(BaseClientBuilder::default(), cache).build();
    let _ = client
        .cached_client()
        .uncached()
        .for_host(&url)
        .get(Url::from(url))
        .send()
        .await;
    let _ = server_task.await?;
    unsafe {
        std::env::remove_var(EnvVars::SSL_CERT_FILE);
    }

    // Check no warning was emitted
    let warning = uv_warnings::WARNINGS
        .lock()
        .expect("Failed to retrieve warnings")
        .iter()
        .any(|msg| msg.starts_with("Ignoring invalid `SSL_CERT_FILE`."));
    assert!(!warning, "Unexpected warning emitted");

    unsafe {
        std::env::set_var(EnvVars::SSL_CERT_FILE, "foo");
    }
    let (server_task, addr) = start_http_user_agent_server().await?;
    let url = DisplaySafeUrl::from_str(&format!("http://{addr}"))?;
    let cache = Cache::temp()?.init()?;
    let client = RegistryClientBuilder::new(BaseClientBuilder::default(), cache).build();
    let _ = client
        .cached_client()
        .uncached()
        .for_host(&url)
        .get(Url::from(url))
        .send()
        .await;
    let _ = server_task.await?;
    unsafe {
        std::env::remove_var(EnvVars::SSL_CERT_FILE);
    }

    // Check warning was emitted
    let warning = uv_warnings::WARNINGS
        .lock()
        .expect("Failed to retrieve warnings")
        .iter()
        .any(|msg| msg.starts_with("Ignoring invalid `SSL_CERT_FILE`."));
    assert!(warning, "Expected warning to be emitted");

    // Disable user-facing warnings
    uv_warnings::disable();

    Ok(())
}
```

Note, this would require you to use nextest to run these tests as you'll need process isolation.

---

_@samypr100 reviewed on 2025-11-20 02:57_

---

_@taearls reviewed on 2025-11-20 18:31_

---

_Review comment by @taearls on `crates/uv-client/tests/it/ssl_certs.rs`:216 on 2025-11-20 18:31_

thanks for the feedback! I see what you're saying, good catch. 

It seems like maybe a test isn't necessary for this, but I'd be happy to add the test you suggested if you'd like. 

for now I'll remove the test case I added.

---

_@samypr100 approved on 2025-11-21 01:13_

looks good to me

---

_Comment by @samypr100 on 2025-12-02 02:56_

@zanieb I believe this one is good to go

---

_Comment by @taearls on 2026-01-16 13:48_

@samypr100 hey! I just wanted to check in. Is there anything I can do to help move this forward?

---

_Comment by @zanieb on 2026-01-16 13:51_

Sorry! I lost track of this over the holidays. Can you rebase since this will conflict with https://github.com/astral-sh/uv/pull/17503 ?

---

_Comment by @taearls on 2026-01-16 19:37_

> Sorry! I lost track of this over the holidays. Can you rebase since this will conflict with https://github.com/astral-sh/uv/pull/17503 ?

No worries, totally understand! I'm happy to do it. I'm out of town this weekend, so it will be a few days before I rebase.


---
