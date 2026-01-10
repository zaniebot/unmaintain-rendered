```yaml
number: 2895
title: Update Rust crate hyper to v1
type: pull_request
state: closed
author: renovate
labels:
  - internal
assignees: []
base: main
head: renovate/hyper-1.x
created_at: 2024-04-08T04:24:54Z
updated_at: 2024-04-08T04:32:47Z
url: https://github.com/astral-sh/uv/pull/2895
synced_at: 2026-01-10T14:43:31Z
```

# Update Rust crate hyper to v1

---

_Pull request opened by @renovate on 2024-04-08 04:24_

[![Mend Renovate](https://app.renovatebot.com/images/banner.svg)](https://renovatebot.com)

This PR contains the following updates:

| Package | Type | Update | Change |
|---|---|---|---|
| [hyper](https://hyper.rs) ([source](https://togithub.com/hyperium/hyper)) | dev-dependencies | major | `0.14.28` -> `1.0.0` |

---

### Release Notes

<details>
<summary>hyperium/hyper (hyper)</summary>

### [`v1.2.0`](https://togithub.com/hyperium/hyper/blob/HEAD/CHANGELOG.md#v120-2024-02-21)

[Compare Source](https://togithub.com/hyperium/hyper/compare/v1.1.0...v1.2.0)

##### Bug Fixes

-   **http2:** typo in trace logging ([#&#8203;3536](https://togithub.com/hyperium/hyper/issues/3536)) ([79862ec2](https://togithub.com/hyperium/hyper/commit/79862ec2e84c32122c820958ceec06d8b7701ff7))
-   **rt:** `Sleep::downcast_mut_pin()` no longer extend lifetime ([7206fe30](https://togithub.com/hyperium/hyper/commit/7206fe30302937075c51c16a69d1eb3bbce6a671), closes [#&#8203;3556](https://togithub.com/hyperium/hyper/issues/3556))

##### Features

-   **http1:** support configurable `max_headers(num)` to client and server ([#&#8203;3523](https://togithub.com/hyperium/hyper/issues/3523)) ([b1142448](https://togithub.com/hyperium/hyper/commit/b114244898828e9fb254bea1f0bbdd24850b2f3f))
-   **http2:**
    -   add config for `max_local_error_reset_streams` in server ([#&#8203;3530](https://togithub.com/hyperium/hyper/issues/3530)) ([d7680e30](https://togithub.com/hyperium/hyper/commit/d7680e30e48926a5a3f94a0986d39181d5ab2218))
    -   add `initial_max_send_streams` method to HTTP/2 client builder ([#&#8203;3524](https://togithub.com/hyperium/hyper/issues/3524)) ([fdfa60d9](https://togithub.com/hyperium/hyper/commit/fdfa60d9fafb8a6bfb40acc4042ee54a2b9aad32))
    -   add `max_pending_accept_reset_streams(num)` back to HTTP/2 server builder ([#&#8203;3507](https://togithub.com/hyperium/hyper/issues/3507) ([a9fa893f](https://togithub.com/hyperium/hyper/commit/a9fa893f18c6409abae2e1dcbba0f4487df54d4f))

##### Breaking Changes

-   The returned lifetime from `Sleep::downcast_mut_pin()`
    is no longer `'static`. This shouldn't affect most usage. This sort of
    breaking change is needed because it is *wrong*.

([7206fe30](https://togithub.com/hyperium/hyper/commit/7206fe30302937075c51c16a69d1eb3bbce6a671))

### [`v1.1.0`](https://togithub.com/hyperium/hyper/blob/HEAD/CHANGELOG.md#v110-2023-12-18)

[Compare Source](https://togithub.com/hyperium/hyper/compare/v1.0.1...v1.1.0)

##### Bug Fixes

-   **http1:**
    -   add internal limit for chunked extensions ([#&#8203;3495](https://togithub.com/hyperium/hyper/issues/3495)) ([d71ff962](https://togithub.com/hyperium/hyper/commit/d71ff962b08aca2f1c9c1724dfdab5bc1ec6ecd2))
    -   reject chunked headers missing a digit ([#&#8203;3494](https://togithub.com/hyperium/hyper/issues/3494)) ([82915386](https://togithub.com/hyperium/hyper/commit/829153865a4d2bbb52227183c8857e57dc3e231b))

##### Features

-   **client:** add `http1::Connection` `without_shutdown()` method ([#&#8203;3430](https://togithub.com/hyperium/hyper/issues/3430)) ([210bfaa7](https://togithub.com/hyperium/hyper/commit/210bfaa711b5da1f6756582a2e4bc3e229924800))
-   **http1:** Add support for sending HTTP/1.1 Chunked Trailer Fields ([#&#8203;3375](https://togithub.com/hyperium/hyper/issues/3375)) ([31b41807](https://togithub.com/hyperium/hyper/commit/31b41807523370f3efbf47ba16c9e1c193b6335a), closes [#&#8203;2719](https://togithub.com/hyperium/hyper/issues/2719))
-   **server:** expose `server::conn::http1::UpgradeableConnection` ([#&#8203;3457](https://togithub.com/hyperium/hyper/issues/3457)) ([6e3042a8](https://togithub.com/hyperium/hyper/commit/6e3042a86f10359624857d31bc9e876f521aee42))

##### v1.0.1 (2023-11-16)

This release "fixes" or adds a few things that should have been in 1.0.0, but were forgotten. Thus, it includes additions that would normally be a semver-minor release, but because it is so close to 1.0.0, it is released as a patch version.

##### Bug Fixes

-   **rt:** implement Read/Write for Pin<P> ([#&#8203;3413](https://togithub.com/hyperium/hyper/issues/3413)) ([dd6d81ca](https://togithub.com/hyperium/hyper/commit/dd6d81ca4a180695dc70d6c9b2aececd29606224), closes [#&#8203;3412](https://togithub.com/hyperium/hyper/issues/3412))

##### Features

-   **rt:** Make ReadBuf::new public ([7161f562](https://togithub.com/hyperium/hyper/commit/7161f56274a30bfbe4a718bbe21d35beaf86b00b))

##### Breaking Changes

-   Pin is #\[fundamental], so providing a Read/Write impl for it theoretically conflicts
    with existing user Read/Write for Pin<...> impls. However, those impls
    probably don't exist yet.
    ([dd6d81ca](https://togithub.com/hyperium/hyper/commit/dd6d81ca4a180695dc70d6c9b2aececd29606224))

### [`v1.0.1`](https://togithub.com/hyperium/hyper/blob/HEAD/CHANGELOG.md#v101-2023-11-16)

[Compare Source](https://togithub.com/hyperium/hyper/compare/v1.0.0...v1.0.1)

This release "fixes" or adds a few things that should have been in 1.0.0, but were forgotten. Thus, it includes additions that would normally be a semver-minor release, but because it is so close to 1.0.0, it is released as a patch version.

##### Bug Fixes

-   **rt:** implement Read/Write for Pin<P> ([#&#8203;3413](https://togithub.com/hyperium/hyper/issues/3413)) ([dd6d81ca](https://togithub.com/hyperium/hyper/commit/dd6d81ca4a180695dc70d6c9b2aececd29606224), closes [#&#8203;3412](https://togithub.com/hyperium/hyper/issues/3412))

##### Features

-   **rt:** Make ReadBuf::new public ([7161f562](https://togithub.com/hyperium/hyper/commit/7161f56274a30bfbe4a718bbe21d35beaf86b00b))

##### Breaking Changes

-   Pin is #\[fundamental], so providing a Read/Write impl for it theoretically conflicts
    with existing user Read/Write for Pin<...> impls. However, those impls
    probably don't exist yet.
    ([dd6d81ca](https://togithub.com/hyperium/hyper/commit/dd6d81ca4a180695dc70d6c9b2aececd29606224))

### [`v1.0.0`](https://togithub.com/hyperium/hyper/blob/HEAD/CHANGELOG.md#v100-2023-11-15)

[Compare Source](https://togithub.com/hyperium/hyper/compare/v0.14.28...v1.0.0)

Be sure to check out the [upgrading guide](https://hyper.rs/guides/1/upgrading).

##### Bug Fixes

-   **client:**
    -   avoid double-polling a Select future ([#&#8203;3290](https://togithub.com/hyperium/hyper/issues/3290)) ([fece9f7f](https://togithub.com/hyperium/hyper/commit/fece9f7f50431cf9533cfe7106b53a77b48db699), closes [#&#8203;3289](https://togithub.com/hyperium/hyper/issues/3289))
    -   early server response shouldn't propagate NO_ERROR ([#&#8203;3275](https://togithub.com/hyperium/hyper/issues/3275)) ([194e6f98](https://togithub.com/hyperium/hyper/commit/194e6f984763f5dc1c376082170a85cc4db40ce4), closes [#&#8203;2872](https://togithub.com/hyperium/hyper/issues/2872))
    -   remove Send bounds for request `Body` ([#&#8203;3266](https://togithub.com/hyperium/hyper/issues/3266)) ([4ace340b](https://togithub.com/hyperium/hyper/commit/4ace340bb00a2ffe8ec93e4955989eb69f29d531), closes [#&#8203;3184](https://togithub.com/hyperium/hyper/issues/3184))
-   **ffi:** fix deadlock in `hyper_executor::poll_next` ([#&#8203;3370](https://togithub.com/hyperium/hyper/issues/3370)) ([0c7d03ef](https://togithub.com/hyperium/hyper/commit/0c7d03eff2f2433e4f4a0a768009d97e1a7858fd), closes [#&#8203;3369](https://togithub.com/hyperium/hyper/issues/3369))
-   **http2:**
    -   don't send keep-alive ping when idle ([#&#8203;3381](https://togithub.com/hyperium/hyper/issues/3381)) ([429ad8a3](https://togithub.com/hyperium/hyper/commit/429ad8a34b20a877b4d17df1f4991a193f4a56f0))
    -   change default server max concurrent streams to 200 ([#&#8203;3362](https://togithub.com/hyperium/hyper/issues/3362)) ([dd638b5b](https://togithub.com/hyperium/hyper/commit/dd638b5b34225d2c5ad0bd01de0ecf738f9a0e12), closes [#&#8203;3358](https://togithub.com/hyperium/hyper/issues/3358))
-   **server:** Respect Expect header only when http proto > 1.0 ([#&#8203;3294](https://togithub.com/hyperium/hyper/issues/3294)) ([43d2f5c6](https://togithub.com/hyperium/hyper/commit/43d2f5c6cfd575f7259a5b3684f7e99cedbd0edb))

##### Features

-   **client:** allow `!Send` IO with HTTP/1 client ([#&#8203;3371](https://togithub.com/hyperium/hyper/issues/3371)) ([cf87eda8](https://togithub.com/hyperium/hyper/commit/cf87eda82ca0af1f5f45b0a0710eaae9ec1aed7b), closes [#&#8203;3363](https://togithub.com/hyperium/hyper/issues/3363))
-   **error:**
    -   `Error::source()` is purposefully unspecified ([#&#8203;3318](https://togithub.com/hyperium/hyper/issues/3318)) ([502a6450](https://togithub.com/hyperium/hyper/commit/502a645053b0d19252d9fdc170b0a2c0a6fe0ba6), closes [#&#8203;2843](https://togithub.com/hyperium/hyper/issues/2843))
    -   change `Display for Error` to only print top error ([#&#8203;3312](https://togithub.com/hyperium/hyper/issues/3312)) ([50f123af](https://togithub.com/hyperium/hyper/commit/50f123afc0e6c289030bf8699dbec826dc47572c), closes [#&#8203;2844](https://togithub.com/hyperium/hyper/issues/2844))
-   **ext:**
    -   make `ReasonPhrase::from_static` a const fn ([d4a61e3d](https://togithub.com/hyperium/hyper/commit/d4a61e3da87a08a25772cd3aa2f503cb4346c81f))
    -   remove `ReasonPhrase::from_bytes_unchecked()` method ([4021c57b](https://togithub.com/hyperium/hyper/commit/4021c57bd9265b9ebc5b88c9bffbb0ac3704bdbe))
-   **lib:**
    -   update to `http` 1.0 ([899e92a5](https://togithub.com/hyperium/hyper/commit/899e92a580961c5bd1cc9b49a8fb7c7cd8b53ef8))
    -   missing Timer will warn or panic ([f3308c04](https://togithub.com/hyperium/hyper/commit/f3308c044d402dfad448bbc0497b14c69a8f22f2), closes [#&#8203;3393](https://togithub.com/hyperium/hyper/issues/3393))
    -   increase MSRV to 1.63 ([#&#8203;3293](https://togithub.com/hyperium/hyper/issues/3293)) ([e68dc961](https://togithub.com/hyperium/hyper/commit/e68dc961a7dad0a96e16898b0da234927564c079))
-   **rt:** rename to `Http2ClientConnExec` and `Http2ServerConnExec` ([52b27faa](https://togithub.com/hyperium/hyper/commit/52b27faa09715b5835804de7abf6998e028736fc))
-   **server:** default `http1` `header_read_timeout` to 30 seconds ([8bf26d1e](https://togithub.com/hyperium/hyper/commit/8bf26d1e394a8f207debe45445a5fb85cc349238))
-   **upgrade:** introduce tracing as an optional unstable feature ([#&#8203;3326](https://togithub.com/hyperium/hyper/issues/3326)) ([da3fc76c](https://togithub.com/hyperium/hyper/commit/da3fc76c06b6caa60f6abc1da570d56d7c8fa468), closes [#&#8203;3319](https://togithub.com/hyperium/hyper/issues/3319))

##### Breaking Changes

-   Upgrade to `http` 1.0.

([899e92a5](https://togithub.com/hyperium/hyper/commit/899e92a580961c5bd1cc9b49a8fb7c7cd8b53ef8))

-   (From previous RCs) `ExecutorClient` is renamed to
    `Http2ClientConnExec`, and `Http2ConnExec` is renamed to
    `Http2ServerConnExec`.

([52b27faa](https://togithub.com/hyperium/hyper/commit/52b27faa09715b5835804de7abf6998e028736fc))

-   If you use client HTTP/1 upgrades, you must call
    `Connection::with_upgrades()` to still work the same.
    ([cf87eda8](https://togithub.com/hyperium/hyper/commit/cf87eda82ca0af1f5f45b0a0710eaae9ec1aed7b))
-   HTTP/2 server builder now has a default max concurrent streams. This is a
    behavior change. Consider setting your own maximum.
    ([dd638b5b](https://togithub.com/hyperium/hyper/commit/dd638b5b34225d2c5ad0bd01de0ecf738f9a0e12))
-   Do not build any logic depending on the exact types of
    an `Error::source()`. They are only for debugging.
    ([502a6450](https://togithub.com/hyperium/hyper/commit/502a645053b0d19252d9fdc170b0a2c0a6fe0ba6))
-   The format no longer prints the error chain. Be sure to
    check if you are logging errors directly.

    The `Error::message()` method is removed, it is no longer needed.

    The `Error::into_cause()` method is removed.
    ([50f123af](https://togithub.com/hyperium/hyper/commit/50f123afc0e6c289030bf8699dbec826dc47572c))
-   The `ReasonPhrase::from_bytes_unchecked()` method is
    gone. Use `from_static()` or `TryFrom` to construct one.

([4021c57b](https://togithub.com/hyperium/hyper/commit/4021c57bd9265b9ebc5b88c9bffbb0ac3704bdbe))

##### v1.0.0-rc.4 (2023-07-10)

##### Bug Fixes

-   **http1:**
    -   http1 server graceful shutdown fix ([#&#8203;3261](https://togithub.com/hyperium/hyper/issues/3261)) ([f4b51300](https://togithub.com/hyperium/hyper/commit/f4b513009d81083081d1c60c1981847bbb17dd5d))
    -   send error on Incoming body when connection errors ([#&#8203;3256](https://togithub.com/hyperium/hyper/issues/3256)) ([52f19259](https://togithub.com/hyperium/hyper/commit/52f192593fb9ebcf6d3894e0c85cbf710da4decd), closes [#&#8203;3253](https://togithub.com/hyperium/hyper/issues/3253))
    -   properly end chunked bodies when it was known to be empty ([#&#8203;3254](https://togithub.com/hyperium/hyper/issues/3254)) ([fec64cf0](https://togithub.com/hyperium/hyper/commit/fec64cf0abdc678e30ca5f1b310c5118b2e01999), closes [#&#8203;3252](https://togithub.com/hyperium/hyper/issues/3252))

##### Features

-   **client:** Make clients able to use non-Send executor ([#&#8203;3184](https://togithub.com/hyperium/hyper/issues/3184)) ([d977f209](https://togithub.com/hyperium/hyper/commit/d977f209bc6068d8f878b22803fc42d90c887fcc), closes [#&#8203;3017](https://togithub.com/hyperium/hyper/issues/3017))
-   **rt:**
    -   replace IO traits with hyper::rt ones ([#&#8203;3230](https://togithub.com/hyperium/hyper/issues/3230)) ([f9f65b7a](https://togithub.com/hyperium/hyper/commit/f9f65b7aa67fa3ec0267fe015945973726285bc2), closes [#&#8203;3110](https://togithub.com/hyperium/hyper/issues/3110))
    -   add downcast on `Sleep` trait ([#&#8203;3125](https://togithub.com/hyperium/hyper/issues/3125)) ([d92d3917](https://togithub.com/hyperium/hyper/commit/d92d3917d950e4c61c37c2170f3ce273d2a0f7d1), closes [#&#8203;3027](https://togithub.com/hyperium/hyper/issues/3027))
-   **service:** change Service::call to take \&self ([#&#8203;3223](https://togithub.com/hyperium/hyper/issues/3223)) ([d894439e](https://togithub.com/hyperium/hyper/commit/d894439e009aa75103f6382a7ba98fb17da72f02), closes [#&#8203;3040](https://togithub.com/hyperium/hyper/issues/3040))

##### Breaking Changes

-   Any IO transport type provided must not implement `hyper::rt::{Read, Write}` instead of
    `tokio::io` traits. You can grab a helper type from `hyper-util` to wrap Tokio types, or implement the traits yourself,
    if it's a custom type.
    ([f9f65b7a](https://togithub.com/hyperium/hyper/commit/f9f65b7aa67fa3ec0267fe015945973726285bc2))
-   `client::conn::http2` types now use another generic for an `Executor`.
    Code that names `Connection` needs to include the additional generic parameter.
    ([d977f209](https://togithub.com/hyperium/hyper/commit/d977f209bc6068d8f878b22803fc42d90c887fcc))
-   The Service::call function no longer takes a mutable reference to self.
    The FnMut trait bound on the service::util::service_fn function and the trait bound
    on the impl for the ServiceFn struct were changed from FnMut to Fn.

([d894439e](https://togithub.com/hyperium/hyper/commit/d894439e009aa75103f6382a7ba98fb17da72f02))

##### v1.0.0-rc.3 (2023-02-23)

##### Bug Fixes

-   **server:** prevent sending 100-continue if user drops request body ([#&#8203;3137](https://togithub.com/hyperium/hyper/issues/3137)) ([499fe1f9](https://togithub.com/hyperium/hyper/commit/499fe1f949895218c4fd2305a0eddaf24f1dd0a9))

##### Features

-   **client:**
    -   add `is_ready()` and `is_closed()` methods to `SendRequest` ([#&#8203;3148](https://togithub.com/hyperium/hyper/issues/3148)) ([3fb59919](https://togithub.com/hyperium/hyper/commit/3fb59919941d3145be6d84dab85d222ea0e7664b))
    -   `http2` builder now requires an `Executor` ([#&#8203;3135](https://togithub.com/hyperium/hyper/issues/3135)) ([8068aa01](https://togithub.com/hyperium/hyper/commit/8068aa011f6477a21ad54230c8fef9e26b330503), closes [#&#8203;3128](https://togithub.com/hyperium/hyper/issues/3128))
    -   remove unneeded HTTP/1 executor ([#&#8203;3108](https://togithub.com/hyperium/hyper/issues/3108)) ([1de9accf](https://togithub.com/hyperium/hyper/commit/1de9accf1e133d1a23311879f466251b2f6481e5))
-   **rt:** make private executor traits public (but sealed) in `rt::bounds` ([#&#8203;3127](https://togithub.com/hyperium/hyper/issues/3127)) ([fc9f3070](https://togithub.com/hyperium/hyper/commit/fc9f30701a159772d0c014de47d16798502bae2c), closes [#&#8203;2051](https://togithub.com/hyperium/hyper/issues/2051), [#&#8203;3097](https://togithub.com/hyperium/hyper/issues/3097))

##### Breaking Changes

-   `hyper::client::conn::Http2::Builder::new` now requires an executor argument.
    ([8068aa01](https://togithub.com/hyperium/hyper/commit/8068aa011f6477a21ad54230c8fef9e26b330503))
-   The method
    `hyper::client::conn::http1::Builder::executor()` is removed, since it did nothing.
    ([1de9accf](https://togithub.com/hyperium/hyper/commit/1de9accf1e133d1a23311879f466251b2f6481e5))

##### v1.0.0-rc.2 (2022-12-29)

##### Bug Fixes

-   **client:** send an error back to client when dispatch misbehaves () ([75aac9f4](https://togithub.com/hyperium/hyper/commit/75aac9f47fe0246016e6133cd3cfa35b63c8904e), closes [#&#8203;2649](https://togithub.com/hyperium/hyper/issues/2649))
-   **http2:** Fix race condition in client dispatcher ([#&#8203;3041](https://togithub.com/hyperium/hyper/issues/3041)) ([f202230c](https://togithub.com/hyperium/hyper/commit/f202230c6fa274f6a4e6cbaad57ca59beb0a5125))

##### Features

-   **body:** upgrade to http-body 1.0.0-rc.2 ([#&#8203;3106](https://togithub.com/hyperium/hyper/issues/3106)) ([51b45e3f](https://togithub.com/hyperium/hyper/commit/51b45e3f8580da5667a45395e6622455b10e2ad3))
-   **client:**
    -   remove http2\_ prefixes from `client::conn::http2::Builder` methods ([669df217](https://togithub.com/hyperium/hyper/commit/669df2173e059544fbaded0d666c5bfc113eaa0e))
    -   remove http1\_ prefixes from `client::conn::http1::Builder` methods ([4cbaef79](https://togithub.com/hyperium/hyper/commit/4cbaef79f0ec03643c09e4e6fbbed23bf589e548))
    -   implement `Clone` for `http2::SendRequest` ([#&#8203;3042](https://togithub.com/hyperium/hyper/issues/3042)) ([00ea49e4](https://togithub.com/hyperium/hyper/commit/00ea49e47a565748a4e4657f7047dca5851f8b7a), closes [#&#8203;3036](https://togithub.com/hyperium/hyper/issues/3036))
    -   allow ignoring HTTP/1 invalid header lines in requests ([81e25fa8](https://togithub.com/hyperium/hyper/commit/81e25fa868c86e4ea81d5a96fdca497a4b1ab3c1))
-   **rt:** Clean up Timer trait ([#&#8203;3037](https://togithub.com/hyperium/hyper/issues/3037)) ([8790fee7](https://togithub.com/hyperium/hyper/commit/8790fee74937016e6b288493bc62c61f7866c310), closes [#&#8203;3028](https://togithub.com/hyperium/hyper/issues/3028))
-   **server:**
    -   remove http1\_ method prefixes from `server::conn::http2::Builder` ([291ed0b4](https://togithub.com/hyperium/hyper/commit/291ed0b49bc7fd6f43890815cdf93aaefaf59011))
    -   remove http1\_ method prefixes from `server::conn::http2::Builder` ([48e70c69](https://togithub.com/hyperium/hyper/commit/48e70c691e44d5e37d4b51fe8980f76d27c989b3))
    -   remove `server::conn::http2::Builder::with_executor()` ([#&#8203;3089](https://togithub.com/hyperium/hyper/issues/3089)) ([ab59a6f7](https://togithub.com/hyperium/hyper/commit/ab59a6f7a1e654b1607744320de5f8477de5d6c8), closes [#&#8203;3087](https://togithub.com/hyperium/hyper/issues/3087))

##### Breaking Changes

-   removes `server::conn::http2::Builder::with_executor()`
    ([ab59a6f7](https://togithub.com/hyperium/hyper/commit/ab59a6f7a1e654b1607744320de5f8477de5d6c8))
-   The return types of `Timer` have been changed.
    ([8790fee7](https://togithub.com/hyperium/hyper/commit/8790fee74937016e6b288493bc62c61f7866c310))
-   The return types for `Frame::into_data()` and `Frame::into_trailers()` have been changed from `Option<T>` to `Result<T, Self>`.

##### v1.0.0-rc.1 (2022-10-25)

##### Bug Fixes

-   **http1:**
    -   trim obs-folded headers when unfolding ([#&#8203;2926](https://togithub.com/hyperium/hyper/issues/2926)) ([d4b5bd4e](https://togithub.com/hyperium/hyper/commit/d4b5bd4ee6af0ae8924cf05ab799cc3e19a3c62d))

##### Features

-   **body:**
    -   rename `Body` struct to `Incoming` ([#&#8203;3022](https://togithub.com/hyperium/hyper/issues/3022)) ([95a153bb](https://togithub.com/hyperium/hyper/commit/95a153bbc2717bd4233486e09848622ceb900574), closes [#&#8203;2971](https://togithub.com/hyperium/hyper/issues/2971))
    -   update `HttpBody` trait to use `Frame`s ([#&#8203;3020](https://togithub.com/hyperium/hyper/issues/3020)) ([0888623d](https://togithub.com/hyperium/hyper/commit/0888623d3764e887706d4e38f82f0fb57c50bd1a), closes [#&#8203;3010](https://togithub.com/hyperium/hyper/issues/3010))
    -   make body::Sender and Body::channel private ([#&#8203;2970](https://togithub.com/hyperium/hyper/issues/2970)) ([d963e6a9](https://togithub.com/hyperium/hyper/commit/d963e6a9504575116f63df2485d8480fdb9b6f0b), closes [#&#8203;2962](https://togithub.com/hyperium/hyper/issues/2962))
    -   remove "full" constructors from `Body` ([#&#8203;2958](https://togithub.com/hyperium/hyper/issues/2958)) ([9e8fc8fc](https://togithub.com/hyperium/hyper/commit/9e8fc8fca195f470a82be5bfb2fd8019c044b97a))
-   **client:**
    -   remove `client::conn::{SendRequest, Connection}` ([#&#8203;2987](https://togithub.com/hyperium/hyper/issues/2987)) ([8ae73cac](https://togithub.com/hyperium/hyper/commit/8ae73cac6a8f6a61944505c121158dc312e7b68f))
    -   remove `client::connect` module ([#&#8203;2949](https://togithub.com/hyperium/hyper/issues/2949)) ([5e206883](https://togithub.com/hyperium/hyper/commit/5e206883984fe6de2812861ec363964d92b89b06))
    -   remove higher-level `hyper::Client` ([#&#8203;2941](https://togithub.com/hyperium/hyper/issues/2941)) ([bb3af17c](https://togithub.com/hyperium/hyper/commit/bb3af17ce1a3841e9170adabcce595c7c8743ea7))
    -   remove `hyper::client::server` ([#&#8203;2940](https://togithub.com/hyperium/hyper/issues/2940)) ([889fa2d8](https://togithub.com/hyperium/hyper/commit/889fa2d87252108eb7668b8bf034ffcc30985117))
    -   introduce version-specific client modules ([#&#8203;2906](https://togithub.com/hyperium/hyper/issues/2906)) ([509672aa](https://togithub.com/hyperium/hyper/commit/509672aada0af68a91d963e69828c6e31c44cb7b))
-   **ffi:** add http1\_allow_multiline_headers ([#&#8203;2918](https://togithub.com/hyperium/hyper/issues/2918)) ([09e35668](https://togithub.com/hyperium/hyper/commit/09e35668e5b094d679efb4b98ecde9cb6f9f2f93))
-   **lib:** remove `stream` cargo feature ([#&#8203;2896](https://togithub.com/hyperium/hyper/issues/2896)) ([ce72f734](https://togithub.com/hyperium/hyper/commit/ce72f73464d96fd67b59ceff08fd424733b43ffa), closes [#&#8203;2855](https://togithub.com/hyperium/hyper/issues/2855))
-   **rt:** add Timer trait ([#&#8203;2974](https://togithub.com/hyperium/hyper/issues/2974)) ([fae97ced](https://togithub.com/hyperium/hyper/commit/fae97ced3a1f71fc46b6eadd3313e19705cc0006))
-   **server:**
    -   remove `server::conn::{Http, Connection}` types ([#&#8203;3013](https://togithub.com/hyperium/hyper/issues/3013)) ([0766d3f7](https://togithub.com/hyperium/hyper/commit/0766d3f78d116ea243222cea134cfe7f418e6a3c), closes [#&#8203;3012](https://togithub.com/hyperium/hyper/issues/3012))
    -   `server::conn::http1` and `server::conn::http2` modules ([#&#8203;3011](https://togithub.com/hyperium/hyper/issues/3011)) ([fc4d3356](https://togithub.com/hyperium/hyper/commit/fc4d3356cb7f2fffff5af9c474fa34c5adc5d6f1), closes [#&#8203;2851](https://togithub.com/hyperium/hyper/issues/2851))
    -   remove the high-level Server API ([#&#8203;2932](https://togithub.com/hyperium/hyper/issues/2932)) ([3c7bef3b](https://togithub.com/hyperium/hyper/commit/3c7bef3b6f6b6c3ec780e5e2db12c9d5795c1b80))
    -   remove `AddrStream` struct ([#&#8203;2869](https://togithub.com/hyperium/hyper/issues/2869)) ([e9cab49e](https://togithub.com/hyperium/hyper/commit/e9cab49e6e18f712b94137966580f6756e32fabb), closes [#&#8203;2850](https://togithub.com/hyperium/hyper/issues/2850))
-   **service:** create own `Service` trait ([#&#8203;2920](https://togithub.com/hyperium/hyper/issues/2920)) ([fee7d361](https://togithub.com/hyperium/hyper/commit/fee7d361c28c7eb42ef6bbfae0db14028d24bfee), closes [#&#8203;2853](https://togithub.com/hyperium/hyper/issues/2853))

##### Breaking Changes

-   The polling functions of the `Body` trait have been
    redesigned.

    The free functions `hyper::body::to_bytes` and `aggregate` have been
    removed. Similar functionality is on
    `http_body_util::BodyExt::collect`.
    ([0888623d](https://togithub.com/hyperium/hyper/commit/0888623d3764e887706d4e38f82f0fb57c50bd1a))
-   Either choose a version-specific `Connection` type, or
    look for the auto-version type in `hyper-util`.
    ([0766d3f7](https://togithub.com/hyperium/hyper/commit/0766d3f78d116ea243222cea134cfe7f418e6a3c))
-   Pick a version-specific connection, or use the combined
    one in `hyper-util`.
    ([8ae73cac](https://togithub.com/hyperium/hyper/commit/8ae73cac6a8f6a61944505c121158dc312e7b68f))
-   Change any manual `impl tower::Service` to implement `hyper::service::Service` instead. The `poll_ready` method has been removed.
    ([fee7d361](https://togithub.com/hyperium/hyper/commit/fee7d361c28c7eb42ef6bbfae0db14028d24bfee))
-   The trait has been renamed.
    ([031454e5](https://togithub.com/hyperium/hyper/commit/031454e5e647dda0648424a944dbef795505e2e4))
-   A channel body will be available in `hyper-util`.
    ([d963e6a9](https://togithub.com/hyperium/hyper/commit/d963e6a9504575116f63df2485d8480fdb9b6f0b))
-   Use the types from `http-body-util`.
    ([9e8fc8fc](https://togithub.com/hyperium/hyper/commit/9e8fc8fca195f470a82be5bfb2fd8019c044b97a))
-   Use `connect` from `hyper-util`.
    ([5e206883](https://togithub.com/hyperium/hyper/commit/5e206883984fe6de2812861ec363964d92b89b06))
-   A pooling client is in the hyper-util crate.
    ([bb3af17c](https://togithub.com/hyperium/hyper/commit/bb3af17ce1a3841e9170adabcce595c7c8743ea7))
-   Tower `Service` utilities will exist in `hyper-util`.
    ([889fa2d8](https://togithub.com/hyperium/hyper/commit/889fa2d87252108eb7668b8bf034ffcc30985117))

##### v0.14.19 (2022-05-27)

##### Bug Fixes

-   **http1:** fix preserving header case without enabling ffi ([#&#8203;2820](https://togithub.com/hyperium/hyper/issues/2820)) ([6a35c175](https://togithub.com/hyperium/hyper/commit/6a35c175f2b416851518b5831c2c7827d6dbd822))
-   **server:** don't add implicit content-length to HEAD responses ([#&#8203;2836](https://togithub.com/hyperium/hyper/issues/2836)) ([67b73138](https://togithub.com/hyperium/hyper/commit/67b73138f110979f3c77ef7b56588f018837e592))

##### Features

-   **server:**
    -   add `Connection::http2_max_header_list_size` option ([#&#8203;2828](https://togithub.com/hyperium/hyper/issues/2828)) ([a32658c1](https://togithub.com/hyperium/hyper/commit/a32658c1ae7f1261fa234a767df963be4fc63521), closes [#&#8203;2826](https://togithub.com/hyperium/hyper/issues/2826))
    -   add `AddrStream::local_addr()` ([#&#8203;2816](https://togithub.com/hyperium/hyper/issues/2816)) ([ffbf610b](https://togithub.com/hyperium/hyper/commit/ffbf610b1631cabfacb20886270e3c137fa93800), closes [#&#8203;2773](https://togithub.com/hyperium/hyper/issues/2773))

##### Breaking Changes

-   **ffi (unstable):**
    -   `hyper_clientconn_options_new` no longer sets the `http1_preserve_header_case` connection option by default.
        Users should now call `hyper_clientconn_options_set_preserve_header_case` if they desire that functionality. ([78de8914](https://togithub.com/hyperium/hyper/commit/78de8914eadeab4b9a2c71a82c77b2ce33fe6c74))

##### v0.14.18 (2022-03-22)

##### Bug Fixes

-   **ffi:** don't build C libraries by default ([1c663706](https://togithub.com/hyperium/hyper/commit/1c6637060e36654ddb2fdfccb0d146c7ad527476))

##### Features

-   **client:** add `HttpInfo::local_addr()` method ([055b4e7e](https://togithub.com/hyperium/hyper/commit/055b4e7ea6bd22859c20d60776b0c8f20d27498e), closes [#&#8203;2767](https://togithub.com/hyperium/hyper/issues/2767))

##### v0.14.17 (2022-02-10)

##### Bug Fixes

-   **client:** avoid panics in uses of `Instant` ([#&#8203;2746](https://togithub.com/hyperium/hyper/issues/2746)) ([dcdd6d10](https://togithub.com/hyperium/hyper/commit/dcdd6d109069949ee68ba70ece4a2b4f21079479))

##### Features

-   **client:** implement the HTTP/2 extended CONNECT protocol from RFC 8441 ([#&#8203;2682](https://togithub.com/hyperium/hyper/issues/2682)) ([5ec094ca](https://togithub.com/hyperium/hyper/commit/5ec094caa5c999e6f919a2bc82f5f3b7d40c2d8a))
-   **error:** add `Error::message` ([#&#8203;2737](https://togithub.com/hyperium/hyper/issues/2737)) ([6932896a](https://togithub.com/hyperium/hyper/commit/6932896a7fca58fe461269461f925da8fd4e8d8a), closes [#&#8203;2732](https://togithub.com/hyperium/hyper/issues/2732))
-   **http1:** implement obsolete line folding ([#&#8203;2734](https://togithub.com/hyperium/hyper/issues/2734)) ([1f0c177b](https://togithub.com/hyperium/hyper/commit/1f0c177b35b14054eb1e5108e75f8bd3ff52813e))

##### v0.14.16 (2021-12-09)

##### Bug Fixes

-   **http1:** return 414 when URI contains more than 65534 characters ([#&#8203;2706](https://togithub.com/hyperium/hyper/issues/2706)) ([5f938fff](https://togithub.com/hyperium/hyper/commit/5f938fffa64df23a2e4af81ed4e6d8bd760e2d05), closes [#&#8203;2701](https://togithub.com/hyperium/hyper/issues/2701))
-   **http2:** received `Body::size_hint()` now return 0 if implicitly empty ([#&#8203;2715](https://togithub.com/hyperium/hyper/issues/2715)) ([84b78b6c](https://togithub.com/hyperium/hyper/commit/84b78b6c877ff9aaa28d1e348a5deb63a9282503))
-   **server:** use case-insensitive comparison for Expect: 100-continue ([#&#8203;2709](https://togithub.com/hyperium/hyper/issues/2709)) ([7435cc33](https://togithub.com/hyperium/hyper/commit/7435cc3399895643062f4e399fae6d5b20b049a1), closes [#&#8203;2708](https://togithub.com/hyperium/hyper/issues/2708))

##### Features

-   **http2:** add `http2_max_send_buf_size` option to client and server ([bff977b7](https://togithub.com/hyperium/hyper/commit/bff977b73ca8d737f5492c86c09fd64735c45461))
-   **server:** add HTTP/1 header read timeout option ([#&#8203;2675](https://togithub.com/hyperium/hyper/issues/2675)) ([842c6553](https://togithub.com/hyperium/hyper/commit/842c6553a5414a3a4a0fbf973079200612a9c3d2), closes [#&#8203;2457](https://togithub.com/hyperium/hyper/issues/2457))

##### v0.14.15 (2021-11-16)

##### Bug Fixes

-   **client:** cancel blocking DNS lookup if `GaiFuture` is dropped ([174b553d](https://togithub.com/hyperium/hyper/commit/174b553d)

##### Features

-   **http1:** add `http1_writev(bool)` options to Client and Server builders, to allow forcing vectored writes ([80627141](https://togithub.com/hyperium/hyper/commit/80627141))
-   **upgrade:** allow http upgrades with any body type ([ab469eb3](https://togithub.com/hyperium/hyper/commit/ab469eb3c6cd5e7a035d734f3d21ff4d2d6a21ab))

##### v0.14.14 (2021-10-22)

##### Bug Fixes

-   **client:**
    -   make ResponseFuture implement Sync ([bd6c35b9](https://togithub.com/hyperium/hyper/commit/bd6c35b98f9513f14ed9cecad933bc7fdb8635ea))
    -   remove ipv6 square brackets before resolving ([910e0268](https://togithub.com/hyperium/hyper/commit/910e02687df3245aae4bc519fb0bd7eb6a34db7d))

##### Features

-   **h2:** always include original h2 error on broken pipe ([6169db25](https://togithub.com/hyperium/hyper/commit/6169db250c932dd012d391389826cd34833077b4))
-   **server:** Remove Send + Sync requirement for Body in with_graceful_shutdown ([1d553e52](https://togithub.com/hyperium/hyper/commit/1d553e52c6953ea3b039f5c3f89d35cb56e2436a))

##### v0.14.13 (2021-09-16)

##### Bug Fixes

-   **client:** don't reuse a connection while still flushing ([c88011da](https://togithub.com/hyperium/hyper/commit/c88011da4ed5b5ca9107c4a2339a7ab054c5f27f))
-   **server:** convert panic to error if Connection::without_shutdown called on HTTP/2 conn ([ea3e2282](https://togithub.com/hyperium/hyper/commit/ea3e228287e714b97aa44c840a487abd3a915e15))

##### Features

-   **ffi:** add hyper_request_set_uri_parts ([a54689b9](https://togithub.com/hyperium/hyper/commit/a54689b921ca16dd0f29b3f4a74feae60218db34))
-   **lib:**
    -   Export more things with Cargo features (server, !http1, !http2) ([0a4b56ac](https://togithub.com/hyperium/hyper/commit/0a4b56acb82ef41a3336f482b240c67c784c434f))
    -   Export rt module independently of Cargo features ([cf6f62c7](https://togithub.com/hyperium/hyper/commit/cf6f62c71eda3b3a8732d86387e4ed8711cf9a86))

##### v0.14.12 (2021-08-24)

##### Bug Fixes

-   **ffi:** on_informational callback had no headers ([39b6d01a](https://togithub.com/hyperium/hyper/commit/39b6d01aa0e520077bb25e16811f5ece00a224d6))
-   **http1:** apply header title case for consecutive dashes ([#&#8203;2613](https://togithub.com/hyperium/hyper/issues/2613)) ([684f2fa7](https://togithub.com/hyperium/hyper/commit/684f2fa76d44fa2b1b063ad0443a1b0d16dfad0e))
-   **http2:** improve errors emitted by HTTP2 `Upgraded` stream shutdown ([#&#8203;2622](https://togithub.com/hyperium/hyper/issues/2622)) ([be08648e](https://togithub.com/hyperium/hyper/commit/be08648e8298cdb13e9879ee761a73f827268962))

##### Features

-   **client:** expose http09 and http1 options on `client::conn::Builder` ([#&#8203;2611](https://togithub.com/hyperium/hyper/issues/2611)) ([73bff4e9](https://togithub.com/hyperium/hyper/commit/73bff4e98c372ce04b006370c0b0d2af29ea8718), closes [#&#8203;2461](https://togithub.com/hyperium/hyper/issues/2461))

##### v0.14.11 (2021-07-21)

##### Bug Fixes

-   **client:** retry when pool checkout returns closed HTTP2 connection ([#&#8203;2585](https://togithub.com/hyperium/hyper/issues/2585)) ([52214f39](https://togithub.com/hyperium/hyper/commit/52214f391c0a18dc66d1ccff9c0c004c5da85002))
-   **http2:**
    -   improve I/O errors emitted by H2Upgraded ([#&#8203;2598](https://togithub.com/hyperium/hyper/issues/2598)) ([f51c677d](https://togithub.com/hyperium/hyper/commit/f51c677dec9debf60cb336dc938bae103adf17a0))
    -   preserve `proxy-authenticate` and `proxy-authorization` headers ([#&#8203;2597](https://togithub.com/hyperium/hyper/issues/2597)) ([52435701](https://togithub.com/hyperium/hyper/commit/5243570137ae49628cb387fff5611eea0add33bf))

##### Features

-   **ffi:** add hyper_request_on_informational ([25d18c0b](https://togithub.com/hyperium/hyper/commit/25d18c0b74ccf9e51f986daa3b2b98c0109f827a))

##### v0.14.10 (2021-07-07)

##### Bug Fixes

-   **http1:**
    -   reject content-lengths that have a plus sign prefix ([06335158](https://togithub.com/hyperium/hyper/commit/06335158ca48724db9bf074398067d2db08613e7))
    -   protect against overflow in chunked decoder ([efd9a982](https://togithub.com/hyperium/hyper/commit/efd9a9821fd2f1ae04b545094de76a435b62e70f))

##### Features

-   **ffi:** add option to get raw headers from response ([8c89a8c1](https://togithub.com/hyperium/hyper/commit/8c89a8c1665b6fbec3f13b8c0e84c79464179c89))

##### v0.14.9 (2021-06-07)

##### Bug Fixes

-   **http1:** reduce memory used with flatten write strategy ([eb0c6463](https://togithub.com/hyperium/hyper/commit/eb0c64639503bbd4f6e3b1ce3a02bff8eeea7ee8))

##### v0.14.8 (2021-05-25)

##### Features

-   **client:** allow to config http2 max concurrent reset streams ([#&#8203;2535](https://togithub.com/hyperium/hyper/issues/2535)) ([b9916c41](https://togithub.com/hyperium/hyper/commit/b9916c410182c6225e857f0cded355ea1b74c865))
-   **error:** add `Error::is_parse_too_large` and `Error::is_parse_status` methods ([#&#8203;2538](https://togithub.com/hyperium/hyper/issues/2538)) ([960a69a5](https://togithub.com/hyperium/hyper/commit/960a69a5878ede82c56f50ac1444a9e75e885a8f))
-   **http2:**
    -   Implement Client and Server CONNECT support over HTTP/2 ([#&#8203;2523](https://togithub.com/hyperium/hyper/issues/2523)) ([5442b6fa](https://togithub.com/hyperium/hyper/commit/5442b6faddaff9aeb7c073031a3b7aa4497fda4d), closes [#&#8203;2508](https://togithub.com/hyperium/hyper/issues/2508))
    -   allow HTTP/2 requests by ALPN when http2\_only is unset ([#&#8203;2527](https://togithub.com/hyperium/hyper/issues/2527)) ([be9677a1](https://togithub.com/hyperium/hyper/commit/be9677a1e782d33c4402772e0fc4ef0a4c49d507))

##### Performance

-   **http2:** reduce amount of adaptive window pings as BDP stabilizes ([#&#8203;2550](https://togithub.com/hyperium/hyper/issues/2550)) ([4cd06bf2](https://togithub.com/hyperium/hyper/commit/4cd06bf2))

##### v0.14.7 (2021-04-22)

##### Bug Fixes

-   **http1:** http1\_title_case_headers should move Builder ([a303b3c3](https://togithub.com/hyperium/hyper/commit/a303b3c329e6b8ecfa1da0b9b9e94736628167e0))

##### Features

-   **server:** implement forgotten settings for case preserving ([4fd6c4cb](https://togithub.com/hyperium/hyper/commit/4fd6c4cb0b58bb0831ae0f876d858aba1588d0e3))

##### v0.14.6 (2021-04-21)

##### Features

-   **client:** add option to allow misplaced spaces in HTTP/1 responses ([#&#8203;2506](https://togithub.com/hyperium/hyper/issues/2506)) ([11345394](https://togithub.com/hyperium/hyper/commit/11345394d968d4817e1a0ee2550228ac0ae7ce74))
-   **http1:** add options to preserve header casing ([#&#8203;2480](https://togithub.com/hyperium/hyper/issues/2480)) ([dbea7716](https://togithub.com/hyperium/hyper/commit/dbea7716f157896bf7d2d417be7b4e382e7dc34f), closes [#&#8203;2313](https://togithub.com/hyperium/hyper/issues/2313))

##### v0.14.5 (2021-03-26)

##### Bug Fixes

-   **client:** omit default port from automatic Host headers ([#&#8203;2441](https://togithub.com/hyperium/hyper/issues/2441)) ([0b11eee9](https://togithub.com/hyperium/hyper/commit/0b11eee9bde421cdc1680cadabfd38c5aff8e62f))
-   **headers:** Support multiple Content-Length values on same line ([#&#8203;2471](https://togithub.com/hyperium/hyper/issues/2471)) ([48fdaf16](https://togithub.com/hyperium/hyper/commit/48fdaf160689f333e9bb63388d0b1d0fa29a1391), closes [#&#8203;2470](https://togithub.com/hyperium/hyper/issues/2470))
-   **server:** skip automatic Content-Length headers when not allowed ([#&#8203;2216](https://togithub.com/hyperium/hyper/issues/2216)) ([8cbf9527](https://togithub.com/hyperium/hyper/commit/8cbf9527dfb313b3f84fcd83260c5c72ce4a1beb), closes [#&#8203;2215](https://togithub.com/hyperium/hyper/issues/2215))

##### Features

-   **client:** allow HTTP/0.9 responses behind a flag ([#&#8203;2473](https://togithub.com/hyperium/hyper/issues/2473)) ([68d4e4a3](https://togithub.com/hyperium/hyper/commit/68d4e4a3db91fb43f41a8c4fce1175ddb56816af), closes [#&#8203;2468](https://togithub.com/hyperium/hyper/issues/2468))
-   **server:** add `AddrIncoming::from_listener` constructor ([#&#8203;2439](https://togithub.com/hyperium/hyper/issues/2439)) ([4c946af4](https://togithub.com/hyperium/hyper/commit/4c946af49cc7fbbc6bd4894283a654625c2ea383))

##### v0.14.4 (2021-02-05)

##### Bug Fixes

-   **build**: Fix compile error when only `http1` feature was enabled.

##### v0.14.3 (2021-02-05)

##### Bug Fixes

-   **client:** HTTP/1 client "Transfer-Encoding" repair code would panic ([#&#8203;2410](https://togithub.com/hyperium/hyper/issues/2410)) ([2c8121f1](https://togithub.com/hyperium/hyper/commit/2c8121f1735aa8efeb0d5e4ef595363c373ba470), closes [#&#8203;2409](https://togithub.com/hyperium/hyper/issues/2409))
-   **http1:** fix server misinterpreting multiple Transfer-Encoding headers ([8f93123e](https://togithub.com/hyperium/hyper/commit/8f93123efef5c1361086688fe4f34c83c89cec02))

##### Features

-   **body:**
    -   reexport `hyper::body::SizeHint` ([#&#8203;2404](https://togithub.com/hyperium/hyper/issues/2404)) ([9956587f](https://togithub.com/hyperium/hyper/commit/9956587f83428a5dbe338ba0b55c1dc0bce8c282))
    -   add `send_trailers` to Body channel's `Sender` ([#&#8203;2387](https://togithub.com/hyperium/hyper/issues/2387)) ([bf8d74ad](https://togithub.com/hyperium/hyper/commit/bf8d74ad1cf7d0b33b470b1e61625ebac56f9c4c), closes [#&#8203;2260](https://togithub.com/hyperium/hyper/issues/2260))
-   **ffi:**
    -   add HYPERE_INVALID_PEER_MESSAGE error code for parse errors ([1928682b](https://togithub.com/hyperium/hyper/commit/1928682b33f98244435ba6d574677546205a15ec))
    -   Initial C API for hyper ([3ae1581a](https://togithub.com/hyperium/hyper/commit/3ae1581a539b67363bd87d9d8fc8635a204eec5d))

##### v0.14.2 (2020-12-29)

##### Features

-   **client:** expose `connect` types without proto feature ([#&#8203;2377](https://togithub.com/hyperium/hyper/issues/2377)) ([73a59e5f](https://togithub.com/hyperium/hyper/commit/73a59e5fc7ddedcb7cbd91e97b33385fde57aa10))
-   **server:** expose `Accept` without httpX features ([#&#8203;2382](https://togithub.com/hyperium/hyper/issues/2382)) ([a6d4fcbe](https://togithub.com/hyperium/hyper/commit/a6d4fcbee65bebf461291def75f4c512ec62a664))

##### v0.14.1 (2020-12-23)

-   Fixes building documentation.

</details>

---

### Configuration

📅 **Schedule**: Branch creation - "on Monday" (UTC), Automerge - At any time (no schedule defined).

🚦 **Automerge**: Disabled by config. Please merge this manually once you are satisfied.

♻ **Rebasing**: Whenever PR becomes conflicted, or you tick the rebase/retry checkbox.

🔕 **Ignore**: Close this PR and you won't be reminded about this update again.

---

 - [ ] <!-- rebase-check -->If you want to rebase/retry this PR, check this box

---

This PR has been generated by [Mend Renovate](https://www.mend.io/free-developer-tools/renovate/). View repository job log [here](https://developer.mend.io/github/astral-sh/uv).
<!--renovate-debug:eyJjcmVhdGVkSW5WZXIiOiIzNy4yNjkuMiIsInVwZGF0ZWRJblZlciI6IjM3LjI2OS4yIiwidGFyZ2V0QnJhbmNoIjoibWFpbiJ9-->


---

_Label `internal` added by @renovate[bot] on 2024-04-08 04:24_

---

_Comment by @renovate[bot] on 2024-04-08 04:24_

### ⚠ Artifact update problem

Renovate failed to update an artifact related to this branch. You probably do not want to merge this PR as-is.

♻ Renovate will retry this branch, including artifacts, only when one of the following happens:

 - any of the package files in this branch needs updating, or 
 - the branch becomes conflicted, or
 - you click the rebase/retry checkbox if found above, or
 - you rename this PR's title to start with "rebase!" to trigger it manually

The artifact failure details are included below:

##### File name: Cargo.lock

```
Command failed: cargo update --config net.git-fetch-with-cli=true --manifest-path crates/uv-client/Cargo.toml --package hyper@0.14.28 --precise 1.2.0
    Updating crates.io index
error: failed to select a version for the requirement `hyper = "^0.14.21"`
candidate versions found which didn't match: 1.2.0
location searched: crates.io index
required by package `reqwest v0.11.26`
    ... which satisfies dependency `reqwest = "^0.11.23"` (locked to 0.11.26) of package `uv-git v0.0.1 (/tmp/renovate/repos/github/astral-sh/uv/crates/uv-git)`
    ... which satisfies path dependency `uv-git` (locked to 0.0.1) of package `distribution-types v0.0.1 (/tmp/renovate/repos/github/astral-sh/uv/crates/distribution-types)`
perhaps a crate was updated and forgotten to be re-vendored?

```



---

_Comment by @zanieb on 2024-04-08 04:31_

Conflicts with / must be paired with https://github.com/astral-sh/uv/issues/2814 ?

---

_Closed by @zanieb on 2024-04-08 04:31_

---

_Comment by @renovate[bot] on 2024-04-08 04:32_

### Renovate Ignore Notification

Because you closed this PR without merging, Renovate will ignore this update. You will not get PRs for *any* future `1.x` releases. But if you manually upgrade to `1.x` then Renovate will re-enable `minor` and `patch` updates automatically.

If you accidentally closed this PR, or if you changed your mind: rename this PR to get a fresh replacement PR.

---

_Branch deleted on 2024-04-08 04:32_

---
