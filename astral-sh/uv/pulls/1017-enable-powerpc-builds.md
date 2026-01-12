```yaml
number: 1017
title: Enable PowerPC builds
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/cross
created_at: 2024-01-19T20:26:18Z
updated_at: 2024-01-20T00:30:40Z
url: https://github.com/astral-sh/uv/pull/1017
synced_at: 2026-01-12T16:04:21Z
```

# Enable PowerPC builds

---

_@charliermarsh_

Closes #1015.

---

_Merged by @charliermarsh on 2024-01-19 22:29_

---

_Closed by @charliermarsh on 2024-01-19 22:29_

---

_Branch deleted on 2024-01-19 22:29_

---

_Comment by @charliermarsh on 2024-01-20 00:21_

I also added a feature here to conditionally enable `zlib-ng`, since it wasn't working on some of the more obscure architectures (https://github.com/astral-sh/puffin/issues/1022).

To test it...

- I verified that the PowerPC builds failed when I added the feature (with libng as the default): https://github.com/astral-sh/puffin/actions/runs/7589457457/job/20674121200?pr=1017.
- I verified that they succeeded once I add `--no-default-features --features rust_backend`: https://github.com/astral-sh/puffin/actions/runs/7589489511/job/20674224333?pr=1017

---

_Comment by @charliermarsh on 2024-01-20 00:30_

I am pretty sure this is working as intended... the crate appears by default:

```
❯ cargo tree -e features -i libz-ng-sys -p puffin
libz-ng-sys v1.1.14
└── libz-ng-sys feature "default"
    └── flate2 v1.0.28
        ├── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin)
        │   └── puffin feature "flate2-zlib-ng" (command-line)
        ├── puffin-extract v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-extract)
        │   └── puffin-extract feature "default"
        │       ├── puffin-build v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-build)
        │       │   └── puffin-build feature "default"
        │       │       ├── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
        │       │       └── puffin-dispatch v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-dispatch)
        │       │           └── puffin-dispatch feature "default"
        │       │               └── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
        │       ├── puffin-distribution v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-distribution)
        │       │   └── puffin-distribution feature "default"
        │       │       ├── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
        │       │       ├── puffin-dispatch v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-dispatch) (*)
        │       │       ├── puffin-installer v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-installer)
        │       │       │   └── puffin-installer feature "default"
        │       │       │       ├── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
        │       │       │       └── puffin-dispatch v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-dispatch) (*)
        │       │       └── puffin-resolver v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-resolver)
        │       │           ├── puffin-resolver feature "clap"
        │       │           │   └── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
        │       │           └── puffin-resolver feature "default"
        │       │               ├── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
        │       │               └── puffin-dispatch v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-dispatch) (*)
        │       └── puffin-installer v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-installer) (*)
        └── zip v0.6.6
            ├── zip feature "deflate"
            │   ├── install-wheel-rs v0.0.1 (/Users/crmarsh/workspace/guffin/crates/install-wheel-rs)
            │   │   ├── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
            │   │   └── puffin-installer v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-installer) (*)
            │   │   ├── install-wheel-rs feature "clap"
            │   │   │   └── install-wheel-rs feature "cli"
            │   │   │       └── install-wheel-rs feature "default"
            │   │   │           ├── puffin-client v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-client)
            │   │   │           │   └── puffin-client feature "default"
            │   │   │           │       ├── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
            │   │   │           │       ├── puffin-dispatch v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-dispatch) (*)
            │   │   │           │       ├── puffin-distribution v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-distribution) (*)
            │   │   │           │       ├── puffin-installer v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-installer) (*)
            │   │   │           │       └── puffin-resolver v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-resolver) (*)
            │   │   │           ├── puffin-distribution v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-distribution) (*)
            │   │   │           └── puffin-resolver v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-resolver) (*)
            │   │   ├── install-wheel-rs feature "cli" (*)
            │   │   ├── install-wheel-rs feature "default" (*)
            │   │   ├── install-wheel-rs feature "parallel"
            │   │   │   └── install-wheel-rs feature "default" (*)
            │   │   └── install-wheel-rs feature "rayon"
            │   │       └── install-wheel-rs feature "parallel" (*)
            │   ├── puffin-distribution v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-distribution) (*)
            │   ├── puffin-extract v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-extract) (*)
            │   └── puffin-resolver v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-resolver) (*)
            └── zip feature "flate2"
                └── zip feature "deflate" (*)
        ├── flate2 feature "any_impl"
        │   ├── flate2 feature "any_zlib"
        │   │   └── flate2 feature "zlib-ng"
        │   │       └── puffin feature "flate2-zlib-ng" (command-line)
        │   └── flate2 feature "rust_backend"
        │       ├── flate2 feature "default"
        │       │   └── async-compression v0.4.6
        │       │       ├── async-compression feature "brotli"
        │       │       │   └── reqwest feature "brotli"
        │       │       │       ├── puffin-client v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-client) (*)
        │       │       │       ├── puffin-distribution v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-distribution) (*)
        │       │       │       ├── puffin-git v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-git)
        │       │       │       │   ├── puffin-git feature "default"
        │       │       │       │   │   ├── distribution-types v0.0.1 (/Users/crmarsh/workspace/guffin/crates/distribution-types)
        │       │       │       │   │   │   └── distribution-types feature "default"
        │       │       │       │   │   │       ├── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
        │       │       │       │   │   │       ├── puffin-build v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-build) (*)
        │       │       │       │   │   │       ├── puffin-cache v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-cache)
        │       │       │       │   │   │       │   ├── puffin-cache feature "clap"
        │       │       │       │   │   │       │   │   └── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
        │       │       │       │   │   │       │   └── puffin-cache feature "default"
        │       │       │       │   │   │       │       ├── gourgeist v0.0.4 (/Users/crmarsh/workspace/guffin/crates/gourgeist)
        │       │       │       │   │   │       │       │   └── gourgeist feature "default"
        │       │       │       │   │   │       │       │       ├── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
        │       │       │       │   │   │       │       │       ├── puffin-build v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-build) (*)
        │       │       │       │   │   │       │       │       └── puffin-dispatch v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-dispatch) (*)
        │       │       │       │   │   │       │       ├── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
        │       │       │       │   │   │       │       ├── puffin-client v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-client) (*)
        │       │       │       │   │   │       │       ├── puffin-dispatch v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-dispatch) (*)
        │       │       │       │   │   │       │       ├── puffin-distribution v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-distribution) (*)
        │       │       │       │   │   │       │       ├── puffin-installer v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-installer) (*)
        │       │       │       │   │   │       │       ├── puffin-interpreter v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-interpreter)
        │       │       │       │   │   │       │       │   └── puffin-interpreter feature "default"
        │       │       │       │   │   │       │       │       ├── gourgeist v0.0.4 (/Users/crmarsh/workspace/guffin/crates/gourgeist) (*)
        │       │       │       │   │   │       │       │       ├── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
        │       │       │       │   │   │       │       │       ├── puffin-build v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-build) (*)
        │       │       │       │   │   │       │       │       ├── puffin-dispatch v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-dispatch) (*)
        │       │       │       │   │   │       │       │       ├── puffin-installer v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-installer) (*)
        │       │       │       │   │   │       │       │       ├── puffin-resolver v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-resolver) (*)
        │       │       │       │   │   │       │       │       └── puffin-traits v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-traits)
        │       │       │       │   │   │       │       │           └── puffin-traits feature "default"
        │       │       │       │   │   │       │       │               ├── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
        │       │       │       │   │   │       │       │               ├── puffin-build v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-build) (*)
        │       │       │       │   │   │       │       │               ├── puffin-dispatch v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-dispatch) (*)
        │       │       │       │   │   │       │       │               ├── puffin-distribution v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-distribution) (*)
        │       │       │       │   │   │       │       │               ├── puffin-installer v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-installer) (*)
        │       │       │       │   │   │       │       │               └── puffin-resolver v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-resolver) (*)
        │       │       │       │   │   │       │       ├── puffin-resolver v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-resolver) (*)
        │       │       │       │   │   │       │       └── puffin-traits v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-traits) (*)
        │       │       │       │   │   │       ├── puffin-client v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-client) (*)
        │       │       │       │   │   │       ├── puffin-dispatch v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-dispatch) (*)
        │       │       │       │   │   │       ├── puffin-distribution v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-distribution) (*)
        │       │       │       │   │   │       ├── puffin-installer v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-installer) (*)
        │       │       │       │   │   │       ├── puffin-resolver v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-resolver) (*)
        │       │       │       │   │   │       └── puffin-traits v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-traits) (*)
        │       │       │       │   │   ├── puffin-distribution v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-distribution) (*)
        │       │       │       │   │   ├── puffin-installer v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-installer) (*)
        │       │       │       │   │   └── puffin-resolver v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-resolver) (*)
        │       │       │       │   └── puffin-git feature "vendored-openssl"
        │       │       │       │       ├── distribution-types v0.0.1 (/Users/crmarsh/workspace/guffin/crates/distribution-types) (*)
        │       │       │       │       ├── puffin-distribution v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-distribution) (*)
        │       │       │       │       ├── puffin-installer v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-installer) (*)
        │       │       │       │       └── puffin-resolver v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-resolver) (*)
        │       │       │       └── puffin-resolver v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-resolver) (*)
        │       │       ├── async-compression feature "deflate"
        │       │       │   └── async_zip feature "deflate"
        │       │       │       ├── puffin-client v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-client) (*)
        │       │       │       └── puffin-extract v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-extract) (*)
        │       │       ├── async-compression feature "flate2"
        │       │       │   ├── async-compression feature "deflate" (*)
        │       │       │   └── async-compression feature "gzip"
        │       │       │       └── reqwest feature "gzip"
        │       │       │           ├── puffin-client v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-client) (*)
        │       │       │           ├── puffin-distribution v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-distribution) (*)
        │       │       │           ├── puffin-git v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-git) (*)
        │       │       │           └── puffin-resolver v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-resolver) (*)
        │       │       ├── async-compression feature "futures-io"
        │       │       │   └── async_zip v0.0.16
        │       │       │       ├── async_zip feature "async-compression"
        │       │       │       │   └── async_zip feature "deflate" (*)
        │       │       │       ├── async_zip feature "default"
        │       │       │       │   ├── puffin-client v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-client) (*)
        │       │       │       │   └── puffin-extract v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-extract) (*)
        │       │       │       ├── async_zip feature "deflate" (*)
        │       │       │       ├── async_zip feature "tokio"
        │       │       │       │   ├── puffin-client v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-client) (*)
        │       │       │       │   └── puffin-extract v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-extract) (*)
        │       │       │       └── async_zip feature "tokio-util"
        │       │       │           └── async_zip feature "tokio" (*)
        │       │       ├── async-compression feature "gzip" (*)
        │       │       └── async-compression feature "tokio"
        │       │           └── reqwest v0.11.23
        │       │               └── reqwest-retry v0.3.0
        │       │                   └── reqwest-retry feature "default"
        │       │                       └── puffin-client v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-client) (*)
        │       │               ├── reqwest feature "__rustls"
        │       │               │   └── reqwest feature "rustls-tls-webpki-roots"
        │       │               │       └── reqwest feature "rustls-tls"
        │       │               │           ├── puffin-client v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-client) (*)
        │       │               │           ├── puffin-distribution v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-distribution) (*)
        │       │               │           ├── puffin-git v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-git) (*)
        │       │               │           └── puffin-resolver v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-resolver) (*)
        │       │               ├── reqwest feature "__tls"
        │       │               │   └── reqwest feature "__rustls" (*)
        │       │               ├── reqwest feature "async-compression"
        │       │               │   ├── reqwest feature "brotli" (*)
        │       │               │   └── reqwest feature "gzip" (*)
        │       │               ├── reqwest feature "blocking"
        │       │               │   └── puffin-git v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-git) (*)
        │       │               │   [dev-dependencies]
        │       │               │   └── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
        │       │               ├── reqwest feature "brotli" (*)
        │       │               ├── reqwest feature "gzip" (*)
        │       │               ├── reqwest feature "hyper-rustls"
        │       │               │   └── reqwest feature "__rustls" (*)
        │       │               ├── reqwest feature "json"
        │       │               │   ├── puffin-client v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-client) (*)
        │       │               │   ├── puffin-distribution v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-distribution) (*)
        │       │               │   ├── puffin-git v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-git) (*)
        │       │               │   ├── puffin-resolver v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-resolver) (*)
        │       │               │   └── reqwest-middleware v0.2.4
        │       │               │       └── reqwest-middleware feature "default"
        │       │               │           ├── puffin-client v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-client) (*)
        │       │               │           └── reqwest-retry v0.3.0 (*)
        │       │               ├── reqwest feature "mime_guess"
        │       │               │   └── reqwest feature "multipart"
        │       │               │       └── reqwest-middleware v0.2.4 (*)
        │       │               ├── reqwest feature "multipart" (*)
        │       │               ├── reqwest feature "rustls"
        │       │               │   [dev-dependencies]
        │       │               │   └── puffin v0.0.3 (/Users/crmarsh/workspace/guffin/crates/puffin) (*)
        │       │               │   └── reqwest feature "__rustls" (*)
        │       │               ├── reqwest feature "rustls-pemfile"
        │       │               │   └── reqwest feature "__rustls" (*)
        │       │               ├── reqwest feature "rustls-tls" (*)
        │       │               ├── reqwest feature "rustls-tls-webpki-roots" (*)
        │       │               ├── reqwest feature "serde_json"
        │       │               │   └── reqwest feature "json" (*)
        │       │               ├── reqwest feature "stream"
        │       │               │   ├── async_http_range_reader v0.4.0 (https://github.com/baszalmstra/async_http_range_reader?rev=8dab2c08ac864fec1df014465264f9a7c8eae905#8dab2c08)
        │       │               │   │   └── async_http_range_reader feature "default"
        │       │               │   │       └── puffin-client v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-client) (*)
        │       │               │   ├── puffin-client v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-client) (*)
        │       │               │   ├── puffin-distribution v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-distribution) (*)
        │       │               │   ├── puffin-git v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-git) (*)
        │       │               │   └── puffin-resolver v0.0.1 (/Users/crmarsh/workspace/guffin/crates/puffin-resolver) (*)
        │       │               ├── reqwest feature "tokio-rustls"
        │       │               │   └── reqwest feature "__rustls" (*)
        │       │               ├── reqwest feature "tokio-util"
        │       │               │   ├── reqwest feature "brotli" (*)
        │       │               │   ├── reqwest feature "gzip" (*)
        │       │               │   └── reqwest feature "stream" (*)
        │       │               ├── reqwest feature "wasm-streams"
        │       │               │   └── reqwest feature "stream" (*)
        │       │               └── reqwest feature "webpki-roots"
        │       │                   └── reqwest feature "rustls-tls-webpki-roots" (*)
        │       └── zip feature "deflate" (*)
        ├── flate2 feature "any_zlib" (*)
        ├── flate2 feature "default" (*)
        ├── flate2 feature "libz-ng-sys"
        │   └── flate2 feature "zlib-ng" (*)
        ├── flate2 feature "miniz_oxide"
        │   └── flate2 feature "rust_backend" (*)
        ├── flate2 feature "rust_backend" (*)
        └── flate2 feature "zlib-ng" (*)
```

But not with `--no-default-features --features flate2-rust_backend`:
```
guffin on  main [$] is 📦 v0.0.3 via 🐍 v3.9.10 via 🦀 v1.75.0
❯ cargo tree -e features -i libz-ng-sys -p puffin --no-default-features --features flate2-rust_backend
error: package ID specification `libz-ng-sys` did not match any packages

	Did you mean `libz-sys`?
```

---

_Comment by @charliermarsh on 2024-01-20 00:30_

\cc @BurntSushi 

---
