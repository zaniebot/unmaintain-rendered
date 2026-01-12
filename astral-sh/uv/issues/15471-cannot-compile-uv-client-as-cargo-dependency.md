```yaml
number: 15471
title: "Cannot compile `uv-client` as cargo dependency"
type: issue
state: closed
author: felixmulder
labels:
  - bug
  - rustlib
assignees: []
created_at: 2025-08-23T16:07:20Z
updated_at: 2025-08-25T10:40:43Z
url: https://github.com/astral-sh/uv/issues/15471
synced_at: 2026-01-12T16:02:11Z
```

# Cannot compile `uv-client` as cargo dependency

---

_@felixmulder_

### Summary

We're running a project that needs to invoke `uv` programmatically (we don't expect any API stability, don't worry). However, I'm a bit confused as to why I can't build the project at all as a dependency.

Minimal repro, `cargo init uv-repro && cd uv-repro` with:

```toml
# Cargo.toml
[package]
name = "uv-repro"
version = "0.1.0"
edition = "2024"

[dependencies]
uv-client = { git = "https://github.com/astral-sh/uv", rev = "088c908cda450d63db838ab41620d25c34a8d1fa" }
```

```shell
$ cargo build
```

Output:

```
   Compiling uv-client v0.0.1 (https://github.com/astral-sh/uv#f1647838)
error[E0277]: the trait bound `reqwest_middleware::client::ClientWithMiddleware: From<RedirectClientWithMiddleware>` is not satisfied
   --> /Users/x/.cargo/git/checkouts/uv-c9e40703e19509a8/f164783/crates/uv-client/src/registry_client.rs:937:25
    |
936 |                     let mut reader = AsyncHttpRangeReader::from_head_response(
    |                                      ---------------------------------------- required by a bound introduced by this call
937 |                         self.uncached_client(url).clone(),
    |                         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ the trait `From<RedirectClientWithMiddleware>` is not implemented for `reqwest_middleware::client::ClientWithMiddleware`
    |
    = help: the trait `From<RedirectClientWithMiddleware>` is not implemented for `reqwest_middleware::client::ClientWithMiddleware`
            but trait `From<reqwest::Client>` is implemented for it
    = help: for that trait implementation, expected `reqwest::Client`, found `RedirectClientWithMiddleware`
    = note: required for `RedirectClientWithMiddleware` to implement `Into<reqwest_middleware::client::ClientWithMiddleware>`
note: required by a bound in `AsyncHttpRangeReader::from_head_response`
   --> /Users/x/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/async_http_range_reader-0.9.1/src/lib.rs:304:22
    |
303 |     pub async fn from_head_response(
    |                  ------------------ required by a bound in this associated function
304 |         client: impl Into<reqwest_middleware::ClientWithMiddleware>,
    |                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ required by this bound in `AsyncHttpRangeReader::from_head_response`

error[E0277]: the trait bound `reqwest_middleware::client::ClientWithMiddleware: From<RedirectClientWithMiddleware>` is not satisfied
   --> /Users/x/.cargo/git/checkouts/uv-c9e40703e19509a8/f164783/crates/uv-client/src/registry_client.rs:936:38
    |
936 |                       let mut reader = AsyncHttpRangeReader::from_head_response(
    |  ______________________________________^
937 | |                         self.uncached_client(url).clone(),
938 | |                         response,
939 | |                         Url::from(url.clone()),
940 | |                         headers.clone(),
941 | |                     )
    | |_____________________^ the trait `From<RedirectClientWithMiddleware>` is not implemented for `reqwest_middleware::client::ClientWithMiddleware`
    |
    = help: the trait `From<RedirectClientWithMiddleware>` is not implemented for `reqwest_middleware::client::ClientWithMiddleware`
            but trait `From<reqwest::Client>` is implemented for it
    = help: for that trait implementation, expected `reqwest::Client`, found `RedirectClientWithMiddleware`
    = note: required for `RedirectClientWithMiddleware` to implement `Into<reqwest_middleware::client::ClientWithMiddleware>`
note: required by a bound in `AsyncHttpRangeReader::from_head_response`
   --> /Users/x/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/async_http_range_reader-0.9.1/src/lib.rs:304:22
    |
303 |     pub async fn from_head_response(
    |                  ------------------ required by a bound in this associated function
304 |         client: impl Into<reqwest_middleware::ClientWithMiddleware>,
    |                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ required by this bound in `AsyncHttpRangeReader::from_head_response`

error[E0277]: the trait bound `reqwest_middleware::client::ClientWithMiddleware: From<RedirectClientWithMiddleware>` is not satisfied
   --> /Users/x/.cargo/git/checkouts/uv-c9e40703e19509a8/f164783/crates/uv-client/src/registry_client.rs:942:22
    |
942 |                     .await
    |                      ^^^^^ the trait `From<RedirectClientWithMiddleware>` is not implemented for `reqwest_middleware::client::ClientWithMiddleware`
    |
    = help: the trait `From<RedirectClientWithMiddleware>` is not implemented for `reqwest_middleware::client::ClientWithMiddleware`
            but trait `From<reqwest::Client>` is implemented for it
    = help: for that trait implementation, expected `reqwest::Client`, found `RedirectClientWithMiddleware`
    = note: required for `RedirectClientWithMiddleware` to implement `Into<reqwest_middleware::client::ClientWithMiddleware>`
note: required by a bound in `AsyncHttpRangeReader::from_head_response`
   --> /Users/x/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/async_http_range_reader-0.9.1/src/lib.rs:304:22
    |
303 |     pub async fn from_head_response(
    |                  ------------------ required by a bound in this associated function
304 |         client: impl Into<reqwest_middleware::ClientWithMiddleware>,
    |                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ required by this bound in `AsyncHttpRangeReader::from_head_response`

For more information about this error, try `rustc --explain E0277`.
error: could not compile `uv-client` (lib) due to 3 previous errors
```

I'm not certain if there is anything specific to the Cargo build that makes it work within the uv repo itself -- in the uv repo at the same revision, I'm able to build the project. Both with `rustc` 1.87 and 1.89.

Cheers,
Felix

### Platform

macOS 15.6.1

### Version

088c908cda450d63db838ab41620d25c34a8d1fa

### Python version

-

---

_Label `bug` added by @felixmulder on 2025-08-23 16:07_

---

_Comment by @zanieb on 2025-08-23 18:50_

Hm the supposedly missing implementation is at https://github.com/astral-sh/uv/blob/4bc6c77f022a55cff3c8c0b3251ce419c372017c/crates/uv-client/src/base_client.rs#L659-L663



---

_Label `rustlib` added by @zanieb on 2025-08-23 18:50_

---

_Comment by @felixmulder on 2025-08-24 07:49_

It's pretty strange, the lockfile in the repro gets multiple versions of the `reqwest-middleware` library:

```toml
[[package]]
name = "reqwest-middleware"
version = "0.4.2"
source = "registry+https://github.com/rust-lang/crates.io-index"
checksum = "57f17d28a6e6acfe1733fe24bcd30774d13bffa4b8a22535b4c8c98423088d4e"
dependencies = [
 "anyhow",
 "async-trait",
 "http",
 "reqwest",
 "serde",
 "thiserror 1.0.69",
 "tower-service",
]

[[package]]
name = "reqwest-middleware"
version = "0.4.2"
source = "git+https://github.com/astral-sh/reqwest-middleware?rev=ad8b9d332d1773fde8b4cd008486de5973e0a3f8#ad8b9d332d1773fde8b4cd008486de5973e0a3f8"
dependencies = [
 "anyhow",
 "async-trait",
 "http",
 "reqwest",
 "serde",
 "thiserror 1.0.69",
 "tower-service",
]
```

I guess the error message is somewhat confusing. I think it has two variants of the library on path when trying to compile 🤔 

Hmm, yes adding:

```toml
[patch.crates-io]
reqwest-middleware = { git = "https://github.com/astral-sh/reqwest-middleware", rev = "ad8b9d332d1773fde8b4cd008486de5973e0a3f8" }
reqwest-retry = { git = "https://github.com/astral-sh/reqwest-middleware", rev = "ad8b9d332d1773fde8b4cd008486de5973e0a3f8" }
```

to `Cargo.toml` in `uv-repro` fixes this. I guess all the forking confuses Cargo 😅 

---

_Comment by @charliermarsh on 2025-08-24 12:14_

Ah yeah, that's a bummer. Hopefully we can get off those forks soon.

---

_Closed by @charliermarsh on 2025-08-24 12:14_

---

_Comment by @felixmulder on 2025-08-24 18:31_

That would be pretty cool! I'll see if I can find some cycles to contribute it back at some point. Thanks for all the tools ☺️ 

---

_Comment by @konstin on 2025-08-25 10:40_

For the reqweest-middleware dependency, the cause we're on a fork is https://github.com/TrueLayer/reqwest-middleware/pull/235, we need get that PR reviewed and merged to get of the fork.

---
