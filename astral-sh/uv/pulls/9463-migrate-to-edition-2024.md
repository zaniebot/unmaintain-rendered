```yaml
number: 9463
title: Migrate to edition 2024
type: pull_request
state: closed
author: konstin
labels:
  - no-build
  - no-test
assignees: []
draft: true
base: main
head: konsti/rust-2024
created_at: 2024-11-27T10:20:03Z
updated_at: 2024-12-10T17:05:13Z
url: https://github.com/astral-sh/uv/pull/9463
synced_at: 2026-01-10T12:00:00Z
```

# Migrate to edition 2024

---

_Pull request opened by @konstin on 2024-11-27 10:20_

Following https://blog.rust-lang.org/2024/11/27/Rust-2024-public-testing.html, i tried migrating uv to rust 2024.

There were a lot of warnings about the changed drop order await, for example in `while let Some(...) = (...).await`.

There are some clippy changes:

 * More consistent lifetime elisions
 * Unnecessary hashes in raw strings are stripped (mainly in snapshots)
 * if-let on `Option` are rewritten to matches. I don't like that one
 * `actual.map_or(true, |actual| actual != expected)` becomes `actual != Some(expected)` (personally i like that style more, but i know that other team members don't).
 * `.map_or(false, |...|)` is rewritten to `.is_some_and(|...|)` or `is_ok_and(|...|)`
 * `let size = ((size + buffer_size - 1) / buffer_size) * buffer_size;` becomes `let size = size.div_ceil(buffer_size) * buffer_size;`


---

_Label `no-build` added by @konstin on 2024-11-27 10:20_

---

_Label `no-test` added by @konstin on 2024-11-27 10:20_

---

_Review comment by @konstin on `crates/uv-auth/src/middleware.rs`:223 on 2024-11-27 10:21_

This is a regression

---

_@konstin reviewed on 2024-11-27 10:21_

---

_Closed by @konstin on 2024-12-10 17:05_

---
