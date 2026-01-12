```yaml
number: 1146
title: "Move from `trim_right` to `trim_end`"
type: pull_request
state: closed
author: jwbowen
labels: []
assignees: []
base: master
head: trim_right-to-trim_end
created_at: 2018-12-21T02:06:48Z
updated_at: 2018-12-21T02:17:11Z
url: https://github.com/BurntSushi/ripgrep/pull/1146
synced_at: 2026-01-12T18:23:13Z
```

# Move from `trim_right` to `trim_end`

---

_@jwbowen_

The `trim_right` method is deprecated in favor of `trim_end` for
`std::string::String` to more naturally support right-to-left
languages. The `trim_end` method was introduced in Rust 1.30.0, and the
current stable version is 1.31.1, so it feels safe to make the change
at this point in time.

---

_Comment by @BurntSushi on 2018-12-21 02:10_

Thank you, but the code churn isn't worth it at this time. I don't know whether there will be a ripgrep 0.10 patch release or not, which must continue to compile on the same Rust version as 0.10.0. Feel free to re-submit once ripgrep has bumped its minimum Rust version though!

---

_Closed by @BurntSushi on 2018-12-21 02:10_

---

_Branch deleted on 2018-12-21 02:17_

---
