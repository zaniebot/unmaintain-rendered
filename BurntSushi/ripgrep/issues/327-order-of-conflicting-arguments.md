```yaml
number: 327
title: Order of conflicting arguments
type: issue
state: closed
author: leahneukirchen
labels:
  - question
assignees: []
created_at: 2017-01-16T13:33:37Z
updated_at: 2017-02-18T20:25:33Z
url: https://github.com/BurntSushi/ripgrep/issues/327
synced_at: 2026-01-12T16:13:21Z
```

# Order of conflicting arguments

---

_@leahneukirchen_

I generally prefer the `--no-heading` output, but there is no shortcut to enable it.  After adding an alias for `rg --no-heading`, I discovered `rg --no-heading --heading` behaves just like `rg --no-heading`.
It would be nice if the "last argument wins", so defaults can be defined yet changed easily.


---

_Comment by @BurntSushi on 2017-01-16 13:36_

I'm not sure if we can.

@kbknapp Is there any way to make this type of thing happen with existing clap?

---

_Label `question` added by @BurntSushi on 2017-01-16 13:36_

---

_Comment by @leahneukirchen on 2017-01-16 14:40_

The clap README mentions `POSIX Compatible Conflicts/Overrides`

---

_Comment by @kbknapp on 2017-01-16 15:25_

In either `heading` or `no-heading` use `Arg::overrides_with(other)` it's meant for exactly that purpose (allowing alias overrides).

I'm on mobile but when I get to a computer I can link to the docs page.

Edit: [docs page](https://docs.rs/clap/2.20.0/clap/struct.Arg.html#method.overrides_with)

---

_Comment by @BurntSushi on 2017-01-16 15:27_

Oh excellent! That sounds familiar. I'll try it out soon.

---

_Closed by @BurntSushi on 2017-02-18 20:25_

---
