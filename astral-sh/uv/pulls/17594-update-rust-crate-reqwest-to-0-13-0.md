```yaml
number: 17594
title: Update Rust crate reqwest to 0.13.0
type: pull_request
state: open
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/reqwest-0.x
created_at: 2026-01-19T01:34:24Z
updated_at: 2026-01-19T01:34:26Z
url: https://github.com/astral-sh/uv/pull/17594
synced_at: 2026-01-19T02:24:58Z
```

# Update Rust crate reqwest to 0.13.0

---

_@renovate_

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [reqwest](https://redirect.github.com/seanmonstar/reqwest) | workspace.dependencies | minor | `0.12.22` → `0.13.0` |

---

### Release Notes

<details>
<summary>seanmonstar/reqwest (reqwest)</summary>

### [`v0.13.1`](https://redirect.github.com/seanmonstar/reqwest/blob/HEAD/CHANGELOG.md#v0131)

[Compare Source](https://redirect.github.com/seanmonstar/reqwest/compare/v0.13.0...v0.13.1)

- Fixes compiling with rustls on Android targets.

### [`v0.13.0`](https://redirect.github.com/seanmonstar/reqwest/blob/HEAD/CHANGELOG.md#v0130)

[Compare Source](https://redirect.github.com/seanmonstar/reqwest/compare/v0.12.28...v0.13.0)

- **Breaking changes**:
  - `rustls` is now the default TLS backend, instead of `native-tls`.
  - `rustls` crypto provider defaults to aws-lc instead of *ring*. (`rustls-no-provider` exists if you want a different crypto provider)
  - `rustls-tls` has been renamed to `rustls`.
  - rustls roots features removed, `rustls-platform-verifier` is used by default.
    - To use different roots, call `tls_certs_only(your_roots)`.
  - `native-tls` now includes ALPN. To disable, use `native-tls-no-alpn`.
  - `query` and `form` are now crate features, disabled by default.
  - Long-deprecated methods and crate features have been removed (such as `trust-dns`, which was renamed `hickory-dns` a while ago).
- Many TLS-related methods renamed to improve autocompletion and discovery, but previous name left in place with a "soft" deprecation. (just documented, no warnings)
  - For example, prefer `tls_backend_rustls()` over `use_rustls_tls()`.

#### v0.12.28

- Fix compiling on Windows if TLS and SOCKS features are not enabled.

#### v0.12.27

- Add `ClientBuilder::windows_named_pipe(name)` option that will force all requests over that Windows Named Piper.

#### v0.12.26

- Fix sending `Accept-Encoding` header only with values configured with reqwest, regardless of underlying tower-http config.

#### v0.12.25

- Add `Error::is_upgrade()` to determine if the error was from an HTTP upgrade.
- Fix sending `Proxy-Authorization` if only username is configured.
- Fix sending `Proxy-Authorization` to HTTPS proxies when the target is HTTP.
- Refactor internal decompression handling to use tower-http.

#### v0.12.24

- Refactor cookie handling to an internal middleware.
- Refactor internal random generator.
- Refactor base64 encoding to reduce a copy.
- Documentation updates.

#### v0.12.23

- Add `ClientBuilder::unix_socket(path)` option that will force all requests over that Unix Domain Socket.
- Add `ClientBuilder::retry(policy)` and `reqwest::retry::Builder` to configure automatic retries.
- Add `ClientBuilder::dns_resolver2()` with more ergonomic argument bounds, allowing more resolver implementations.
- Add `http3_*` options to `blocking::ClientBuilder`.
- Fix default TCP timeout values to enabled and faster.
- Fix SOCKS proxies to default to port 1080
- (wasm) Add cache methods to `RequestBuilder`.

#### v0.12.22

- Fix socks proxies when resolving IPv6 destinations.

#### v0.12.21

- Fix socks proxy to use `socks4a://` instead of `socks4h://`.
- Fix `Error::is_timeout()` to check for hyper and IO timeouts too.
- Fix request `Error` to again include URLs when possible.
- Fix socks connect error to include more context.
- (wasm) implement `Default` for `Body`.

#### v0.12.20

- Add `ClientBuilder::tcp_user_timeout(Duration)` option to set `TCP_USER_TIMEOUT`.
- Fix proxy headers only using the first matched proxy.
- (wasm) Fix re-adding `Error::is_status()`.

#### v0.12.19

- Fix redirect that changes the method to GET should remove payload headers.
- Fix redirect to only check the next scheme if the policy action is to follow.
- (wasm) Fix compilation error if `cookies` feature is enabled (by the way, it's a noop feature in wasm).

#### v0.12.18

- Fix compilation when `socks` enabled without TLS.

#### v0.12.17

- Fix compilation on macOS.

#### v0.12.16

- Add `ClientBuilder::http3_congestion_bbr()` to enable BBR congestion control.
- Add `ClientBuilder::http3_send_grease()` to configure whether to send use QUIC grease.
- Add `ClientBuilder::http3_max_field_section_size()` to configure the maximum response headers.
- Add `ClientBuilder::tcp_keepalive_interval()` to configure TCP probe interval.
- Add `ClientBuilder::tcp_keepalive_retries()` to configure TCP probe count.
- Add `Proxy::headers()` to add extra headers that should be sent to a proxy.
- Fix `redirect::Policy::limit()` which had an off-by-1 error, allowing 1 more redirect than specified.
- Fix HTTP/3 to support streaming request bodies.
- (wasm) Fix null bodies when calling `Response::bytes_stream()`.

#### v0.12.15

- Fix Windows to support both `ProxyOverride` and `NO_PROXY`.
- Fix http3 to support streaming response bodies.
- Fix http3 dependency from public API misuse.

#### v0.12.14

- Fix missing `fetch_mode_no_cors()`, marking as deprecated when not on WASM.

#### v0.12.13

- Add `Form::into_reader()` for blocking `multipart` forms.
- Add `Form::into_stream()` for async `multipart` forms.
- Add support for SOCKS4a proxies.
- Fix decoding responses with multiple zstd frames.
- Fix `RequestBuilder::form()` from overwriting a previously set `Content-Type` header, like the other builder methods.
- Fix cloning of request timeout in `blocking::Request`.
- Fix http3 synchronization of connection creation, reducing unneccesary extra connections.
- Fix Windows system proxy to use `ProxyOverride` as a `NO_PROXY` value.
- Fix blocking read to correctly reserve and zero read buffer.
- (wasm) Add support for request timeouts.
- (wasm) Fix `Error::is_timeout()` to return true when from a request timeout.

#### v0.12.12

- (wasm) Fix compilation by not compiler `tokio/time` on WASM.

#### v0.12.11

- Fix decompression returning an error when HTTP/2 ends with an empty data frame.

#### v0.12.10

- Add `ClientBuilder::connector_layer()` to allow customizing the connector stack.
- Add `ClientBuilder::http2_max_header_list_size()` option.
- Fix propagating body size hint (`content-length`) information when wrapping bodies.
- Fix decompression of chunked bodies so the connections can be reused more often.

#### v0.12.9

- Add `tls::CertificateRevocationLists` support.
- Add crate features to enable webpki roots without selecting a rustls provider.
- Fix `connection_verbose()` to output read logs.
- Fix `multipart::Part::file()` to automatically include content-length.
- Fix proxy to internally no longer cache system proxy settings.

#### v0.12.8

- Add support for SOCKS4 proxies.
- Add `multipart::Form::file()` method for adding files easily.
- Add `Body::wrap()` to wrap any `http_body::Body` type.
- Fix the pool configuration to use a timer to remove expired connections.

#### v0.12.7

- Revert adding `impl Service<http::Request<_>>` for `Client`.

#### v0.12.6

- Add support for `danger_accept_invalid_hostnames` for `rustls`.
- Add `impl Service<http::Request<Body>>` for `Client` and `&'_ Client`.
- Add support for `!Sync` bodies in `Body::wrap_stream()`.
- Enable happy eyeballs when `hickory-dns` is used.
- Fix `Proxy` so that `HTTP(S)_PROXY` values take precedence over `ALL_PROXY`.
- Fix `blocking::RequestBuilder::header()` from unsetting `sensitive` on passed header values.

#### v0.12.5

- Add `blocking::ClientBuilder::dns_resolver()` method to change DNS resolver in blocking client.
- Add `http3` feature back, still requiring `reqwest_unstable`.
- Add `rustls-tls-no-provider` Cargo feature to use rustls without a crypto provider.
- Fix `Accept-Encoding` header combinations.
- Fix http3 resolving IPv6 addresses.
- Internal: upgrade to rustls 0.23.

#### v0.12.4

- Add `zstd` support, enabled with `zstd` Cargo feature.
- Add `ClientBuilder::read_timeout(Duration)`, which applies the duration for each read operation. The timeout resets after a successful read.

#### v0.12.3

- Add `FromStr` for `dns::Name`.
- Add `ClientBuilder::built_in_webpki_certs(bool)` to enable them separately.
- Add `ClientBuilder::built_in_native_certs(bool)` to enable them separately.
- Fix sending `content-length: 0` for GET requests.
- Fix response body `content_length()` to return value when timeout is configured.
- Fix `ClientBuilder::resolve()` to use lowercase domain names.

#### v0.12.2

- Fix missing ALPN when connecting to socks5 proxy with rustls.
- Fix TLS version limits with rustls.
- Fix not detected ALPN h2 from server with native-tls.

#### v0.12.1

- Fix `ClientBuilder::interface()` when no TLS is enabled.
- Fix `TlsInfo::peer_certificate()` being truncated with rustls.
- Fix panic if `http2` feature disabled but TLS negotiated h2 in ALPN.
- Fix `Display` for `Error` to not include its source error.

### [`v0.12.28`](https://redirect.github.com/seanmonstar/reqwest/blob/HEAD/CHANGELOG.md#v01228)

[Compare Source](https://redirect.github.com/seanmonstar/reqwest/compare/v0.12.27...v0.12.28)

- Fix compiling on Windows if TLS and SOCKS features are not enabled.

### [`v0.12.27`](https://redirect.github.com/seanmonstar/reqwest/blob/HEAD/CHANGELOG.md#v01227)

[Compare Source](https://redirect.github.com/seanmonstar/reqwest/compare/v0.12.26...v0.12.27)

- Add `ClientBuilder::windows_named_pipe(name)` option that will force all requests over that Windows Named Piper.

### [`v0.12.26`](https://redirect.github.com/seanmonstar/reqwest/blob/HEAD/CHANGELOG.md#v01226)

[Compare Source](https://redirect.github.com/seanmonstar/reqwest/compare/v0.12.25...v0.12.26)

- Fix sending `Accept-Encoding` header only with values configured with reqwest, regardless of underlying tower-http config.

### [`v0.12.25`](https://redirect.github.com/seanmonstar/reqwest/blob/HEAD/CHANGELOG.md#v01225)

[Compare Source](https://redirect.github.com/seanmonstar/reqwest/compare/v0.12.24...v0.12.25)

- Add `Error::is_upgrade()` to determine if the error was from an HTTP upgrade.
- Fix sending `Proxy-Authorization` if only username is configured.
- Fix sending `Proxy-Authorization` to HTTPS proxies when the target is HTTP.
- Refactor internal decompression handling to use tower-http.

### [`v0.12.24`](https://redirect.github.com/seanmonstar/reqwest/blob/HEAD/CHANGELOG.md#v01224)

[Compare Source](https://redirect.github.com/seanmonstar/reqwest/compare/v0.12.23...v0.12.24)

- Refactor cookie handling to an internal middleware.
- Refactor internal random generator.
- Refactor base64 encoding to reduce a copy.
- Documentation updates.

### [`v0.12.23`](https://redirect.github.com/seanmonstar/reqwest/blob/HEAD/CHANGELOG.md#v01223)

[Compare Source](https://redirect.github.com/seanmonstar/reqwest/compare/v0.12.22...v0.12.23)

- Add `ClientBuilder::unix_socket(path)` option that will force all requests over that Unix Domain Socket.
- Add `ClientBuilder::retry(policy)` and `reqwest::retry::Builder` to configure automatic retries.
- Add `ClientBuilder::dns_resolver2()` with more ergonomic argument bounds, allowing more resolver implementations.
- Add `http3_*` options to `blocking::ClientBuilder`.
- Fix default TCP timeout values to enabled and faster.
- Fix SOCKS proxies to default to port 1080
- (wasm) Add cache methods to `RequestBuilder`.

</details>

---

### Configuration

📅 **Schedule**: Branch creation - Between 12:00 AM and 03:59 AM, only on Monday ( * 0-3 * * 1 ) (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR was generated by [Mend Renovate](https://mend.io/renovate/). View the [repository job log](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiI0Mi43NC41IiwidXBkYXRlZEluVmVyIjoiNDIuNzQuNSIsInRhcmdldEJyYW5jaCI6Im1haW4iLCJsYWJlbHMiOlsiaW50ZXJuYWwiXX0=-->


---

_Label `internal` added by @renovate[bot] on 2026-01-19 01:34_

---

_Comment by @renovate[bot] on 2026-01-19 01:34_

### ⚠️ Artifact update problem

Renovate failed to update an artifact related to this branch. You probably do not want to merge this PR as-is.

♻ Renovate will retry this branch, including artifacts, only when one of the following happens:

 - any of the package files in this branch needs updating, or 
 - the branch becomes conflicted, or
 - you click the rebase/retry checkbox if found above, or
 - you rename this PR's title to start with "rebase!" to trigger it manually

The artifact failure details are included below:

##### File name: Cargo.lock

```
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path Cargo.toml --package reqwest@0.12.22 --precise 0.13.1
    Updating crates.io index
error: failed to select a version for the requirement `reqwest = "^0.12.0"`
candidate versions found which didn't match: 0.13.1
location searched: crates.io index
required by package `astral-reqwest-middleware v0.4.2`
    ... which satisfies dependency `reqwest-middleware = "^0.4.2"` of package `uv-auth v0.0.15 (/tmp/renovate/repos/github/astral-sh/uv/crates/uv-auth)`
    ... which satisfies path dependency `uv-auth` (locked to 0.0.15) of package `uv v0.9.26 (/tmp/renovate/repos/github/astral-sh/uv/crates/uv)`

```



---
