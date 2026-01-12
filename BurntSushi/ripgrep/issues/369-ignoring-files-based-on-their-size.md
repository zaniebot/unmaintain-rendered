```yaml
number: 369
title: Ignoring files based on their size
type: issue
state: closed
author: mensfeld
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2017-02-18T15:58:50Z
updated_at: 2017-03-12T20:59:21Z
url: https://github.com/BurntSushi/ripgrep/issues/369
synced_at: 2026-01-12T16:13:21Z
```

# Ignoring files based on their size

---

_@mensfeld_

Hey, is there an option to use ripgrep and exclude files based on their size? I have a lot of +10MB text files that I don't want to grep and would love to be able to exclude them all. Thank you.

---

_Comment by @BurntSushi on 2017-02-18 16:00_

No. Can you use some other attribute of the text files? You can specify paths to ignore at the command line with `-g '!dir/to/ignore'` or `-g '!*.txt'`.

---

_Label `question` added by @BurntSushi on 2017-02-18 17:19_

---

_Comment by @mensfeld on 2017-02-18 17:34_

Hello @BurntSushi. Thank you for such fast reaction. Unfortunately I can't. I use rg as a part in a "chain" where I get some locations and I need to do search based on that. If it is not possible, I will just wrap it around with a shell script with a list of dirs/files to ignore.

---

_Comment by @BurntSushi on 2017-02-18 17:39_

It is indeed not possible right now. It would have to be explicitly added to ripgrep as a new flag. Which I'd be OK with, since it should be relatively easy to add. It will have a performance cost, but that seems fine since the end user would have to specifically ask for it anyway.

---

_Label `enhancement` added by @BurntSushi on 2017-02-18 17:39_

---

_Label `help wanted` added by @BurntSushi on 2017-02-18 17:39_

---

_Label `question` removed by @BurntSushi on 2017-02-18 17:39_

---

_Comment by @mensfeld on 2017-02-18 17:44_

@BurntSushi - yes it would have a perf. impact but only when needed which shouldn't be that bad. For now I will just find those files on my own and exclude them. Thank you

---

_Comment by @tiehuis on 2017-02-23 10:39_

I'll probably end up looking at implementing this over the next few days. Just to outline the intended approach from a quick glance before beginning.

 - Add a new size matcher into `Ignore` and co in the `ignore` crate (in file `dir.rs`)
 - Add the size check into `matched` (in file `dir.rs`)
 - Add the top-level options into all the required locations
 - Add some tests

There probably should be some nice intuitive values expected for the command line option, too (i.e. `--limit-size=2K` `--limit-size=1G`).



---

_Comment by @BurntSushi on 2017-02-23 11:47_

> Add a new size matcher into Ignore and co in the ignore crate (in file dir.rs)

I think the `ignore` crate is probably the right place, but it feels like the filter should be implemented in `walk.rs`, not `dir.rs`. The stuff in `dir.rs` is pretty tightly coupled to glob matching, where as the stuff in `walk.dir` is generalized to recursive directory walking. Note that you will have to implement the check on both the single threaded and parallel walkers.

> There probably should be some nice intuitive values expected for the command line option, too (i.e. --limit-size=2K --limit-size=1G).

That seems like nice extra credit. Feel free to punt on that if you like. :-)

---

_Comment by @BurntSushi on 2017-03-12 20:59_

Fixed in https://github.com/BurntSushi/ripgrep/pull/385

---

_Closed by @BurntSushi on 2017-03-12 20:59_

---
