```yaml
number: 2057
title: Add wasm feature which allows a more fine-grained control over regex features
type: issue
state: closed
author: d3lm
labels: []
assignees: []
created_at: 2021-11-11T08:58:36Z
updated_at: 2021-11-12T19:02:04Z
url: https://github.com/BurntSushi/ripgrep/issues/2057
synced_at: 2026-01-12T16:13:24Z
```

# Add wasm feature which allows a more fine-grained control over regex features

---

_@d3lm_

For work I need to implement simplified version of `ripgrep` that operates on an in-memory file system to allow text and file search using glob patterns. It all gets compiled to WASM and ultimately is part of a kernel used for [StackBlitz](https://stackblitz.com/) to run Node.js entirely in a browser. For supporting glob patterns I am using `globset`, however it uses `regex` internally for improved performance and it also enables the `perf` feature.

I was wondering if you would be open for some `wasm` feature for instance (just an example), that allows a more fine grained control over the regex features to reduce binary size. For example, when you enable the `wasm` feature for `globset` it would only enable `std` on regex but not `perf`. This way, users can transitively enable features on `regex` and if they can bare more bytes they could enable the `perf` feature themselves in their own `Cargo.toml`. Or you could choose to only enable `perf-dfa` also gives pretty good bench results.

Wondering what your thoughts are about this. I think WASM is a very valid use case.

---

_Comment by @BurntSushi on 2021-11-11 09:45_

I would want to see what kind of actual difference it makes in binary size. I'm not convinced that simply disabling some features in the regex crate will be enough. And I do _not_ want to start maintaining a totally alternate build path that is dedicated to reducing binary size. I think that's exactly what would happen if a wasm feature were added.

---

_Comment by @d3lm on 2021-11-12 19:02_

Yes you're right. I have investigated a bit more and the difference when disabling the `perf` feature is not that big. And if you enable only `perf-dfa` (even if it has no extra dependencies) it's even less. So this might be neglectable.

Closing this issue then.

---

_Closed by @d3lm on 2021-11-12 19:02_

---
