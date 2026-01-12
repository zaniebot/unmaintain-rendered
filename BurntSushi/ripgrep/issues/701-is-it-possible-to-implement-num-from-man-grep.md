```yaml
number: 701
title: is it possible to implement -NUM (from man grep)
type: issue
state: closed
author: sitaramc
labels: []
assignees: []
created_at: 2017-12-01T04:09:43Z
updated_at: 2017-12-01T11:45:43Z
url: https://github.com/BurntSushi/ripgrep/issues/701
synced_at: 2026-01-12T16:13:22Z
```

# is it possible to implement -NUM (from man grep)

---

_@sitaramc_

is it possible to implement -NUM; i.e., make the "-C" optional, as it is in grep.

Please feel free to say "no".   It's mainly muscle memory that is causing me to ask, nothing more serious.

---

_Comment by @okdana on 2017-12-01 05:01_

As far as i can tell, this is hard to achieve with `clap` (the argument-parsing library `rg` uses).

The way you would do this with a traditional `getopt()` sort of method is designate `-0` through `-9` as valid short options and then basically concatenate them together in order of appearance, resetting them whenever one appears that's not adjacent to a previous one in the same argument. Then you convert the resulting string into a number.

`clap` is designed to be a much higher-level method of argument parsing, so it abstracts away things like the order and argument index that options appear in. In other words, there is no clean way AFAIK to tell the difference between `rg -12`, `rg -21`, `rg -1 -i -2`, and so on. I'm not an expert on `clap`'s quite extensive feature set, but i'm *pretty* sure that's the case, at least.

Maybe someone else knows something that i don't, but if not, this might have to be a [`clap` feature request](https://github.com/kbknapp/clap-rs/issues/new). It does seem like something that should reasonably be within its scope.

---

_Comment by @sitaramc on 2017-12-01 09:20_

I understand; thanks for the detailed explanation.  (I'll pass on pushing this up to the clap project, at least for now.)

---

_Closed by @sitaramc on 2017-12-01 09:20_

---

_Comment by @BurntSushi on 2017-12-01 11:45_

Huh. I didn't even know this was a thing in GNU grep. If clap had a way to do this, then there would probably have to be an option here to do it: https://docs.rs/clap/2.28.0/clap/struct.Arg.html I don't think such an option exists.

To be honest, I'm not sure I'd even be on board with this even if clap did support it. :)

---
