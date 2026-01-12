```yaml
number: 1447
title: Implement FromStr for Glob
type: pull_request
state: closed
author: Larusso
labels:
  - rollup
assignees: []
base: master
head: implement/from_str_for_glob
created_at: 2019-12-16T19:26:04Z
updated_at: 2020-02-17T22:16:35Z
url: https://github.com/BurntSushi/ripgrep/pull/1447
synced_at: 2026-01-12T18:23:13Z
```

# Implement FromStr for Glob

---

_@Larusso_

The `globset::Glob` type [`new`] function creates a new value with an `&str` parameter which returns an `Result<Glob, Error>` object. This is exactly what [`std::str::FromStr::from_str`][`std::str::FromStr`] defines. Libraries like [`clap`] use [`std::str::FromStr`] to create objects from provided commandline arguments. This change makes this library usable without a newtype wrapper.

[`std::str::FromStr`]: 	https://doc.rust-lang.org/std/str/trait.FromStr.html
[`clap`]:		https://docs.rs/clap/2.33.0/clap/macro.value_t.html
[`new`]:		https://docs.rs/globset/0.4.4/globset/struct.Glob.html#method.new

---

_@BurntSushi approved on 2020-02-17 00:46_

Thanks!

---

_Label `rollup` added by @BurntSushi on 2020-02-17 00:47_

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
