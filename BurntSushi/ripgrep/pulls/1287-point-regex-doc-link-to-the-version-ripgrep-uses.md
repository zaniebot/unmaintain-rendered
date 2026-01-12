```yaml
number: 1287
title: Point regex doc link to the version ripgrep uses
type: pull_request
state: merged
author: skierpage
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2019-05-28T21:20:25Z
updated_at: 2019-06-01T12:44:56Z
url: https://github.com/BurntSushi/ripgrep/pull/1287
synced_at: 2026-01-12T18:23:13Z
```

# Point regex doc link to the version ripgrep uses

---

_@skierpage_

Cargo.toml says regex 1.0.5, and its doc is different, e.g. adds "symmetric differences" under https://docs.rs/regex/1.0.5/regex/#character-classes

---

_Review comment by @BurntSushi on `GUIDE.md`:113 on 2019-05-29 10:59_

Nice catch. Could you please use `*` here instead of `1.0.5` though? In particular, the `Cargo.toml` merely specifies the minimal version of regex that ripgrep can compile with. The [actual version used is in `Cargo.lock`](https://github.com/BurntSushi/ripgrep/blob/d1e4d28f3090cd0ce6e2eb7931e6b897e9235c30/Cargo.lock#L766). I'm pretty good about keeping dependencies up to date, so I think just having the docs link to the latest version in the README is the most convenient choice here.

---

_@BurntSushi requested changes on 2019-05-29 11:00_

Thanks! Nice catch. One small nit though.

---

_@skierpage reviewed on 2019-05-31 23:16_

---

_Review comment by @skierpage on `GUIDE.md`:113 on 2019-05-31 23:16_

Done, I think (I've never updated a GitHub pull request, thanks for your patience).

I also made a [pull request for docs.rs About ](https://github.com/rust-lang/docs.rs/pull/362) to mention *|latest|newest works in links, that was news to me :smiley:.

---

_@BurntSushi approved on 2019-06-01 12:44_

Thanks!

---

_Merged by @BurntSushi on 2019-06-01 12:44_

---

_Closed by @BurntSushi on 2019-06-01 12:44_

---
