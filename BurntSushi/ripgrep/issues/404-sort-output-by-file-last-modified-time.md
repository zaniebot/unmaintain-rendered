```yaml
number: 404
title: sort output by file last modified time
type: issue
state: closed
author: wavexx
labels:
  - enhancement
  - icebox
assignees: []
created_at: 2017-03-12T12:58:47Z
updated_at: 2018-08-26T22:42:26Z
url: https://github.com/BurntSushi/ripgrep/issues/404
synced_at: 2026-01-12T16:13:21Z
```

# sort output by file last modified time

---

_@wavexx_

I'm throwing this here for consideration. When I'm searching through my source files, I often not only want to find all occurrences, but when multiple hits are involved, I also want to find the most recent modification (by mtime). It would be nice to be able to sort output files (*with context*), sorted by time and reverse time.

Afaik, none of the various searching tools allow to do this. Not easily at least.

To do it currently, I actually do two passes:

- find file candidates fist
- sort files by mtime
- search within files

This is necessary as I obviously like to see the match context as well (otherwise I could stop at step 2). But having to find candidates first introduces extra overhead that cannot be avoided.

This requires buffering matches before showing the output, of course, but the search can be ran in parallel still.

---

_Label `enhancement` added by @BurntSushi on 2017-03-12 13:27_

---

_Comment by @BurntSushi on 2017-03-12 13:29_

@wavexx This does seem mildly useful, but I don't know when I'll have the time to work on it.

> This requires buffering matches before showing the output, of course, but the search can be ran in parallel still.

While strictly true, this is an oversimplification. Namely, running a search in parallel requires buffering to avoid interleaving results. Sorting with parallelism requires something more than just a simple buffering scheme however, and it is rather difficult to do in the current setup (which parallelizes things at level of directory traversal). This is why the `--sort-files` flag forces ripgrep to operate in single threaded mode.

---

_Label `icebox` added by @BurntSushi on 2017-04-09 13:10_

---

_Comment by @BurntSushi on 2018-01-31 02:59_

It should be possible to implement this in the `ignore` crate without too much trouble now that [`walkdir`'s `sort_by` method](https://docs.rs/walkdir/2.1.0/walkdir/struct.WalkDir.html#method.sort_by) exposes the necessary information. Namely, you can get the `Path` from the `DirEntry` and stat it to determine various attributes. This will have performance implications and should force single threaded mode. We'll also need to think carefully about flags.

---

_Renamed from "Optionally sort output files" to "sort output by file last modified time" by @BurntSushi on 2018-04-25 20:59_

---

_Comment by @BurntSushi on 2018-08-24 14:14_

Here is what I propose:

* Add new `--sort` and `--sortr` flags to ripgrep. Both accept the same set of pre-determined choices. The former sorts in ascending order where as the latter sorts in descending order (or "reverse" order).
* Implementation detail: Whenever `--sort` or `--sortr` are given, ripgrep enters single threaded mode.
* Initially, the set of available parameters to the flag will be `path`, `modified`, `accessed`, `created`. `path` will sort by the file path (and therefore, `--sort path` will behave exactly the same as `--sort-files` does today). `modified`/`accessed`/`created` will sort by a file's last-modified/last-accessed/created timestamps, when available. When not available, ripgrep will stop execution before searching and print an error if possible. Otherwise, sort order will be unspecified.

We'll likely therefore deprecate `--sort-files`, but leave it as a hidden alias for `--sort path`.

Thoughts?

---

_Comment by @wavexx on 2018-08-24 14:19_

I like it.

---

_Closed by @BurntSushi on 2018-08-26 22:42_

---
