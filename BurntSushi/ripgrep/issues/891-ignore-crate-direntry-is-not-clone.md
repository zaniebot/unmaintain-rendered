```yaml
number: 891
title: "ignore crate: DirEntry is not Clone"
type: issue
state: closed
author: stevedonovan
labels: []
assignees: []
created_at: 2018-04-21T13:47:12Z
updated_at: 2018-04-21T17:35:12Z
url: https://github.com/BurntSushi/ripgrep/issues/891
synced_at: 2026-01-12T16:13:22Z
```

# ignore crate: DirEntry is not Clone

---

_@stevedonovan_

Not a ripgrep issue, so excuse me _ignoring_ the standard boilerplate.

`ignore::walk::DirEntry` cannot be cloned, which is an issue if I want to use it in [findr](https://github.com/stevedonovan/findr).  `findr` uses the most excellent embedded mini-scripting language Rhai, and Rust structs must be clonable to inter-operate with it.

I've been using a combination of `walkdir` (which does provide a clonable `DirEntry`) and the `gitignore` crate.  In effect, I have to reproduce a (subset) of `ignore` functionality in `findr` - it would be rather nice to offload that to a crate that already knows about directory iteration that can ignore non-essential files. 

Edit: Had a look - not so trivial, since `Error` must be `Clone` and `std::io::Error` is not....

---

_Closed by @BurntSushi on 2018-04-21 16:01_

---

_Comment by @BurntSushi on 2018-04-21 16:11_

This is now on crates.io in `ignore 0.4.2`.

---

_Comment by @stevedonovan on 2018-04-21 17:35_

Cool, thanks!

---
