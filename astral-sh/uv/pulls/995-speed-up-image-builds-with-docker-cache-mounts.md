```yaml
number: 995
title: Speed up image builds with Docker cache mounts
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: zb/docker-cache
created_at: 2024-01-19T02:21:03Z
updated_at: 2024-01-19T23:40:03Z
url: https://github.com/astral-sh/uv/pull/995
synced_at: 2026-01-12T16:04:21Z
```

# Speed up image builds with Docker cache mounts

---

_@zanieb_

Inspired by the slightly disturbing discussion at https://github.com/rust-lang/cargo/issues/2644

---

_Comment by @zanieb on 2024-01-19 03:20_

Oh _sigh_ cache mounts don't work in GitHub Actions:
- https://github.com/moby/buildkit/issues/1512
- https://github.com/docker/build-push-action/issues/716
- https://github.com/moby/buildkit/issues/1673

Might need to cache `/var/lib/buildkit`per https://github.com/moby/buildkit/issues/1512#issuecomment-1618763074
or do something like:

- https://github.com/dashevo/gh-action-cache-buildkit-state
- https://github.com/moby/buildkit/issues/1512#issuecomment-1319736671
- https://github.com/overmindtech/buildkit-cache-dance
- https://depot.dev/

---

_Comment by @konstin on 2024-01-19 10:05_

What i like to do is to have an empty crate with nothing but the `Cargo.toml`, `Cargo.lock` and dummy `lib.rs`/`main.rs`, build that, then add the source and build again. The first build gets cached and when the deps didn't change the second build runs with all deps already built (https://github.com/PyO3/maturin/blob/3492c978e9a3031aaa60f3c7051990f2c621fd73/Dockerfile#L17-L25).

---

_Comment by @zanieb on 2024-01-19 14:42_

That makes sense although it feels like more of a hack; several comments suggest that in https://github.com/rust-lang/cargo/issues/2644.

The cache mounts seem like a total pain to do in CI.

---

_Closed by @zanieb on 2024-01-19 23:40_

---
