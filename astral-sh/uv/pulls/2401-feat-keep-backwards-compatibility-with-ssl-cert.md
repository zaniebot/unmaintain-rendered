```yaml
number: 2401
title: "feat: keep backwards compatibility with `SSL_CERT_FILE` without requiring `--native-tls`"
type: pull_request
state: merged
author: samypr100
labels:
  - enhancement
assignees: []
merged: true
base: main
head: backwards-compat-ssl-cert-file
created_at: 2024-03-13T02:02:43Z
updated_at: 2024-03-13T12:54:23Z
url: https://github.com/astral-sh/uv/pull/2401
synced_at: 2026-01-10T14:49:08Z
```

# feat: keep backwards compatibility with `SSL_CERT_FILE` without requiring `--native-tls`

---

_Pull request opened by @samypr100 on 2024-03-13 02:02_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Small follow up to https://github.com/astral-sh/uv/pull/2362 to check if `SSL_CERT_FILE` is set to enable `--native-tls` functionality. This maintains backwards compatibility with `0.1.17` and below users leveraging only `SSL_CERT_FILE`.

Closes https://github.com/astral-sh/uv/issues/2400

## Test Plan

<!-- How was it tested? -->
Assuming `SSL_CERT_FILE` is already working via `--native-tls`, this is simply a shortcut to enable `--native-tls` functionality implicitly while still being able to let `rustls-native-certs` handle the loading of `SSL_CERT_FILE` instead of ourselves.

Edit: Manually tested by setting up own self-signed CA certificate bundle and set `SSL_CERT_FILE` to this and confirmed the loading happens without having to specify `--native-tls`.

---

_Renamed from "feat: keep backwards compatibility with `SSL_CERT_FILE` without requiring --native-tls" to "feat: keep backwards compatibility with `SSL_CERT_FILE` without requiring `--native-tls`" by @samypr100 on 2024-03-13 02:07_

---

_Marked ready for review by @samypr100 on 2024-03-13 02:08_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:123 on 2024-03-13 02:15_

Should we instead change `tls::load` to add a certificate based on `SSL_CERT_FILE`? I think we would just call `root_cert_store.add` with the result of this: https://github.com/astral-sh/uv/pull/2351/files#diff-dcc1f69d132524500a0dcf217aa6db521517536daaf9364f3085961626fe722cR120

---

_@charliermarsh reviewed on 2024-03-13 02:15_

---

_@samypr100 reviewed on 2024-03-13 02:38_

---

_Review comment by @samypr100 on `crates/uv-client/src/registry_client.rs`:123 on 2024-03-13 02:38_

I was thinking more towards delegating the parsing of `SSL_CERT_FILE` to [rustls-native-certs](https://github.com/rustls/rustls-native-certs/blob/main/src/lib.rs#L73) as we already do when we combine (`--native-tls` with `SSL_CERT_FILE`).

The `SSL_CERT_FILE` is actually a CA bundle, meaning it can contain more than one cert. I believe we'd have to load each entry, get a vector of Certificates, and them add them to the builder one by one and handle potential incompatible cert errors along the way (we get this for free with rustls-native-certs).
I did see an API reqwest to [load a pem bundle](https://docs.rs/reqwest/latest/reqwest/tls/struct.Certificate.html#method.from_pem_bundle), but didn't see one to add them all at once, so we'd need to call `add_root_certificate` for each one in a loop as we build client.

Sidenote, I've just tested this current PR with creating my own self-signed CA's and set `SSL_CERT_FILE` to this and can confirm this solution would effectively delegate the parsing of `SSL_CERT_FILE` to `rustls-native-certs` properly (as we already do with the combination of `--native-tls` with `SSL_CERT_FILE`)

---

_@samypr100 reviewed on 2024-03-13 02:53_

---

_Review comment by @samypr100 on `crates/uv-client/src/registry_client.rs`:123 on 2024-03-13 02:53_

If we're concerned with a user providing an invalid `SSL_CERT_FILE` path, how about a nice warning?

```rust
            let ssl_cert_file_exists = env::var_os("SSL_CERT_FILE")
                .map_or(false, |path| {
                    let path_exists = Path::new(&path).exists();
                    if !path_exists {
                        warn_user_once!("Ignoring invalid value from environment for SSL_CERT_FILE. File path {} does not exists.", path.to_string_lossy());
                    }
                    path_exists
                });

            let tls = tls::load(if self.native_tls || ssl_cert_file_exists {
                Roots::Native
            } else {
                Roots::Webpki
            })
```

---

_Comment by @zanieb on 2024-03-13 02:53_

Thanks! I was going to flag this too when looking at the changelog — I think this is a very reasonable behavior.

---

_Label `enhancement` added by @zanieb on 2024-03-13 02:53_

---

_@zanieb reviewed on 2024-03-13 02:55_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:123 on 2024-03-13 02:55_

I think that seems reasonable.

---

_@samypr100 reviewed on 2024-03-13 03:05_

---

_Review comment by @samypr100 on `crates/uv-client/src/registry_client.rs`:123 on 2024-03-13 03:05_

Added in 85d92e8

---

_@charliermarsh reviewed on 2024-03-13 03:13_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:124 on 2024-03-13 03:13_

Nit: `is_some_and`

---

_@charliermarsh reviewed on 2024-03-13 03:14_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:127 on 2024-03-13 03:14_

Nit: `path.simplified_display()`

---

_@charliermarsh approved on 2024-03-13 04:22_

Thanks!

---

_Merged by @charliermarsh on 2024-03-13 04:33_

---

_Closed by @charliermarsh on 2024-03-13 04:33_

---

_Branch deleted on 2024-03-13 12:54_

---
