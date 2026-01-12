```yaml
number: 1636
title: "cli: add colored output"
type: pull_request
state: closed
author: pickfire
labels: []
assignees: []
base: master
head: patch-1
created_at: 2020-07-07T08:04:06Z
updated_at: 2020-07-07T13:01:32Z
url: https://github.com/BurntSushi/ripgrep/pull/1636
synced_at: 2026-01-12T18:23:14Z
```

# cli: add colored output

---

_@pickfire_

_No description provided._

---

_Comment by @BurntSushi on 2020-07-07 11:34_

This doesn't (or shouldn't) work, because [ripgrep disables clap's color feature](https://github.com/BurntSushi/ripgrep/blob/ffd4c9ccba0ffc74270a8d3ae75f11a7ba7a1a64/Cargo.toml#L55-L58) which avoids bringing in a [second dependency for handling colors in ripgrep's dependency tree](https://github.com/clap-rs/clap/tree/v2.33.1#features-enabled-by-default).

We could reconsider this once ripgrep moves to clap 3, which IIRC, uses `termcolor` for dealing with colors instead of `ansi_term`, which is the same crate that ripgrep uses.

---

_Closed by @BurntSushi on 2020-07-07 11:34_

---

_Branch deleted on 2020-07-07 13:01_

---
