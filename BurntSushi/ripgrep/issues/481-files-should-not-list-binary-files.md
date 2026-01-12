```yaml
number: 481
title: "`--files` should not list binary files"
type: issue
state: closed
author: skrattaren
labels: []
assignees: []
created_at: 2017-05-11T19:14:32Z
updated_at: 2017-05-11T22:04:20Z
url: https://github.com/BurntSushi/ripgrep/issues/481
synced_at: 2026-01-12T16:13:22Z
```

# `--files` should not list binary files

---

_@skrattaren_

[similar to #436]
Quoting manpage:
```
 --files
        Print each file that would be searched (but don't search).
```
But that is incorrect, `--files` lists all "unignored" files, including binary ones, which will not be searched unless `--text` added.  [Example](https://github.com/genestack/user-tutorials/tree/master/source/_static):

```
 % ls .
favicon.ico  genestack.css  logo.svg

 % rg --files                                           
favicon.ico                                              
genestack.css
logo.svg

 % rg -l 0  
logo.svg

 % rg -al 0
favicon.ico
logo.svg
```

I expect `rg --files` to omit binary files unless `--text` is added.

If there is any way of doing that (listing files without ignored and binary ones), I would gladly accept that too.

---

_Comment by @BurntSushi on 2017-05-11 19:27_

Can you propose an implementation that does what you want?

---

_Comment by @BurntSushi on 2017-05-11 19:28_

How do you determine whether a file is binary or not?

---

_Comment by @skrattaren on 2017-05-11 19:33_

> Can you propose an implementation that does what you want?

Sorry, I'm not quite follow you.  Does any similar tool does that?  `ag` does not, there's similar issue opened there as well (before me).

> How do you determine whether a file is binary or not?

Again, you lost me there.  How does `ripgrep` already do that?  In proposed example *.ico* file isn't searched by `ripgrep` unless I add `--text`.  I presume there is some check for that already, which isn't triggered in `--files` mode.  Am I missing something?

---

_Comment by @BurntSushi on 2017-05-11 19:34_

> Am I missing something?

Binary file detection happens during search.

---

_Comment by @skrattaren on 2017-05-11 19:46_

I thought so.
But I would still like a way of
> listing files without ignored and binary ones

(I'd like to use `ripgrep` in *vim+ctrlp*).

Do you consider this too complicated to be implemented?

---

_Comment by @BurntSushi on 2017-05-11 20:10_

> Do you consider this too complicated to be implemented?

It's not about complexity. The whole point of the `--files` flag is to list files that *would* be searched. Filtering out binary files from that list requires them to be searched, which is exactly what `--files` says *won't* happen. More practically, the reason why `--files` exists is because it's much faster than searching.

If you really want this, then all you need to do is run a search. i.e., Instead of `rg --files`, use `rg -l '.?'`, which will print every file that matched the given regex (which always matches), but also omits binary files.

---

_Comment by @skrattaren on 2017-05-11 22:04_

I'll take it, thanks =)

---

_Closed by @skrattaren on 2017-05-11 22:04_

---
