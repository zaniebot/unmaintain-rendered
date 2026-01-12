```yaml
number: 2418
title: "--sort=path is not properly documented"
type: issue
state: closed
author: safinaskar
labels:
  - doc
  - rollup
assignees: []
created_at: 2023-02-14T11:15:18Z
updated_at: 2023-11-25T20:03:57Z
url: https://github.com/BurntSushi/ripgrep/issues/2418
synced_at: 2026-01-12T16:13:24Z
```

# --sort=path is not properly documented

---

_@safinaskar_

#### What version of ripgrep are you using?

```text
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`cargo install ripgrep`

#### What operating system are you using ripgrep on?

Debian GNU/Linux Stretch

#### Describe your bug.

`--sort=path` is not fully documented in `rg --help` output. I can imagine two different implementations: in the first one path simply sorted as strings. This is what you get when you run `LC_ALL=C sort`. So we get this order:
```text
a+
a/a
a0
```

The second way is to threat `/` specially. This gives the following order:
```text
a/a
a+
a0
```

My experiments show that `rg --sort=path` uses second way. But this is not documented, so I'm not sure I can rely on it

---

_Label `doc` added by @BurntSushi on 2023-02-14 14:40_

---

_Label `rollup` added by @BurntSushi on 2023-11-22 21:51_

---

_Closed by @BurntSushi on 2023-11-25 20:03_

---
