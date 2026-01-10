---
number: 6185
title: "`uv-client` cannot be compiled"
type: issue
state: closed
author: fyusifov
labels:
  - question
assignees: []
created_at: 2024-08-18T19:38:32Z
updated_at: 2024-08-19T07:28:44Z
url: https://github.com/astral-sh/uv/issues/6185
synced_at: 2026-01-10T01:23:57Z
---

# `uv-client` cannot be compiled

---

_Issue opened by @fyusifov on 2024-08-18 19:38_

`uv-client` cannot be compiled with below error message:

```shell
   Compiling uv-client v0.0.1 (https://github.com/astral-sh/uv.git#615dda0e)
error[E0277]: the trait bound `reqwest_middleware::client::ClientWithMiddleware: From<ClientWithMiddleware>` is not satisfied
   --> /Users/fyusifov/.cargo/git/checkouts/uv-01b33d56c4bfa7c4/615dda0/crates/uv-client/src/registry_client.rs:574:21
    |
573 |                 let mut reader = AsyncHttpRangeReader::from_head_response(
    |                                  ---------------------------------------- required by a bound introduced by this call
574 |                     self.uncached_client().client(),
    |                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ the trait `From<ClientWithMiddleware>` is not implemented for `reqwest_middleware::client::ClientWithMiddleware`, which is required by `ClientWithMiddleware: Into<reqwest_middleware::client::ClientWithMiddleware>`
    |
    = help: the trait `From<reqwest::Client>` is implemented for `reqwest_middleware::client::ClientWithMiddleware`
    = help: for that trait implementation, expected `reqwest::Client`, found `ClientWithMiddleware`
    = note: required for `ClientWithMiddleware` to implement `Into<reqwest_middleware::client::ClientWithMiddleware>`
note: required by a bound in `AsyncHttpRangeReader::from_head_response`
   --> /Users/fyusifov/.cargo/registry/src/index.crates.io-6f17d22bba15001f/async_http_range_reader-0.8.0/src/lib.rs:302:22
    |
301 |     pub async fn from_head_response(
    |                  ------------------ required by a bound in this associated function
302 |         client: impl Into<reqwest_middleware::ClientWithMiddleware>,
    |                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ required by this bound in `AsyncHttpRangeReader::from_head_response`

error[E0277]: the trait bound `reqwest_middleware::client::ClientWithMiddleware: From<ClientWithMiddleware>` is not satisfied
   --> /Users/fyusifov/.cargo/git/checkouts/uv-01b33d56c4bfa7c4/615dda0/crates/uv-client/src/registry_client.rs:573:34
    |
573 |                   let mut reader = AsyncHttpRangeReader::from_head_response(
    |  __________________________________^
574 | |                     self.uncached_client().client(),
575 | |                     response,
576 | |                     url.clone(),
577 | |                     headers,
578 | |                 )
    | |_________________^ the trait `From<ClientWithMiddleware>` is not implemented for `reqwest_middleware::client::ClientWithMiddleware`, which is required by `ClientWithMiddleware: Into<reqwest_middleware::client::ClientWithMiddleware>`
    |
    = help: the trait `From<reqwest::Client>` is implemented for `reqwest_middleware::client::ClientWithMiddleware`
    = help: for that trait implementation, expected `reqwest::Client`, found `ClientWithMiddleware`
    = note: required for `ClientWithMiddleware` to implement `Into<reqwest_middleware::client::ClientWithMiddleware>`
note: required by a bound in `AsyncHttpRangeReader::from_head_response`
   --> /Users/fyusifov/.cargo/registry/src/index.crates.io-6f17d22bba15001f/async_http_range_reader-0.8.0/src/lib.rs:302:22
    |
301 |     pub async fn from_head_response(
    |                  ------------------ required by a bound in this associated function
302 |         client: impl Into<reqwest_middleware::ClientWithMiddleware>,
    |                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ required by this bound in `AsyncHttpRangeReader::from_head_response`

error[E0277]: the trait bound `reqwest_middleware::client::ClientWithMiddleware: From<ClientWithMiddleware>` is not satisfied
   --> /Users/fyusifov/.cargo/git/checkouts/uv-01b33d56c4bfa7c4/615dda0/crates/uv-client/src/registry_client.rs:579:18
    |
579 |                 .await
    |                  ^^^^^ the trait `From<ClientWithMiddleware>` is not implemented for `reqwest_middleware::client::ClientWithMiddleware`, which is required by `ClientWithMiddleware: Into<reqwest_middleware::client::ClientWithMiddleware>`
    |
    = help: the trait `From<reqwest::Client>` is implemented for `reqwest_middleware::client::ClientWithMiddleware`
    = help: for that trait implementation, expected `reqwest::Client`, found `ClientWithMiddleware`
    = note: required for `ClientWithMiddleware` to implement `Into<reqwest_middleware::client::ClientWithMiddleware>`
note: required by a bound in `AsyncHttpRangeReader::from_head_response`
   --> /Users/fyusifov/.cargo/registry/src/index.crates.io-6f17d22bba15001f/async_http_range_reader-0.8.0/src/lib.rs:302:22
    |
301 |     pub async fn from_head_response(
    |                  ------------------ required by a bound in this associated function
302 |         client: impl Into<reqwest_middleware::ClientWithMiddleware>,
    |                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ required by this bound in `AsyncHttpRangeReader::from_head_response`

For more information about this error, try `rustc --explain E0277`.
error: could not compile `uv-client` (lib) due to 3 previous errors
```

Steps to reproduce:

1. Create new project: `cargo new <project_name>`
2. Add `uv-client` as a project dependency: `cargo add uv-client --git https://github.com/astral-sh/uv.git`
3. Run the program: `cargo run`


---

_Comment by @charliermarsh on 2024-08-18 22:16_

I think you need to do something like this because we have a forks of these crates:

```toml
[dependencies]
uv-client = { git = "https://github.com/astral-sh/uv.git", rev = "615dda0e944f1daa65bbb8ee2f9d87a1fc914911" }

[patch.crates-io]
reqwest-middleware = { git = "https://github.com/astral-sh/reqwest-middleware", rev = "21ceec9a5fd2e8d6f71c3ea2999078fecbd13cbe" }
reqwest-retry = { git = "https://github.com/astral-sh/reqwest-middleware", rev = "21ceec9a5fd2e8d6f71c3ea2999078fecbd13cbe" }
```

---

_Closed by @charliermarsh on 2024-08-18 22:16_

---

_Label `question` added by @charliermarsh on 2024-08-18 22:16_

---

_Comment by @fyusifov on 2024-08-19 07:28_

Thanks, @charliermarsh. This worked. 

---

_Referenced in [astral-sh/uv#7689](../../astral-sh/uv/issues/7689.md) on 2024-09-25 18:26_

---
