```yaml
number: 366
title: "Zero length matches don't colourise properly"
type: issue
state: closed
author: filterfish
labels:
  - wontfix
assignees: []
created_at: 2017-02-17T08:07:43Z
updated_at: 2017-02-19T13:24:09Z
url: https://github.com/BurntSushi/ripgrep/issues/366
synced_at: 2026-01-12T16:13:21Z
```

# Zero length matches don't colourise properly

---

_@filterfish_

I use this little grep command to highlight keywords, in for example, logs (or in this case packages):

```bash
apt-cache search cudf | grep -E --colour '^|cudf'
```
When I use `rg '^|cudf'` it doesn't colourise the matches that match `^cudf` which is incorrect.

Anyway I've attached two screenshots to show the difference (I couldn't work out how to set the colours in Markdown!). Note the difference in brightness  is simply due to my compositor settings and nothing to do do with this issue.

grep: 
![colours-grep](https://cloud.githubusercontent.com/assets/12899/23057368/f3f594c4-f543-11e6-8b18-b599ee312276.png)

rg:

![colours-rg](https://cloud.githubusercontent.com/assets/12899/23057376/029fe128-f544-11e6-9630-ae38289a1610.png)



---

_Comment by @BurntSushi on 2017-02-17 12:01_

Interesting. This is an artifact of the regex engine and I'm not sure there's much that can be done. In particular, the regex engine has *leftmost first* matching semantics (which means that the first branch in an alternation that matches is used), which tends to match the behavior that most folks expect because they are the default matching semantics of backtracking engines. Most finite automata based engines provide *leftmost longest* matching semantics (which means that the branch in an alternation that matches the most text is used), including the one inside of GNU grep. For example, while you get your desired results with `grep -E`, if you try `grep -P` (which uses PCRE), you will see the same behavior as ripgrep.

I think the only work-around available to you is to encode the match priority you want. For example, instead of `rg '^|cudf` use `rg 'cudf|^'`, which does what you want.

---

_Closed by @BurntSushi on 2017-02-17 12:01_

---

_Label `wontfix` added by @BurntSushi on 2017-02-17 12:01_

---

_Comment by @filterfish on 2017-02-19 08:07_

Blimey. And there was me thinking I had an OK understanding of regexes. Clearly I was way of the mark!
Thanks for the in-depth reason as to why and the workaround; I hadn't thought of that.

---

_Comment by @BurntSushi on 2017-02-19 13:24_

Yeah, match priority in alternations is tricky business. It is occasionally useful though. :-)

---
