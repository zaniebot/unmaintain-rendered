---
number: 6126
title: "docs(cookbook): Provide a custom TypedValueParser example"
type: pull_request
state: merged
author: epage
labels: []
assignees: []
merged: true
base: master
head: p
created_at: 2025-09-11T17:12:28Z
updated_at: 2025-09-11T20:20:42Z
url: https://github.com/clap-rs/clap/pull/6126
synced_at: 2026-01-07T13:12:20-06:00
---

# docs(cookbook): Provide a custom TypedValueParser example

---

_Pull request opened by @epage on 2025-09-11 17:12_

Inspired by #6124

Taken from `cargo release`

---

_@eirnym reviewed on 2025-09-11 20:05_

---

_Review comment by @eirnym on `examples/typed-derive/custom.rs`:75 on 2025-09-11 20:05_

this will be more readable and propagate good practices

```rust
#[allow(clippy::needless_collect, reason="Erasing a lifetime")]
```

---

_@epage reviewed on 2025-09-11 20:12_

---

_Review comment by @epage on `examples/typed-derive/custom.rs`:75 on 2025-09-11 20:12_

This doesn't work with our MSRV of 1.74

---

_@eirnym reviewed on 2025-09-11 20:20_

---

_Review comment by @eirnym on `examples/typed-derive/custom.rs`:75 on 2025-09-11 20:20_

Thank you for that piece of knowledge, I haven't know it

---
