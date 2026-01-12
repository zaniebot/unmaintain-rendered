```yaml
number: 1252
title: publicize the --pre (and --pre-glob) flags a bit more
type: issue
state: closed
author: BurntSushi
labels:
  - doc
assignees: []
created_at: 2019-04-16T20:42:55Z
updated_at: 2020-05-09T03:24:44Z
url: https://github.com/BurntSushi/ripgrep/issues/1252
synced_at: 2026-01-12T16:13:23Z
```

# publicize the --pre (and --pre-glob) flags a bit more

---

_@BurntSushi_

In reference to: https://news.ycombinator.com/item?id=19675934

It sounds like while the `--pre` flag is a fairly niche feature, it can be _really_ useful in certain circumstances. Currently, the `--pre` flag isn't really advertised prominently anywhere, and it may not be obvious how to use it effectively.

The poster from HN graciously provided an example script they're using, which seems pretty neat. We might consider using it as an example:

https://gist.github.com/ColonolBuendia/314826e37ec35c616d70506c38dc65aa

---

_Label `doc` added by @BurntSushi on 2019-04-16 20:42_

---

_Comment by @ColonelBuendia on 2019-04-25 17:19_

I wrote some things about this at the aforementioned gist.  
Cut aggressively and feel free to copy over anything as needed.  

---

_Comment by @ronjouch on 2019-06-16 21:07_

Related work: [rga: ripgrep, but also search in PDFs, E-Books, Office documents, zip, tar.gz, etc](https://phiresky.github.io/blog/2019/rga--ripgrep-for-zip-targz-docx-odt-epub-jpg/), which is a frontend for `rg --pre`, plus archive traversal and caching. [HN discussion](https://news.ycombinator.com/item?id=20196982) from 2019/06/16.

---

_Comment by @ColonelBuendia on 2019-06-17 00:36_

If helpful, one additional use I would be willing to write up a bit is a highly ad-hoc full text search.  
More or less a loop of:  
`rg --pre rgpre "" "$1" > "$1".sanchopanda`.    
From there, lose all the other stuff and go back to the much more usable 
`rg whateverthehell  -g '*.sanchopanda'`  
until someone chooses to rerun the initial "indexer".  

This clearly falls outside the core functionality of code search, and has all it's own tradeoffs.  However while obvious, likely niche, and so close to the core functionality for a programmer, it's gibberish for everyone else, even the technically inclined, for whom it would be a boon.  

---

_Comment by @BurntSushi on 2019-06-17 00:44_

@ColonolBuendia If you wanted to give that a go, I'm sure we could find a place for it in the ripgrep docs. If it's closer to a very specific use case, then it might be a good FAQ item. If it's more like an explanation for how `--pre` works with some basic but useful examples, then it might be a good GUIDE section. I confess though, that I don't quite understand what you mean yet. :-)

---

_Comment by @ColonelBuendia on 2019-06-17 01:01_

Let me know how to edit the write up in the gist as it is now if its not very clear on the use of --pre.  As Pascal said, it would have been shorter if I had spent more time on it.  

In terms of a guide, that sounds right to me.  The documentation curerntly is highly convincing that rg is faster.  I'm not even sure the unconvinced on speed are convincable from here.   
But it's not very friendly to those who know very little about similair tools already.  Thats my reference point broadly, and for this flag in particular.   It's not your obligation to enlighten anyone certainly, but given the reach of ripgrep, it's an opportunity.  

edit:  The full text idea i just mentioned is an example of how to use ripgrep -pre in an interesting way  more than anything.  It's also a real thing I was able to do to kill an instance of microsoft search server 2010* that was running for a similair purpose.

*Apparantly, this is common.  It was made free, and that was that.  I imagine if you help kil enough of these, you get on the holiday card list for the ms security team forever.  

---

_Comment by @ColonelBuendia on 2019-11-19 14:21_

Noting that following a username change, the gist mentioned is now located here:
https://gist.github.com/ColonelBuendia/314826e37ec35c616d70506c38dc65aa

An updated usage is here: 
https://github.com/ColonelBuendia/rgpipe

---

_Closed by @BurntSushi on 2020-05-09 03:24_

---
