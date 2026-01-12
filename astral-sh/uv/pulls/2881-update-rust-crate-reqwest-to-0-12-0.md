```yaml
number: 2881
title: Update Rust crate reqwest to 0.12.0
type: pull_request
state: closed
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/reqwest-0.x
created_at: 2024-04-08T03:04:53Z
updated_at: 2024-04-08T03:55:48Z
url: https://github.com/astral-sh/uv/pull/2881
synced_at: 2026-01-12T16:05:17Z
```

# Update Rust crate reqwest to 0.12.0

---

_@renovate_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [reqwest](https://togithub.com/seanmonstar/reqwest) | dev-dependencies | minor | `0.11.23` -> `0.12.0` |
| [reqwest](https://togithub.com/seanmonstar/reqwest) | workspace.dependencies | minor | `0.11.23` -> `0.12.0` |

---

### Release Notes

<details>
<summary>seanmonstar/reqwest (reqwest)</summary>

### [`v0.12.3`](https://togithub.com/seanmonstar/reqwest/blob/HEAD/CHANGELOG.md#v0123)

[Compare Source](https://togithub.com/seanmonstar/reqwest/compare/v0.12.2...v0.12.3)

-   Add `FromStr` for `dns::Name`.
-   Add `ClientBuilder::built_in_webpki_certs(bool)` to enable them separately.
-   Add `ClientBuilder::built_in_native_certs(bool)` to enable them separately.
-   Fix sending `content-length: 0` for GET requests.
-   Fix response body `content_length()` to return value when timeout is configured.
-   Fix `ClientBuilder::resolve()` to use lowercase domain names.

### [`v0.12.2`](https://togithub.com/seanmonstar/reqwest/blob/HEAD/CHANGELOG.md#v0122)

[Compare Source](https://togithub.com/seanmonstar/reqwest/compare/v0.12.1...v0.12.2)

-   Fix missing ALPN when connecting to socks5 proxy with rustls.
-   Fix TLS version limits with rustls.
-   Fix not detected ALPN h2 from server with native-tls.

### [`v0.12.1`](https://togithub.com/seanmonstar/reqwest/blob/HEAD/CHANGELOG.md#v0121)

[Compare Source](https://togithub.com/seanmonstar/reqwest/compare/v0.12.0...v0.12.1)

-   Fix `ClientBuilder::interface()` when no TLS is enabled.
-   Fix `TlsInfo::peer_certificate()` being truncated with rustls.
-   Fix panic if `http2` feature disabled but TLS negotiated h2 in ALPN.
-   Fix `Display` for `Error` to not include its source error.

### [`v0.12.0`](https://togithub.com/seanmonstar/reqwest/blob/HEAD/CHANGELOG.md#v0120)

[Compare Source](https://togithub.com/seanmonstar/reqwest/compare/v0.11.27...v0.12.0)

-   Upgrade to `hyper`, `http`, and `http-body` v1.
-   Add better support for converting to and from `http::Request` and `http::Response`.
-   Add `http2` optional cargo feature, default on.
-   Add `charset` optional cargo feature, default on.
-   Add `macos-system-configuration` cargo feature, default on.
-   Change all optional dependencies to no longer be exposed as implicit features.
-   Add `ClientBuilder::interface(str)` to specify the local interface to bind to.
-   Experimental: disables the `http3` feature temporarily.

#### v0.11.27

-   Add `hickory-dns` feature, deprecating `trust-dns`.
-   (wasm) Fix `Form::text()` to not set octet-stream for plain text fields.

#### v0.11.26

-   Revert `system-configuration` upgrade, which broke MSRV on macOS.

#### v0.11.25

-   Fix `Certificate::from_pem_bundle()` parsing.
-   Fix Apple linker errors from detecting system proxies.

#### v0.11.24

-   Add `Certificate::from_pem_bundle()` to add a bundle.
-   Add `http3_prior_knowledge()` to blocking client builder.
-   Remove `Sync` bounds requirement for `Body::wrap_stream()`.
-   Fix HTTP/2 to retry `REFUSED_STREAM` requests.
-   Fix instances of converting `Url` to `Uri` that could panic.

#### v0.11.23

-   Add `Proxy::custom_http_auth(val)` for setting the raw `Proxy-Authorization` header when connecting to proxies.
-   Fix redirect to reject locations that are not `http://` or `https://`.
-   Fix setting `nodelay` when TLS is enabled but URL is HTTP.
-   (wasm) Add `ClientBuilder::user_agent(val)`.
-   (wasm) add `multipart::Form::headers(headers)`.

#### v0.11.22

-   Fix compilation on Windows when `trust-dns` is enabled.

#### v0.11.21

-   Add automatically detecting macOS proxy settings.
-   Add `ClientBuilder::tls_info(bool)`, which will put `tls::TlsInfo` into the response extensions.
-   Fix trust-dns resolver from possible hangs.
-   Fix connect timeout to be split among multiple IP addresses.

#### v0.11.20

-   Fix `deflate` decompression back to using zlib, as outlined in the spec.

#### v0.11.19

-   Add `ClientBuilder::http1_ignore_invalid_headers_in_responses()` option.
-   Add `ClientBuilder::http1_allow_spaces_after_header_name_in_responses()` option.
-   Add support for `ALL_PROXY` environment variable.
-   Add support for `use_preconfigured_tls` when combined with HTTP/3.
-   Fix `deflate` decompression from using the zlib decoder.
-   Fix `Response::{text, text_with_charset}()` to strip BOM characters.
-   Fix a panic when HTTP/3 is used if UDP isn't able to connect.
-   Fix some dependencies for HTTP/3.
-   Increase MSRV to 1.63.

#### v0.11.18

-   Fix `RequestBuilder::json()` method from overriding a previously set `content-type` header. An existing value will be left in place.
-   Upgrade internal dependencies for rustls and compression.

#### v0.11.17

-   Upgrade internal dependencies of Experimental HTTP/3 to use quinn v0.9
-   (wasm) Fix blob url support

#### v0.11.16

-   Chore: set MSRV in `Cargo.toml`.
-   Docs: fix build on docs.rs

#### v0.11.15

-   Add `RequestBuilder` methods to split and reconstruct from its parts.
-   Add experimental HTTP/3 support.
-   Fix `connection_verbose` to log `write_vectored` calls.
-   (wasm) Make requests actually cancel if the future is dropped.

#### v0.11.14

-   Adds `Proxy::no_proxy(url)` that works like the NO_PROXY environment variable.
-   Adds `multipart::Part::headers(headers)` method to add custom headers.
-   (wasm) Add `Response::bytes_stream()`.
-   Perf: several internal optimizations reducing copies and memory allocations.

#### v0.11.13

-   Add `ClientBuilder::dns_resolver()` option for custom DNS resolvers.
-   Add `ClientBuilder::tls_sni(bool)` option to enable or disable TLS Server Name Indication.
-   Add `Identity::from_pkcs8_pem()` constructor when using `native-tls`.
-   Fix `redirect::Policy::limited(0)` from following any redirects.

#### v0.11.12

-   Add `ClientBuilder::resolve_to_addrs()` which allows a slice of IP addresses to be specified for a single host.
-   Add `Response::upgrade()` to await whether the server agrees to an HTTP upgrade.

#### v0.11.11

-   Add HTTP/2 keep-alive configuration methods on `ClientBuilder`.
-   Add `ClientBuilder::http1_allow_obsolete_multiline_headers_in_responses()`.
-   Add `impl Service<Request>` for `Client` and `&'_ Client`.
-   (wasm) Add `RequestBuilder::basic_auth()`.
-   Fix `RequestBuilder::header` to not override `sensitive` if user explicitly set on a `HeaderValue`.
-   Fix rustls parsing of elliptic curve private keys.
-   Fix Proxy URL parsing of some invalid targets.

#### v0.11.10

-   Add `Error::url()` to access the URL of an error.
-   Add `Response::extensions()` to access the `http::Extensions` of a response.
-   Fix `rustls-native-certs` to log an error instead of panicking when loading an invalid system certificate.
-   Fix passing Basic Authorization header to proxies.

#### v0.11.9

-   Add `ClientBuilder::http09_responses(bool)` option to allow receiving HTTP/0.9 responses.
-   Fix HTTP/2 to retry requests interrupted by an HTTP/2 graceful shutdown.
-   Fix proxy loading from environment variables to ignore empty values.

#### v0.11.8

-   Update internal webpki-roots dependency.

#### v0.11.7

-   Add `blocking::ClientBuilder::resolve()` option, matching the async builder.
-   Implement `From<tokio::fs::File>` for `Body`.
-   Fix `blocking` request-scoped timeout applying to bodies as well.
-   (wasm) Fix request bodies using multipart vs formdata.
-   Update internal `rustls` to 0.20.

#### v0.11.6

-   (wasm) Fix request bodies more.

#### v0.11.5

-   Add `ClientBuilder::http1_only()` method.
-   Add `tls::Version` type, and `ClientBuilder::min_tls_version()` and `ClientBuilder::max_tls_version()` methods.
-   Implement `TryFrom<Request>` for `http::Request`.
-   Implement `Clone` for `Identity`.
-   Fix `NO_PROXY`environment variable parsing to more closely match curl's. Comma-separated entries are now trimmed for whitespace, and `*` is allowed to match everything.
-   Fix redirection to respect `https_only` option.
-   (wasm) Add `Body::as_bytes()` method.
-   (wasm) Fix sometimes wrong conversation of bytes into a `JsValue`.
-   (wasm) Avoid dependency on serde-serialize feature.

#### v0.11.4

-   Add `ClientBuilder::resolve()` option to override DNS resolution for specific domains.
-   Add `native-tls-alpn` Cargo feature to use ALPN with the native-tls backend.
-   Add `ClientBuilder::deflate()` option and `deflate` Cargo feature to support decoding response bodies using deflate.
-   Add `RequestBuilder::version()` to allow setting the HTTP version of a request.
-   Fix allowing "invalid" certificates with the `rustls-tls` backend, when the server uses TLS v1.2 or v1.3.
-   (wasm) Add `try_clone` to `Request` and `RequestBuilder`

#### v0.11.3

-   Add `impl From<hyper::Body> for reqwest::Body`.
-   (wasm) Add credentials mode methods to `RequestBuilder`.

#### v0.11.2

-   Add `CookieStore` trait to customize the type that stores and retrieves cookies for a session.
-   Add `cookie::Jar` as a default `CookieStore`, easing creating some session cookies before creating the `Client`.
-   Add `ClientBuilder::http2_adaptive_window()` option to configure an adaptive HTTP2 flow control behavior.
-   Add `ClientBuilder::http2_max_frame_size()` option to adjust the maximum HTTP2 frame size that can be received.
-   Implement `IntoUrl` for `String`, making it more convenient to create requests with `format!`.

#### v0.11.1

-   Add `ClientBuilder::tls_built_in_root_certs()` option to disable built-in root certificates.
-   Fix `rustls-tls` glue to more often support ALPN to upgrade to HTTP/2.
-   Fix proxy parsing to assume `http://` if no scheme is found.
-   Fix connection pool idle reaping by enabling hyper's `runtime` feature.
-   (wasm) Add `Request::new()` constructor.

### [`v0.11.27`](https://togithub.com/seanmonstar/reqwest/blob/HEAD/CHANGELOG.md#v01127)

[Compare Source](https://togithub.com/seanmonstar/reqwest/compare/v0.11.26...v0.11.27)

-   Add `hickory-dns` feature, deprecating `trust-dns`.
-   (wasm) Fix `Form::text()` to not set octet-stream for plain text fields.

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about these updates again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4yNjkuMiIsInVwZGF0ZWRJblZlciI6IjM3LjI2OS4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiJ9-->


---

_Comment by @renovate[bot] on 2024-04-08 03:04_

### ⚠ Artifact update problem

Renovate failed to update artifacts related to this branch. You probably do not want to merge this PR as-is.

♻ Renovate will retry this branch, including artifacts, only when one of the following happens:

 - any of the package files in this branch needs updating, or 
 - the branch becomes conflicted, or
 - you click the rebase/retry checkbox if found above, or
 - you rename this PR's title to start with "rebase!" to trigger it manually

The artifact failure details are included below:

##### File name: Cargo.lock

```
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path crates/uv/Cargo.toml --package reqwest@0.11.26 --precise 0.12.3
    Updating crates.io index
error: failed to select a version for the requirement `reqwest = "^0.11.10"`
candidate versions found which didn't match: 0.12.3
location searched: crates.io index
required by package `reqwest-middleware v0.2.5`
    ... which satisfies dependency `reqwest-middleware = "^0.2.4"` (locked to 0.2.5) of package `requirements-txt v0.0.1 (/tmp/renovate/repos/github/astral-sh/uv/crates/requirements-txt)`
perhaps a crate was updated and forgotten to be re-vendored?

```

##### File name: Cargo.lock

```
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path Cargo.toml --package reqwest@0.11.26 --precise 0.12.3
    Updating crates.io index
error: failed to select a version for the requirement `reqwest = "^0.11.10"`
candidate versions found which didn't match: 0.12.3
location searched: crates.io index
required by package `reqwest-middleware v0.2.5`
    ... which satisfies dependency `reqwest-middleware = "^0.2.4"` (locked to 0.2.5) of package `requirements-txt v0.0.1 (/tmp/renovate/repos/github/astral-sh/uv/crates/requirements-txt)`
perhaps a crate was updated and forgotten to be re-vendored?

```



---

_Label `internal` added by @renovate[bot] on 2024-04-08 03:04_

---

_Comment by @zanieb on 2024-04-08 03:55_

Being managed in https://github.com/astral-sh/uv/pull/2817

---

_Closed by @zanieb on 2024-04-08 03:55_

---

_Comment by @renovate[bot] on 2024-04-08 03:55_

### Renovate Ignore Notification

Because you closed this PR without merging, Renovate will ignore this update (`0.12.0`). You will get a PR once a newer version is released. To ignore this dependency forever, add it to the `ignoreDeps` array of your Renovate config.

If you accidentally closed this PR, or if you changed your mind: rename this PR to get a fresh replacement PR.

---

_Branch deleted on 2024-04-08 03:55_

---
