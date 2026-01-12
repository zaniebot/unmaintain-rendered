```yaml
number: 263
title: "implement `--sort-files` option"
type: issue
state: closed
author: daxim
labels:
  - question
assignees: []
created_at: 2016-12-01T11:12:45Z
updated_at: 2021-11-09T08:15:59Z
url: https://github.com/BurntSushi/ripgrep/issues/263
synced_at: 2026-01-12T16:13:21Z
```

# implement `--sort-files` option

---

_@daxim_

Ack has it. I always want to receive sorted output because I then can easily cross-check by eye-balling and compare the list of search results with the output of `ls` or [`tree`](http://mama.indstate.edu/users/ice/tree/), which are also sorted.

---

_Comment by @BurntSushi on 2016-12-01 12:03_

This is related to #152 but it subtly different. #152 is asking for *deterministic* output and you're asking for *sorted* output. What do you want to sort by? Should it be customizable? What do you hope for this to achieve that, say, `rg foo | sort` does not? Is it acceptable to lose parallelism? (I hope so, because doing this while retaining parallelism seems hard.)

---

_Label `question` added by @BurntSushi on 2016-12-01 12:04_

---

_Comment by @nerdrew on 2016-12-02 00:05_

@BurntSushi Hmmm. My bad. I don't care about sorting as much as I care about grouping by directory. So #152 seems more like what I'm looking for.

E.g.

```
dir1/file1.rs:blah
dir2/file4.rs:blah
dir1/file2.rs:blah
```

vs

```
dir1/file1.rs:blah
dir1/file2.rs:blah
dir2/file4.rs:blah
```

Totally understand the performance penalty for grouping results. Is it feasible to use the parallel runners to do the searching and aggregate the results afterward?

---

_Comment by @BurntSushi on 2016-12-02 00:14_

@nerdrew Well, I mean, yes, that's what a hypothetical solution would have to do. But now you've introduced a cost: extra memory use. There might be extra time cost too, for having to do the aggregate, but it could be immeasurable.

Of course, that might have been feasible in 0.2.x, but 0.3.x introduced a parallel directory iterator so that actually crawling through the directories themselves is parallelized. Making that do aggregation (and importantly, knowing when an aggregation is complete) seems hard.

I would say that there's basically two options here:

1. Deal with `-j1` if you want determinism.
2. Implement a new parallel searcher that's similar to the one in 0.2.x, but add aggregation. (This wouldn't be *that* hard.)

---

_Comment by @daxim on 2016-12-02 03:06_

> What do you want to sort by?

By the relative path name of the matching files; treat it as a string. Directory depth does not matter.

> Should it be customizable?

No, it should simply follow LC_COLLATE.

> What do you hope for this to achieve that, say, rg foo | sort does not?

The output of normal `ack --sort-files` on an interactive tty is human-readable with its path name headings above the matching lines, each group visually separated from each other, line numbers and colours. But when `rg` is piped into something, it's not human-readable any more. The path names are smushed together with the matching lines, there are no double line feeds to create paragraphs, line numbers are lost and results are not highlighted any more.

> Is it acceptable to lose parallelism?

Yes.


---

_Comment by @BurntSushi on 2016-12-02 11:45_

If `--sort-files` can imply `-j1`, then I think this is a relatively straight-forward thing to implement. It involves calling [`WalkDir::sort_by`](https://docs.rs/walkdir/1.0.2/walkdir/struct.WalkDir.html#method.sort_by) from the the single threaded [`ignore::Walk`](https://docs.rs/ignore/0.1.5/ignore/struct.Walk.html) iterator, which in turn requires exposing a [`sort_by` method on `ignore::WalkBuilder](https://docs.rs/ignore/0.1.5/ignore/struct.WalkBuilder.html).

---

_Closed by @BurntSushi on 2017-01-07 03:46_

---
