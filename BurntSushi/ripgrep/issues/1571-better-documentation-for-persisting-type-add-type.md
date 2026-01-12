```yaml
number: 1571
title: Better documentation for persisting --type-add/--type-clear using config files.
type: issue
state: closed
author: llamasoft
labels:
  - enhancement
  - doc
assignees: []
created_at: 2020-05-07T20:28:26Z
updated_at: 2020-05-09T03:24:43Z
url: https://github.com/BurntSushi/ripgrep/issues/1571
synced_at: 2026-01-12T16:13:23Z
```

# Better documentation for persisting --type-add/--type-clear using config files.

---

_@llamasoft_

A while back I discovered that ripgrep supports a `--type-add` option and I was quite excited.  I find myself using the same glob patterns pretty regularly, so having a way to define common patterns seemed like it would save me a tremendous amount of time... until I saw that "Type settings are NOT persisted".  
I found this rather disappointing because it meant that `--type-add` would only ever save time if I wrapped `rg` in an alias or function.  
It wasn't until much later (today in fact!) that I saw that ripgrep supports configuration files.  This effectively means that I _can_ persist my type definitions.

Given the severe limitation of type definitions not being automatically persisted, I would strongly suggest updating the help text/manpages of `--type-add` and `--type-clear` to also mention the existence of using a configuration file as a workaround.  Yes, the configuration file guide uses `--type-add` as an example, but most users may not realize that a configuration file solution exists if something doesn't point them directly to it.

In summary, I suggest changing this line:

> Note that this MUST be passed to every invocation of ripgrep. Type settings are NOT persisted.

Into something like this:

> Note that this MUST be passed to every invocation of ripgrep. Type settings are NOT persisted.  See CONFIGURATION FILES for a workaround.

---

_Comment by @BurntSushi on 2020-05-07 21:26_

Sounds good. For such a small change, I'd encourage folks to submit a PR for it.

---

_Label `doc` added by @BurntSushi on 2020-05-07 21:26_

---

_Label `enhancement` added by @BurntSushi on 2020-05-07 21:26_

---

_Closed by @BurntSushi on 2020-05-09 03:24_

---
