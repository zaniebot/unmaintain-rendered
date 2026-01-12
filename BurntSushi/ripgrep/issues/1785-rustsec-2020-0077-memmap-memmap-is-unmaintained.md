```yaml
number: 1785
title: "RUSTSEC-2020-0077: memmap: memmap is unmaintained"
type: issue
state: closed
author: brightly-salty
labels: []
assignees: []
created_at: 2021-01-17T21:27:07Z
updated_at: 2021-01-17T23:58:46Z
url: https://github.com/BurntSushi/ripgrep/issues/1785
synced_at: 2026-01-12T16:13:24Z
```

# RUSTSEC-2020-0077: memmap: memmap is unmaintained

---

_@brightly-salty_

When `cargo-audit` is called on any crate that directly or indirectly depends on `grep-searcher`, the following error will appear:

```
Crate:         memmap
Version:       0.7.0
Warning:       unmaintained
Title:         memmap is unmaintained
Date:          2020-12-02
ID:            RUSTSEC-2020-0077
URL:           https://rustsec.org/advisories/RUSTSEC-2020-0077
Dependency tree:
memmap 0.7.0
└── grep-searcher 0.1.7
```

It can be easily remedied by switching from memmap to memmap2 or or mapr (more info at https://rustsec.org/advisories/RUSTSEC-2020-0077, https://github.com/danburkert/memmap-rs/issues/90)

I would be willing to attempt to implement this and submit a PR if it would be accepted.

---

_Closed by @BurntSushi on 2021-01-17 23:58_

---
