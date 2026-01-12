```yaml
number: 1836
title: rg should skip holes in files
type: issue
state: closed
author: umanwizard
labels:
  - wontfix
assignees: []
created_at: 2021-03-25T18:34:26Z
updated_at: 2021-03-25T18:55:34Z
url: https://github.com/BurntSushi/ripgrep/issues/1836
synced_at: 2026-01-12T16:13:24Z
```

# rg should skip holes in files

---

_@umanwizard_

Some files on some filesystems (e.g. ext4) can be sparse - they contain holes which are read as all zero bytes and occupy no space on disk.

```
$ truncate -s 1000G fake_big_file
$ du fake_big_file
0       fake_big_file
$ rg foo fake_big_file # this takes a long time, but could be made to take a short time
```

See e.g. this (never landed) PR to `ag` from 2015: https://github.com/ggreer/the_silver_searcher/pull/579

---

_Comment by @BurntSushi on 2021-03-25 18:55_

Unfortunately, I don't think this is a good fit for ripgrep. I note also that GNU grep doesn't appear to implement any hole-oriented optimizations either.

For the most part, I just don't see how to robustly implement something like this. Your implementation in `ag`, for example, seems tightly coupled to the memory map method of searching. Memory maps don't work for all files. Moreover, when the "skip holes" flag is enabled, there is a non-trivial additional amount of overhead for every file searched.

With that said, I don't think you're stuck. I suspect that you would be able to write a preprocessor program that did this handling for you. See the section in the guide on the `--pre` flag: https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#preprocessor

---

_Closed by @BurntSushi on 2021-03-25 18:55_

---

_Label `wontfix` added by @BurntSushi on 2021-03-25 18:55_

---
