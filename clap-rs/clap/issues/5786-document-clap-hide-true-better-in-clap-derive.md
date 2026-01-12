```yaml
number: 5786
title: "Document `#[clap(hide = true)]` better in clap-derive"
type: issue
state: closed
author: adamchalmers
labels: []
assignees: []
created_at: 2024-10-22T20:00:54Z
updated_at: 2024-10-23T02:17:25Z
url: https://github.com/clap-rs/clap/issues/5786
synced_at: 2026-01-12T16:14:17Z
```

# Document `#[clap(hide = true)]` better in clap-derive

---

_@adamchalmers_

I wanted to know how to hide a Clap CLI that I made via its `derive(Parser)` macro. So I went to docs.rs/clap-derive and searched both the tutorial and the reference for the strings "hide" and "hidden". They didn't show up anywhere.

Later when searching through discussions, I found the `#[clap(hide = true)]` solution, and I was surprised it wasn't in the docs.

---

_Comment by @epage on 2024-10-23 02:17_

`hide` is a "raw" attribute and we do [suggest people look to the builder API for raw attributes](https://docs.rs/clap/latest/clap/_derive/index.html#terminology).

Improving this situation is being explored in  https://github.com/clap-rs/clap/discussions/4090

Closing in favor of that.

---

_Closed by @epage on 2024-10-23 02:17_

---
