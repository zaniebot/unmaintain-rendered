```yaml
number: 17427
title: "Upgrading `reqwest` to `0.13.1` to improve TLS experience"
type: issue
state: open
author: salmonsd
labels:
  - enhancement
assignees: []
created_at: 2026-01-12T22:36:00Z
updated_at: 2026-01-12T23:10:53Z
url: https://github.com/astral-sh/uv/issues/17427
synced_at: 2026-01-12T23:24:19Z
```

# Upgrading `reqwest` to `0.13.1` to improve TLS experience

---

_@salmonsd_

### Summary

It has been well documented that many end users have experienced issues with `--native-tls` not working as expected, especially when it comes to using `uv` with corporate CAs. And using `allow-insecure-host` is not a great option long-term for security reasons.

By upgrading `reqwest` to `0.13.1`, we can take advantage of using `rustls` as the default backend and utilize `rustls-platform-verifier` instead of `rustls-native-certs`, which allows us to use more cert signatures supported through `aws-lc` instead of _ring_. We can still include and use `native-tls` to preserve existing functionality, but this would eliminate the `SSL_CERT_DIR` and `SSL_CERT_FILE` env var needs for many end users, provided certs are included in their OS certificate store. 

The following updates would need to be made to update `reqwest` to `0.13.1` along with updating necessary and/or changed features:
- [astral-sh/reqwest-middleware](https://github.com/astral-sh/reqwest-middleware) (merge updates from TrueLayer/reqwest-middleware)
  - `astral-reqwest-middleware` --> update to `0.5.0`
  - `astral-reqwest-tracing` --> update to `0.6.0`
  - `astral-reqwest-retry` --> update to `0.9.0`
- [astral-sh/ambient-id](https://github.com/astral-sh/ambient-id)
- [astral-sh/async_http_range_reader](https://github.com/astral-sh/async_http_range_reader)

Main updates for `uv/Cargo.toml` (ambient-id, and dev-dependencies excluded):
```toml
reqwest = { version = "0.13.1", default-features = false, features = ["json", "gzip", "deflate", "zstd", "stream", "system-proxy", "rustls", "socks", "multipart", "http2", "blocking", "query", "form", "native-tls"] }
reqwest-middleware = { version = "0.5.0", package = "astral-reqwest-middleware", features = ["multipart", "json", "query", "form"] }
reqwest-retry = { version = "0.9.0", package = "astral-reqwest-retry" }
```

I was able to build and patch locally and was able to successfully run `uv` to work with my Corporate Cert.

Related:

- https://github.com/rustls/rustls-native-certs/tree/main
> Instead of this crate, we suggest using rustls-platform-verifier which provides a more robust solution with a simpler API. This crate is still maintained, but mostly for use inside the platform verifier on platforms where no other solution is available. For more context, see deployment considerations.
- https://github.com/astral-sh/uv/issues/11595
- https://github.com/astral-sh/uv/issues/9243
- https://github.com/astral-sh/uv/issues/4534
- https://github.com/astral-sh/uv/issues/16360
- https://github.com/astral-sh/uv/issues/17355
- https://github.com/astral-sh/uv/issues/16412
- https://github.com/rustls/rustls/issues/2825
- https://github.com/seanmonstar/reqwest/pull/2915/changes

### Example

These changes would only affect the `uv-client` crate with the subsequent changes.

- remove all calls for `built_in_root_certs` no longer available from `reqwest`
- `uv/crates/uv-client/src/base_client.rs`

```rust
    fn create_client(
        &self,
        user_agent: &str,
        timeout: Duration,
        _ssl_cert_file_exists: bool,
        _ssl_cert_dir_exists: bool,
        security: Security,
        redirect_policy: RedirectPolicy,
    ) -> Client {
        // Configure the builder.
        let client_builder = ClientBuilder::new()
            .http1_title_case_headers()
            .user_agent(user_agent)
            .pool_max_idle_per_host(20)
            .read_timeout(timeout)
            .redirect(redirect_policy.reqwest_policy());

        // If necessary, accept invalid certificates.
        let client_builder = match security {
            Security::Secure => client_builder,
            Security::Insecure => client_builder.danger_accept_invalid_certs(true),
        };

        // Note: SSL_CERT_FILE/SSL_CERT_DIR are NOT supported by rustls-platform-verifier.
        // Users needing custom certificates should either:
        //   1. Install certificates in OS certificate store (recommended)
        //   2. Use --native-tls flag on Linux (uses OpenSSL which supports SSL_CERT_*)
        let client_builder = if self.native_tls {
            client_builder.tls_backend_native()
        } else {
            client_builder.tls_backend_rustls()
        };
```


---

_Label `enhancement` added by @salmonsd on 2026-01-12 22:36_

---

_Comment by @zanieb on 2026-01-12 23:09_

Per your code comment, this would break support for `SSL_CERT_FILE` and `SSL_CERT_DIR`?

---

_Comment by @zanieb on 2026-01-12 23:10_

This would also break the functionality added in https://github.com/astral-sh/uv/pull/14816? cc @nilskch

---
