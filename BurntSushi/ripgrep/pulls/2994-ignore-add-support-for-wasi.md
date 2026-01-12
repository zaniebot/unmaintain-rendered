```yaml
number: 2994
title: "ignore: add support for WASI"
type: pull_request
state: closed
author: Timmmm
labels: []
assignees: []
base: master
head: user/timh/wasi
created_at: 2025-02-20T15:59:54Z
updated_at: 2025-08-17T18:35:03Z
url: https://github.com/BurntSushi/ripgrep/pull/2994
synced_at: 2026-01-12T18:23:14Z
```

# ignore: add support for WASI

---

_@Timmmm_

Add support for compiling to `wasm32-wasip2`. For this target `WalkParallel` becomes single threaded. This allows projects that depend on `ignore` to be compiled for WASI without having to modify their code.

Note this depends on [a corresponding PR for `walkdir`](https://github.com/BurntSushi/walkdir/pull/205) so it won't compile currently.

To test add this to `Cargo.toml`:

```
[patch.crates-io]
walkdir = { git = "https://github.com/Timmmm/walkdir.git", branch = "user/timh/wasi" }
```

---

_Comment by @BurntSushi on 2025-08-17 18:35_

See conversation in https://github.com/BurntSushi/walkdir/pull/205

In summary, unless there is an exceptionally good reason to, I generally don't add support for nightly only stuff to libraries. I've done it in the past, and it's almost always been a maintenance headache.

I'd be happy to have these PRs re-submitted once they use stable APIs.

---

_Closed by @BurntSushi on 2025-08-17 18:35_

---
