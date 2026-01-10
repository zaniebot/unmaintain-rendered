```yaml
number: 2817
title: "Upgrade `reqwest` to v0.12.3"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/reqwest
created_at: 2024-04-04T01:54:21Z
updated_at: 2024-04-10T15:20:45Z
url: https://github.com/astral-sh/uv/pull/2817
synced_at: 2026-01-10T14:43:31Z
```

# Upgrade `reqwest` to v0.12.3

---

_Pull request opened by @charliermarsh on 2024-04-04 01:54_

## Summary

Closes #2814.


---

_Label `internal` added by @charliermarsh on 2024-04-04 01:54_

---

_@charliermarsh reviewed on 2024-04-04 02:05_

---

_Review comment by @charliermarsh on `crates/uv-client/src/base_client.rs`:144 on 2024-04-04 02:05_

This is a big win. We can remove all the custom TLS logic because they added granular cert settings to the builder (below).

---

_Marked ready for review by @charliermarsh on 2024-04-04 02:05_

---

_Comment by @charliermarsh on 2024-04-04 02:07_

Oh, also need to look at https://github.com/astral-sh/uv/issues/2814#issuecomment-2035914083.

---

_@zanieb reviewed on 2024-04-04 02:12_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:144 on 2024-04-04 02:12_

Is it granular enough for #1339?

---

_@charliermarsh reviewed on 2024-04-04 02:17_

---

_Review comment by @charliermarsh on `crates/uv-client/src/base_client.rs`:144 on 2024-04-04 02:17_

The only change I'm referencing here is that you can selectively enable native certs or the bundled certs, rather than having to enable _both_ or none.

---

_@charliermarsh reviewed on 2024-04-04 02:20_

---

_Review comment by @charliermarsh on `crates/uv-client/src/base_client.rs`:144 on 2024-04-04 02:20_

(So, I'm not sure.)

---

_@zanieb reviewed on 2024-04-04 02:24_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:144 on 2024-04-04 02:24_

Ah okay nevermind

---

_@samypr100 reviewed on 2024-04-04 11:45_

---

_Review comment by @samypr100 on `crates/uv-client/tests/user_agent_version.rs`:72 on 2024-04-04 11:45_

```suggestion
    // Wait for the server task to complete, to be a good citizen.
    server_task.await?;

    Ok(())
}

#[tokio::test]
async fn test_user_agent_has_linehaul() -> Result<()> {
    // Set up the TCP listener on a random available port
    let listener = TcpListener::bind("127.0.0.1:0").await?;
    let addr = listener.local_addr()?;

    // Spawn the server loop in a background task
    let server_task = tokio::spawn(async move {
        let svc = service_fn(move |req: Request<hyper::body::Incoming>| {
            // Get User Agent Header and send it back in the response
            let user_agent = req
                .headers()
                .get(USER_AGENT)
                .and_then(|v| v.to_str().ok())
                .map(|s| s.to_string())
                .unwrap_or_default(); // Empty Default
            future::ok::<_, hyper::Error>(Response::new(Full::new(Bytes::from(user_agent))))
        });
        // Start Server (not wrapped in loop {} since we want a single response server)
        // If you want server to accept multiple connections, wrap it in loop {}
        let (socket, _) = listener.accept().await.unwrap();
        let socket = TokioIo::new(socket);
        tokio::task::spawn(async move {
            http1::Builder::new()
                .serve_connection(socket, svc)
                .with_upgrades()
                .await
                .expect("Server Started");
        });
    });

    // Add some representative markers for an Ubuntu CI runner
    let markers = MarkerEnvironment {
        implementation_name: "cpython".to_string(),
        implementation_version: StringVersion {
            string: "3.12.2".to_string(),
            version: "3.12.2".parse()?,
        },
        os_name: "posix".to_string(),
        platform_machine: "x86_64".to_string(),
        platform_python_implementation: "CPython".to_string(),
        platform_release: "6.5.0-1016-azure".to_string(),
        platform_system: "Linux".to_string(),
        platform_version: "#16~22.04.1-Ubuntu SMP Fri Feb 16 15:42:02 UTC 2024".to_string(),
        python_full_version: StringVersion {
            string: "3.12.2".to_string(),
            version: "3.12.2".parse()?,
        },
        python_version: StringVersion {
            string: "3.12".to_string(),
            version: "3.12".parse()?,
        },
        sys_platform: "linux".to_string(),
    };

    // Initialize uv-client
    let cache = Cache::temp()?;
    let mut builder = RegistryClientBuilder::new(cache).markers(&markers);

    let linux = Platform::new(
        Os::Manylinux {
            major: 2,
            minor: 38,
        },
        Arch::X86_64,
    );
    let macos = Platform::new(
        Os::Macos {
            major: 14,
            minor: 4,
        },
        Arch::Aarch64,
    );
    if cfg!(target_os = "linux") {
        builder = builder.platform(&linux);
    } else if cfg!(target_os = "macos") {
        builder = builder.platform(&macos);
    }
    let client = builder.build();

    // Send request to our dummy server
    let res = client
        .cached_client()
        .uncached()
        .get(format!("http://{addr}"))
        .send()
        .await?;

    // Check the HTTP status
    assert!(res.status().is_success());

    // Check User Agent
    let body = res.text().await?;

    // Wait for the server task to complete, to be a good citizen.
    server_task.await?;

    // Unpack User-Agent with linehaul
    let (uv_version, uv_linehaul) = body
        .split_once(' ')
        .expect("Failed to split User-Agent header.");

    // Deserializing Linehaul
    let linehaul: LineHaul = serde_json::from_str(uv_linehaul)?;

    // Assert uv version
    assert_eq!(uv_version, format!("uv/{}", version()));

    // Assert linehaul
    let installer_info = linehaul.installer.unwrap();
    let system_info = linehaul.system.unwrap();
    let impl_info = linehaul.implementation.unwrap();

    assert_eq!(installer_info.name.unwrap(), "uv".to_string());
    assert_eq!(installer_info.version.unwrap(), version());

    assert_eq!(system_info.name.unwrap(), markers.platform_system);
    assert_eq!(system_info.release.unwrap(), markers.platform_release);

    assert_eq!(
        impl_info.name.unwrap(),
        markers.platform_python_implementation
    );
    assert_eq!(
        impl_info.version.unwrap(),
        markers.python_full_version.version.to_string()
    );

    assert_eq!(
        linehaul.python.unwrap(),
        markers.python_full_version.version.to_string()
    );
    assert_eq!(linehaul.cpu.unwrap(), markers.platform_machine);

    assert_eq!(linehaul.openssl_version, None);
    assert_eq!(linehaul.setuptools_version, None);
    assert_eq!(linehaul.rustc_version, None);

    if cfg!(windows) {
        assert_eq!(linehaul.distro, None);
    } else if cfg!(target_os = "linux") {
        // Using `os_info` to confirm our values are as expected in Linux
        let info = os_info::get();
        let Some(distro_info) = linehaul.distro else {
            panic!("got no distro, but expected one in linehaul")
        };
        assert_eq!(distro_info.id.as_deref(), info.codename());
        if let Some(ref name) = distro_info.name {
            assert_eq!(name, &info.os_type().to_string());
        }
        if let Some(ref version) = distro_info.version {
            assert_eq!(version, &info.version().to_string());
        }
        assert!(distro_info.libc.is_some());
    } else if cfg!(target_os = "macos") {
        // We mock the macOS version
        let distro_info = linehaul.distro.unwrap();
        assert_eq!(distro_info.id, None);
        assert_eq!(distro_info.name.unwrap(), "macOS");
        assert_eq!(distro_info.version, Some("14.4".to_string()));
        assert_eq!(distro_info.libc, None);
    }
```

This re-adds back the linehaul test below using the new hyper changes.

These are the imports as well

```
use pep508_rs::{MarkerEnvironment, StringVersion};
use platform_tags::{Arch, Os, Platform};
use uv_client::LineHaul;
```

---

_@charliermarsh reviewed on 2024-04-05 22:03_

---

_Review comment by @charliermarsh on `Cargo.toml`:153 on 2024-04-05 22:03_

(No longer necessary, 0.12.3 was released.)

---

_@charliermarsh reviewed on 2024-04-05 23:45_

---

_Review comment by @charliermarsh on `crates/uv-client/tests/user_agent_version.rs`:72 on 2024-04-05 23:45_

Thx!

---

_@samypr100 reviewed on 2024-04-06 01:40_

---

_Review comment by @samypr100 on `crates/uv-client/Cargo.toml`:57 on 2024-04-06 01:40_

@charliermarsh I noticed this got updated to `3.8.2` in the lock file, causing the error in ubuntu. This should probably be switched to `os_info = { version = "=3.7.0", default-features = false }` in the mean time as 3.8 has had some breaking changes.

---

_@samypr100 reviewed on 2024-04-06 01:45_

---

_Review comment by @samypr100 on `crates/uv-client/Cargo.toml`:57 on 2024-04-06 01:45_

I'll do a follow-on PR after this to improve these tests and hopefully remove this dependency.

---

_@charliermarsh reviewed on 2024-04-06 01:47_

---

_Review comment by @charliermarsh on `crates/uv-client/Cargo.toml`:57 on 2024-04-06 01:47_

Thank you so much.

---

_@charliermarsh reviewed on 2024-04-06 01:48_

---

_Review comment by @charliermarsh on `crates/uv-client/Cargo.toml`:57 on 2024-04-06 01:48_

Updated...

---

_@samypr100 reviewed on 2024-04-06 02:29_

---

_Review comment by @samypr100 on `crates/uv-client/Cargo.toml`:57 on 2024-04-06 02:29_

Awesome, tests passing 🎉

---

_Comment by @zanieb on 2024-04-06 15:02_

Let me know if you want my review.

---

_Comment by @charliermarsh on 2024-04-06 15:03_

I'm waiting on a reqwest-middleware release.

---

_Renamed from "Upgrade `reqwest` to v0.12.2" to "Upgrade `reqwest` to v0.12.3" by @charliermarsh on 2024-04-10 14:59_

---

_Merged by @charliermarsh on 2024-04-10 15:20_

---

_Closed by @charliermarsh on 2024-04-10 15:20_

---

_Branch deleted on 2024-04-10 15:20_

---
