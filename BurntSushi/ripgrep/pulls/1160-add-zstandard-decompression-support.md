```yaml
number: 1160
title: Add ZStandard decompression support
type: pull_request
state: closed
author: chipturner
labels: []
assignees: []
base: master
head: master
created_at: 2019-01-11T15:51:44Z
updated_at: 2019-01-23T01:57:57Z
url: https://github.com/BurntSushi/ripgrep/pull/1160
synced_at: 2026-01-12T18:23:13Z
```

# Add ZStandard decompression support

---

_@chipturner_

Tests pass:

```
$ cargo test misc::compressed_zstd
    Finished dev [unoptimized + debuginfo] target(s) in 0.08s
     Running target/debug/deps/rg-404897210952ac78

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured; 2 filtered out

     Running target/debug/deps/integration-fc86fa5a18341e74

running 1 test
test misc::compressed_zstd ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured; 188 filtered out

```

As well as verification by cli.

Trunk:

```
$ rg -c -z Watson tests/data
tests/data/sherlock.bz2:2
tests/data/sherlock.gz:2
tests/data/sherlock.lz4:2
tests/data/sherlock.lzma:2
tests/data/sherlock.xz:2
```

Built with this change:
```
$ target/debug/rg -c -z Watson tests/data
tests/data/sherlock.gz:2
tests/data/sherlock.bz2:2
tests/data/sherlock.lz4:2
tests/data/sherlock.lzma:2
tests/data/sherlock.xz:2
tests/data/sherlock.zstd:2
```

---

_@BurntSushi approved on 2019-01-11 15:54_

Nice! Looks great!

---

_Comment by @okdana on 2019-01-11 15:54_

#1100 

---

_Comment by @BurntSushi on 2019-01-23 01:57_

Closed by #1100. Thanks for working on this!

---

_Closed by @BurntSushi on 2019-01-23 01:57_

---
