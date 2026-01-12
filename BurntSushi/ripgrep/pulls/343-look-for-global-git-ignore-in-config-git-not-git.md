```yaml
number: 343
title: Look for global git/ignore in ~/.config/git, not ~/git
type: pull_request
state: merged
author: blueyed
labels: []
assignees: []
merged: true
base: master
head: home-config-for-git-ignore
created_at: 2017-01-29T15:33:39Z
updated_at: 2017-01-30T22:06:22Z
url: https://github.com/BurntSushi/ripgrep/pull/343
synced_at: 2026-01-12T18:23:12Z
```

# Look for global git/ignore in ~/.config/git, not ~/git

---

_@blueyed_

The documentation says:

> If `$XDG_CONFIG_HOME` is not set or is empty, then
> `$HOME/.config/git/ignore` is used instead.

This is the expected behavior, but the code looked at ~/git/ignore
instead.

This is my first Rust code, so it might be wrong..!

---

_Review comment by @BurntSushi on `ignore/src/gitignore.rs` on 2017-01-29 15:37_

I think you need `env::home_dir().map(|p| p.join(".config"))` since `env::home_dir()` returns an `Option<PathBuf>`.

---

_@BurntSushi requested changes on 2017-01-29 15:37_

Ah thanks for catching this! This does appear to be a bug. I think you just need to fix the compile error and this should be good to go. Thanks!

---

_@blueyed reviewed on 2017-01-29 15:52_

---

_Review comment by @blueyed on `ignore/src/gitignore.rs` on 2017-01-29 15:52_

That worked, thanks!

---

_@BurntSushi approved on 2017-01-30 21:58_

---

_Merged by @BurntSushi on 2017-01-30 21:59_

---

_Closed by @BurntSushi on 2017-01-30 21:59_

---

_Branch deleted on 2017-01-30 22:06_

---
