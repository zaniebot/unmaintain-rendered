```yaml
number: 1901
title: Option to show matches in filenames
type: issue
state: closed
author: Nudin
labels:
  - duplicate
assignees: []
created_at: 2021-06-17T12:32:06Z
updated_at: 2021-06-17T12:36:32Z
url: https://github.com/BurntSushi/ripgrep/issues/1901
synced_at: 2026-01-12T16:13:24Z
```

# Option to show matches in filenames

---

_@Nudin_

I use ripgrep to browse source code, especially code that I'm not familiar with yet. A pattern I do very regullary when I want to know where some symbol (variable, function, class, script, whatever) is used is:
```
$ fd symbolname
$ rg symbolname
```
It would be nice to have this merged into one command as well as recive a single consistent output. I would therefore love to se an rg option `--search-filename` that will not only search the filecontents for matches of the supplied regexp but also the filenames. If a filename matches the regex but no line in it, the file would still be shown. If the filename matches and the file contains matching lines, filename and lines would be displayed as usual, except that the match in the filename would also be highlighted (if colors are enabled).

Simple example of how this could look like:
```
$ rg --search-filename symbolname
some-file.sh
5: Do something with symbolname
23: # call symbolname.sh
24: ./symbolname.sh

symbolname.sh

other-file-with-symbolname.sh
17: this is another file with symbolname
54: more occurences of symbolname
```

---

_Comment by @BurntSushi on 2021-06-17 12:36_

Dupe of #1034 

---

_Closed by @BurntSushi on 2021-06-17 12:36_

---

_Label `duplicate` added by @BurntSushi on 2021-06-17 12:36_

---
