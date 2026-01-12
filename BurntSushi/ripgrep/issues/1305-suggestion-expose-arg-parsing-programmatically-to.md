```yaml
number: 1305
title: "Suggestion: Expose arg parsing programmatically / to libripgrep"
type: issue
state: closed
author: phiresky
labels:
  - wontfix
assignees: []
created_at: 2019-06-18T09:50:24Z
updated_at: 2019-06-18T10:36:45Z
url: https://github.com/BurntSushi/ripgrep/issues/1305
synced_at: 2026-01-12T16:13:23Z
```

# Suggestion: Expose arg parsing programmatically / to libripgrep

---

_@phiresky_

In [rga](https://github.com/phiresky/ripgrep-all), I currently pass most arguments directly to ripgrep. But now I noticed I need to handle some of those flags myself, currently specifically --text, --binary, --null-data, --encoding, and their short versions to fix binary detection and [handle encodings](https://github.com/phiresky/ripgrep-all/issues/5).

In other words, a command given with `--pre` might also want to know about certain configuration options given to ripgrep itself.

If there was a way to access the clap App or even better the higher level configuration of ripgrep given a Vec<OsStr> of args, that would be great!

Currently, it looks like all of the arg parsing is in the ripgrep crate itself, which can't be loaded as a library, as far as I can tell.

---

_Comment by @BurntSushi on 2019-06-18 10:36_

Sorry, but I don't have any plans to do this. ripgrep core should stay a binary crate. I'd suggest either parsing what you need from the process arguments directly, or designing your own command line argument parameters and invoke libripgrep directly.

It is a non-goal of mine to make ripgrep arbitrarily flexible, and this is a step too far in that direction. Making things too flexible increases maintenance burden and the complexity of the code.

---

_Closed by @BurntSushi on 2019-06-18 10:36_

---

_Label `wontfix` added by @BurntSushi on 2019-06-18 10:36_

---
