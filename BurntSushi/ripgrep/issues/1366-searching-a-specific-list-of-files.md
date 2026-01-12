```yaml
number: 1366
title: Searching a specific list of files
type: issue
state: closed
author: rkhoury
labels:
  - wontfix
assignees: []
created_at: 2019-09-06T12:09:57Z
updated_at: 2019-10-29T11:36:44Z
url: https://github.com/BurntSushi/ripgrep/issues/1366
synced_at: 2026-01-12T16:13:23Z
```

# Searching a specific list of files

---

_@rkhoury_

#### Feature Request

Based on https://unix.stackexchange.com/questions/494595/how-can-i-search-a-specific-list-of-files-with-ripgrep

It would be great if ripgrep could natively support iterating a specific list of file paths to search within from a parameter supplied to the program.  I would have naturally have gone with --files had that not already been used, but perhaps --searchfiles <FILE>?  And perhaps any other filter supplied through --type/--type-not would just apply to this file list?

I'm assuming that it'd be more efficient for a single instance of ripgrep to be able to handle iterating a specific list of files than to use something like xargs. The bonus of this is also being able to more easily do this kind of iterating on platforms where gnu tools aren't available.

---

_Label `wontfix` added by @BurntSushi on 2019-09-06 12:11_

---

_Comment by @BurntSushi on 2019-09-06 12:16_

I'm going to have to decline here, sorry. I think the fact that ripgrep already accepts a list of files as arguments is good enough. And in particular, applying filter rules to a list of files given via a hypothetical `--searchfiles` is the opposite behavior from giving files on the command line. Invariably, both choices are likely reasonable, which means this is a feature that is likely a launching point for even more feature requests. While that sounds like a lame cop-out, this is just something that I need to take into account when evaluating new features.

I acknowledge the fact that `xargs` being unavailable makes this more difficult for folks. But I think this is probably something to be solved in ecosystems that lack `xargs` (or an equivalent).

Note also that `xargs` isn't strictly required. "Modern" Unix shells support more convenient syntax if you just have a file listing files:

```
$ ls
bar  foo  list  quux

$ cat list
foo
bar
quux

$ rg test $(cat list)
foo
1:test

bar
1:test

quux
1:test
```

---

_Closed by @BurntSushi on 2019-09-06 12:16_

---

_Comment by @rkhoury on 2019-09-06 12:23_

Thanks for the explanation.  It's a bit of a bummer.  The scenario I intended on using it was from within a Windows-based developer tool that will be searching many (10,000+) files and I know I'll be hitting command line length limits from within windows.
Anyway, I'll just have to batch up the search requests outside of the command instantiation.

---

_Comment by @lymslive on 2019-10-29 07:16_

I also want this feature, but found there is already an issuer, but closed?
In a large complex project, I think it convenient to keep a list of "useful" file list.
For example, I usually first create such a file list with command some like `rg --type cpp --files > GFILES` . And later I want make use of this file list to search something, that I believe it's more efficient and flexible if a parameter(say --searchfiles) can accept this file list than each time have to use `--type` or `--glob` alike.

Many thanks if you can consider this feature more closely.

---

_Comment by @BurntSushi on 2019-10-29 11:36_

If people want this feature, then there way to convince me is to address my actual concerns in my comment above.

> And later I want make use of this file list to search something, that I believe it's more efficient and flexible if a parameter(say --searchfiles) can accept this file list than each time have to use --type or --glob alike.

Why? I don't understand why `rg -tcpp` is unacceptable to you. I'm also skeptical of your claim about efficiency. I'm not going to add a feature I disagree with based on hand wavy claims. Please stick to concrete things with examples.

---
