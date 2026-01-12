```yaml
number: 203
title: "RFE: print the program's official name ('ripgrep') in -h/--version output"
type: issue
state: closed
author: siebenmann
labels:
  - doc
assignees: []
created_at: 2016-10-30T01:01:10Z
updated_at: 2016-11-06T17:21:39Z
url: https://github.com/BurntSushi/ripgrep/issues/203
synced_at: 2026-01-12T18:23:11Z
```

# RFE: print the program's official name ('ripgrep') in -h/--version output

---

_@siebenmann_

Right now, I need to remember to update ripgrep every so often because it keeps improving (which is great). But I use ripgrep as `rg`, which means that every time I want to check for updates I have to remember what the program's official name is so that I can hunt it down. So I think it would be convenient if one or both of `rg --version` and `rg --help` mentioned that the official full name was 'ripgrep'. It would be great if `rg --help` also mentioned the master repository URL.

(The manpage mentions that rg is ripgrep, but I don't always remember to put the manpage somewhere that `man rg` can find it because I only put the `rg` command in my personal `$HOME/bin` directory. And even the manpage doesn't mention authorship information or the master repository URL.)


---

_Label `doc` added by @BurntSushi on 2016-10-30 01:03_

---

_Comment by @BurntSushi on 2016-10-30 01:03_

Thanks for filing this. I agree it's important to get these little details right. I'll try to knock this off before the next release.


---

_Comment by @BurntSushi on 2016-10-30 01:05_

FWIW, I would have happily made the command itself called `ripgrep` and insisted that folks alias it to a shorter command, because I don't really like having dual names (`rg`/`ripgrep`) for the same thing. Alas, with the way the world works, I actually think this would have been a significant hurdle to adoption. ("because it takes too long to type `ripgrep`")


---

_Closed by @BurntSushi on 2016-11-06 17:21_

---
