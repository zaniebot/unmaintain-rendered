```yaml
number: 2314
title: Include/exclude searched files with implicit wildcard or regex (ag -G)
type: issue
state: open
author: oandrew
labels:
  - enhancement
  - question
assignees: []
created_at: 2022-09-28T18:22:03Z
updated_at: 2023-10-26T06:35:39Z
url: https://github.com/BurntSushi/ripgrep/issues/2314
synced_at: 2026-01-12T16:13:24Z
```

# Include/exclude searched files with implicit wildcard or regex (ag -G)

---

_@oandrew_

#### Describe your feature request

First, thanks for ripgrep.

There is one feature in silversearcher (ag) that's holding me back - filter file paths with regex e.g. `ag <pattern> -G <regex>`.
ripgrep seems to support only globs (I'm guessing for performance reasons?)
- This means in most cases (partial match somewhere in the path) you have to type extra characters (wildcard + quotes):
```
$ rg -g '*test*' func
$ ag -G test func
```
- some things are not possible to express due to limitations of globs.

99% of the time I simply use it to do a partial match, so having something like  `rg -G test func` as shortcut to `rg -g '*test*' func` would totally resolve it for me. 

**So this boils down to:**
1.  (simple) Could we have a new flag `-G <glob>` that's similar to `-g <glob>` but `<glob>` is implicitly surrounded with wildcards? Then you could avoid typing and escaping `*` in most cases. 
e.g. `rg -G test func` would be equivalent to `rg -g '*test*' func`
3. (complex) `rg -G <pattern>` could accept a regex which would allow other use cases in addition to partial match. 

Would you consider a PR for the first approach?

Thanks!


---

_Comment by @BurntSushi on 2022-09-28 18:36_

It would be good to search the tracker first... :-) Dupe of #48, #54, #75, #91 (lots of discussion here), #193, #285.

There's just so many different ways to already do this that I don't see the point of adding a new flag for it. `rg --files | rg test` can be easily put into an alias or a wrapper shell script. Here's mine:

https://github.com/BurntSushi/dotfiles/blob/2f58eedf3b7f7dae7f0a7cea1a641459e25e5d07/.aliasrc#L111-L121

You could also use a purpose driven tool specifically for searching by file name: https://github.com/sharkdp/fd/

---

_Closed by @BurntSushi on 2022-09-28 18:36_

---

_Label `duplicate` added by @BurntSushi on 2022-09-28 18:36_

---

_Label `wontfix` added by @BurntSushi on 2022-09-28 18:36_

---

_Comment by @oandrew on 2022-09-28 18:39_

@BurntSushi  I wasn't referring to file search, but filtering files included in result (`rg --glob`). I searched the tracker but didn't find anything similar.

Updated the title to be less confusing


---

_Renamed from "Filter files with implicit wildcard or regex (ag -G)" to "Include/exclude files with implicit wildcard or regex (ag -G)" by @oandrew on 2022-09-28 18:41_

---

_Renamed from "Include/exclude files with implicit wildcard or regex (ag -G)" to "Include/exclude searched files with implicit wildcard or regex (ag -G)" by @oandrew on 2022-09-28 18:44_

---

_Comment by @BurntSushi on 2022-09-28 18:44_

Ooooo, I see. Sorry, I missed that.

I'll re-open this for now. I don't have time to respond, but I'm not quite inclined to accept this, at least as of now. ripgrep needs its filtering internals (and interface) overhauled at some point, so I'd rather not add anything substantially new to it as of now.

---

_Reopened by @BurntSushi on 2022-09-28 18:44_

---

_Label `duplicate` removed by @BurntSushi on 2022-09-28 18:44_

---

_Label `wontfix` removed by @BurntSushi on 2022-09-28 18:44_

---

_Label `enhancement` added by @BurntSushi on 2022-09-28 18:44_

---

_Label `question` added by @BurntSushi on 2022-09-28 18:44_

---

_Comment by @relicode on 2023-10-26 06:35_

Minimalistic POSIX-compatible script with wildcard support:
```sh
#!/bin/sh

PATTERN="$1"
shift

(for DIR in "${@:-.}"; do rg --files "$DIR"; done) | rg "$PATTERN"
```

---
