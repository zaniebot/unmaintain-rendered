```yaml
number: 247
title: "-H makes no difference"
type: issue
state: closed
author: TheRealTroff
labels:
  - doc
assignees: []
created_at: 2016-11-23T15:39:46Z
updated_at: 2016-11-28T22:40:54Z
url: https://github.com/BurntSushi/ripgrep/issues/247
synced_at: 2026-01-12T18:23:11Z
```

# -H makes no difference

---

_@TheRealTroff_

```
$ rg ripgrep
rg.1
24:ripgrep (rg) combines the usability of The Silver Searcher (an ack
27:Project home page: https://github.com/BurntSushi/ripgrep
269:This is enabled by default when ripgrep thinks it will be faster.
345:Show the version number of ripgrep and exit.
368:inside of ripgrep.
380:ripgrep.
```

```
$ rg -H ripgrep
rg.1
24:ripgrep (rg) combines the usability of The Silver Searcher (an ack
27:Project home page: https://github.com/BurntSushi/ripgrep
269:This is enabled by default when ripgrep thinks it will be faster.
345:Show the version number of ripgrep and exit.
368:inside of ripgrep.
380:ripgrep.
```

In contrast:

```
$ grep -H -n ripgrep *
rg.1:24:ripgrep (rg) combines the usability of The Silver Searcher (an ack
rg.1:27:Project home page: https://github.com/BurntSushi/ripgrep
rg.1:269:This is enabled by default when ripgrep thinks it will be faster.
rg.1:345:Show the version number of ripgrep and exit.
rg.1:368:inside of ripgrep.
rg.1:380:ripgrep.
```

This makes the output much less pleasant to parse.

---

_Comment by @BurntSushi on 2016-11-23 16:02_

This seems like intended behavior. The `-H` flag simply controls whether to show the file name or not. When searching multiple files, ripgrep enables `-H` by default. Compare the output of `rg ripgrep rg.1` and `rg ripgrep rg.1 -H`, for example.

It would be much more helpful if you included the problem you're trying to solve. If you want output like grep, then you get it for free automatically if you pipe to another command or redirect stdout. Alternatively, use the `--no-heading` flag.

---

_Comment by @TheRealTroff on 2016-11-23 16:06_

Fair enough, that works, but that's not how I read the documentation of -H.

---

_Comment by @BurntSushi on 2016-11-23 16:07_

@TheRealTroff Could you suggest a better phrasing?

---

_Label `doc` added by @BurntSushi on 2016-11-23 16:07_

---

_Comment by @TheRealTroff on 2016-11-23 16:18_

Actually, no. You've conflated -H with --heading. The behaviour of -H is that described by --heading as far as I can tell. If I remove the "prefix each match..." I don't see a point to have -H at all.

---

_Comment by @BurntSushi on 2016-11-23 16:24_

The behavior of `-H` is precisely the same as it is in grep, so I think you're going to have to clarify further. `-H` has very little utility. It's essentially only useful to enforce consistent output regardless of the number of arguments. e.g., `grep foo bar` won't show the file name in matches but `grep -H foo bar` will. The same is true for `rg`.

`--heading`/`--no-heading` is about grouping results.

---

_Comment by @TheRealTroff on 2016-11-23 16:25_

I've already shown that the behavior is different in the original post? I've updated it for better formatting.

---

_Comment by @BurntSushi on 2016-11-23 16:32_

@TheRealTroff How so? In all cases, a file name is shown.

---

_Comment by @BurntSushi on 2016-11-27 00:17_

@TheRealTroff Is there something we can add to the docs that will make this clearer? (I can take a crack at it, but I'd consider something from you much more valuable!)

---

_Closed by @BurntSushi on 2016-11-28 22:40_

---
