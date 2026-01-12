```yaml
number: 138
title: "Feature request: --files-without-matches"
type: issue
state: closed
author: mernen
labels:
  - enhancement
assignees: []
created_at: 2016-09-30T13:24:25Z
updated_at: 2023-01-06T16:18:26Z
url: https://github.com/BurntSushi/ripgrep/issues/138
synced_at: 2026-01-12T16:13:21Z
```

# Feature request: --files-without-matches

---

_@mernen_

One flag that occasionally comes useful is to identify files that _don't_ match a certain expression. Both `ag` and `ack` have `-L`/`--files-without-matches` (effectively the opposite of `-l`/`--files-with-matches`).

One minor problem here is that `rg -L` is already taken for following symlinks.

Curiously, `ag -lv` appears to be a synonym of `-L`. I'm not sure whether this is intentional or not, but it seems like a perfectly reasonable approach to me — in fact, years ago, before I knew of `-L`, this is actually what I first tried with `ack`, and got bogus results (it interprets this combo as "list all files that have at least one line that does not match the given expression", like ripgrep does today). Personally, I don't think I've ever had a use case for this.


---

_Label `enhancement` added by @BurntSushi on 2016-09-30 13:29_

---

_Comment by @BurntSushi on 2016-09-30 13:32_

I'm not opposed to adding `--files-without-matches`, but I don't think we need to add a short alias for it. I'd at least like to keep `-L` for following symlinks.

I'm also not opposed to making `-v` a bit smarter with respect to whether `-l` is given or not, but I worry that it will make "find files with a line that doesn't match" impossible to do.


---

_Comment by @andyleejordan on 2016-10-18 18:41_

Ha, I almost added this earlier @mernen but ran into exactly the problem you described 😄 


---

_Comment by @BurntSushi on 2016-11-19 15:19_

OK, so I did some more thinking about this and I think the _current_ behavior of `-lv` is probably the right one. I totally get that it can be confusing, but `-v` pretty solidly refers to matching at the line level and isn't a generic "do the opposite" flag. In particular, `grep -lv` behaves the same as `rg -lv`.

Here is a proposed specification.

Add a new flag `--files-without-matches`. Only show the path of each file that contains zero matches.


---

_Label `help wanted` added by @BurntSushi on 2016-11-19 15:19_

---

_Label `help wanted` removed by @BurntSushi on 2016-11-19 15:19_

---

_Closed by @BurntSushi on 2016-11-20 01:05_

---

_Comment by @hauntsaninja on 2020-01-30 02:54_

(If, like me, you're coming here from Google, the flag that made it into ripgrep is `--files-without-match`)

---

_Comment by @sankalp-khare on 2022-11-03 13:18_

I keep coming back here to re-check what the flag is 😢 

---

_Comment by @jeremyfiel on 2023-01-06 16:09_

how can we use this flag in vs code search?

---

_Comment by @BurntSushi on 2023-01-06 16:18_

This is the issue tracker for ripgrep. Your question is about VS Code UX. You are in the wrong place.

---
