```yaml
number: 1705
title: "Update dependencies for `ignore`"
type: pull_request
state: closed
author: jyn514
labels: []
assignees: []
base: master
head: crossbeam
created_at: 2020-10-15T22:17:28Z
updated_at: 2020-11-02T16:48:25Z
url: https://github.com/BurntSushi/ripgrep/pull/1705
synced_at: 2026-01-12T18:23:14Z
```

# Update dependencies for `ignore`

---

_@jyn514_

- crossbeam-channel 0.4 -> 0.5
- crossbeam-utils 0.7 -> 0.8

This removes a duplicate dependency for cargo (previously it depended on
both 0.5 directly and 0.4 through `ignore`).

Let me know if I should also update dependencies for other crates, I'm happy to do so.

---

_Comment by @jyn514 on 2020-10-15 22:20_

This adds a new duplicate dependency on `cfg-if` 0.1 and 1.0, so you may want to wait for https://github.com/rust-lang/log/pull/417 to be merged and get a release.

```
$ cargo tree -d
cfg-if v0.1.10
├── encoding_rs v0.8.23
│   ├── encoding_rs_io v0.1.7
│   │   └── grep-searcher v0.1.7 (/home/joshua/src/rust/ripgrep/crates/searcher)
│   │       ├── grep v0.2.7 (/home/joshua/src/rust/ripgrep/crates/grep)
│   │       │   └── ripgrep v12.1.1 (/home/joshua/src/rust/ripgrep)
│   │       └── grep-printer v0.1.5 (/home/joshua/src/rust/ripgrep/crates/printer)
│   │           └── grep v0.2.7 (/home/joshua/src/rust/ripgrep/crates/grep) (*)
│   └── grep-searcher v0.1.7 (/home/joshua/src/rust/ripgrep/crates/searcher) (*)
└── log v0.4.11
    ├── globset v0.4.5 (/home/joshua/src/rust/ripgrep/crates/globset)
    │   ├── grep-cli v0.1.5 (/home/joshua/src/rust/ripgrep/crates/cli)
    │   │   └── grep v0.2.7 (/home/joshua/src/rust/ripgrep/crates/grep) (*)
    │   └── ignore v0.4.16 (/home/joshua/src/rust/ripgrep/crates/ignore)
    │       └── ripgrep v12.1.1 (/home/joshua/src/rust/ripgrep)
    ├── grep-cli v0.1.5 (/home/joshua/src/rust/ripgrep/crates/cli) (*)
    ├── grep-regex v0.1.8 (/home/joshua/src/rust/ripgrep/crates/regex)
    │   └── grep v0.2.7 (/home/joshua/src/rust/ripgrep/crates/grep) (*)
    ├── grep-searcher v0.1.7 (/home/joshua/src/rust/ripgrep/crates/searcher) (*)
    ├── ignore v0.4.16 (/home/joshua/src/rust/ripgrep/crates/ignore) (*)
    └── ripgrep v12.1.1 (/home/joshua/src/rust/ripgrep)

cfg-if v1.0.0
└── crossbeam-utils v0.8.0
    └── ignore v0.4.16 (/home/joshua/src/rust/ripgrep/crates/ignore) (*)
``` 

---

_Comment by @BurntSushi on 2020-10-16 14:01_

I'll give it a bit for a new release of `log` to come out. cc @KodrAus

But having a duplicate dependency on `cfg-if` is not so bad if necessary. It is very small.

---

_Comment by @jyn514 on 2020-10-16 14:11_

Let me also work on updating `encoding_rs` while I'm at it, then.

---

_Closed by @BurntSushi on 2020-11-02 15:52_

---

_Branch deleted on 2020-11-02 16:48_

---
