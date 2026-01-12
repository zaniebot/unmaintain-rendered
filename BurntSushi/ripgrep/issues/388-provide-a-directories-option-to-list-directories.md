```yaml
number: 388
title: "Provide a `--directories` option to list directories"
type: issue
state: closed
author: mikepqr
labels: []
assignees: []
created_at: 2017-03-01T06:06:19Z
updated_at: 2017-10-21T22:20:19Z
url: https://github.com/BurntSushi/ripgrep/issues/388
synced_at: 2026-01-12T16:13:21Z
```

# Provide a `--directories` option to list directories

---

_@mikepqr_

I'm [using ripgrep with `fzf`](http://blog.owen.cymru/fzf-ripgrep-navigate-with-bash-faster-than-ever-before/) to fly around my filesystem much faster than I could by using `find` with `fzf`.

This approach uses `rg --files`, which is not only a nice terse, sensible alternative to a long, pedantic `find` command. It's also orders of magnitude faster!

It would be great if I could do `rg --directories` and have it work just like `rg --files`, but give me all the directories, rather than all the files.

`find` obviously supports a huge variety of options that are out of scope for a tool focussed on searching within files. This particular use case seems (to me at least! ymmv!) common enough that it  might be worth supporting with ripgrep.

---

_Comment by @bag-man on 2017-03-01 08:23_

+1!

---

_Comment by @BurntSushi on 2017-03-01 12:08_

This is a dupe of #169, which I closed on the grounds that it is out of scope. Please read that issue though, because it includes ideas for workarounds. Do they work for you?

My strong preference here is for someone to write a Rust tool that does this for you. It would be relatively simple to do today, since all of the directory traversal and `gitignore` code has been factored out into separate libraries. The `--files` listing is a *secondary* feature of ripgrep. It is intended to tell you what files ripgrep will search.

---

_Comment by @BurntSushi on 2017-03-01 12:09_

@bag-man Please avoid `+1` comments.

---

_Comment by @mikepqr on 2017-03-01 13:30_

Thanks! Sorry I didn't search old isuses!

Piping `rg --hidden --files --null | xargs -0 dirname | uniq` to `fzf` works perfectly for me. Thank you.

(`rg --hidden --files --null | xargs -0 dirname | sort -u` works less well because nothing comes out of the pipeline until rg is finished, so I can't start searching in `fzf` immediately)

---

_Comment by @BurntSushi on 2017-03-01 13:41_

@williamsmj Since #169, ripgrep has grown a `--sort-files` flag which will do what you want. It will force ripgrep to use a single thread, but it will emit results incrementally instead of blocking until the entire traversal is complete.

---

_Closed by @BurntSushi on 2017-03-01 13:41_

---

_Comment by @mikepqr on 2017-03-01 13:47_

So `rg --hidden --sort-files --files --null 2> /dev/null | xargs -0 dirname | uniq`?

Or can I drop some part of the pipeline?

Note the important thing about piping through `sort` for this use case is the `-u` not the sort.

---

_Comment by @BurntSushi on 2017-03-01 13:50_

@williamsmj Ah I see. If you don't care about the sort, then it'd be fine to drop `--sort-files` so long as `uniq` does what you want. Otherwise, it looks good to me!

---

_Comment by @ddickstein on 2017-05-21 02:32_

I still got duplicates when I tried this command.  Removing `--sort-files` and replacing `uniq` with `sort -u` seemed to work more reliably.  Perhaps there's a bug in the sort?

---

_Comment by @BurntSushi on 2017-05-21 04:07_

@ddickstein can you open a new issue with a reproducible example? ripgrep should never be showing the same file path twice in the output of --files.

---

_Comment by @ddickstein on 2017-05-21 11:17_

Well the duplicates would appear with the `dirname` call, right?  The idea here is that we have a list of sorted files, then we call `dirname` on all of them (introducing duplicates), but then we call `uniq` which resolves duplicate directory names.  Since there were duplicates in output and I'm assuming there isn't a bug in `uniq` though it's limited by the need for its input to be sorted, I would guess that the problem is because of a sorting failure with `--sort-files` rather than a deduping failure with `--files`.  The problem disappeared when I instead did sort+unique at the end by omitting `--sort-files` and replacing `uniq` with `sort -u`.  I can try to reproduce at some point and file an issue, but with that change my setup is working the way I'd like.

---

_Comment by @BurntSushi on 2017-05-21 14:57_

@ddickstein Oh I see. `--sort-files` is correct as far as I can see. `--sort-files` sorts the complete file paths, which happens before `dirname` is applied. For example, this is valid sorted output of `rg --files --sorted-files`:

```
a/b.txt
a/c/foo.txt
a/d.txt
```

But if you apply `dirname` to each, then you get

```
a
a/c
a
```

So basically, you need to sort/dedup after applying `dirname`.

---

_Comment by @ddickstein on 2017-05-21 21:09_

Oh interesting.  So if it sorted by file type (file v directory) and then alphabetically instead of just alphabetizing file paths then it would have worked?  I wonder what the other use cases would be for one kind of sort vs the other.

---

_Comment by @matematikaadit on 2017-10-21 22:20_

Relevant project, https://github.com/sharkdp/fd. A simple, fast and user-friendly alternative to find.
It also uses `regex` and `ignore` crates.

---
