```yaml
number: 1424
title: "String ownership in `about` function"
type: issue
state: closed
author: juanibiapina
labels: []
assignees: []
created_at: 2019-03-02T17:33:11Z
updated_at: 2020-01-01T16:52:22Z
url: https://github.com/clap-rs/clap/issues/1424
synced_at: 2026-01-12T16:14:10Z
```

# String ownership in `about` function

---

_@juanibiapina_

I would like to understand what is the reason for functions like `about`: https://docs.rs/clap/2.32.0/clap/struct.App.html#method.about to take `&str`.

Although this is generally not a problem, in my current scenario I create a lot of subcommands in a loop, and it's inconvenient to keep the original String around just to satisfy this signature.

---

_Closed by @juanibiapina on 2020-01-01 16:52_

---
