```yaml
number: 1393
title: "Put ignore's parallel walk support behind a feature gate"
type: pull_request
state: closed
author: nox
labels: []
assignees: []
base: master
head: ignore-parallel-feature
created_at: 2019-09-29T17:56:00Z
updated_at: 2020-02-17T21:25:51Z
url: https://github.com/BurntSushi/ripgrep/pull/1393
synced_at: 2026-01-12T18:23:13Z
```

# Put ignore's parallel walk support behind a feature gate

---

_@nox_

We would like to use the ignore crate in [mozjs_sys](https://github.com/servo/mozjs) to tell cargo to rebuild
SpiderMonkey on filesystem changes in its own sources, but we don't
want to bring crossbeam-channel and whatnot in the build for such a trivial
operation.

---

_Comment by @nox on 2019-09-30 09:28_

I don't understand the build failure.

---

_Comment by @BurntSushi on 2020-02-17 21:25_

Thanks @nox! I'm going to close this PR now for procedural reasons, but I'm not necessarily opposed to it:

1. Firstly, are you still interested in using `ignore`? Apologies for taking so long to respond to this, but I was on a bit of a hiatus.
2. Secondly, I'm a little confused about avoiding `crossbeam-channel` here. `crossbeam-channel` is one of the lighter weight dependencies of `ignore`, and itself does not bring in much else. `regex`, `globset` and friends are much heavier. So it seems a little weird to me to go through all this trouble just to cut out `crossbeam-channel`. Maybe you could say a bit more about this?

Thanks!

---

_Closed by @BurntSushi on 2020-02-17 21:25_

---
