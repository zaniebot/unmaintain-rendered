```yaml
number: 102
title: How to search for hyphen followed by a character
type: issue
state: closed
author: kaushalmodi
labels: []
assignees: []
created_at: 2016-09-26T14:36:07Z
updated_at: 2016-09-26T16:25:40Z
url: https://github.com/BurntSushi/ripgrep/issues/102
synced_at: 2026-01-12T18:23:11Z
```

# How to search for hyphen followed by a character

---

_@kaushalmodi_

I was trying to search for hyphen followed by a character. But that did not work, even when trying to escape the hyphen.

This example demonstrates that:

```
km²~/temp/:proj/SOMEPRJ> tree -a
.
└── foo

0 directories, 1 file
km²~/temp/:proj/SOMEPRJ> cat foo
-bar
km²~/temp/:proj/SOMEPRJ> \rg --no-ignore-vcs '-' 
foo
1:-bar
km²~/temp/:proj/SOMEPRJ> \rg --no-ignore-vcs '-b'  
Unknown flag: '-b'

Usage: rg [options] -e PATTERN ... [<path> ...]
       rg [options] <pattern> [<path> ...]
       rg [options] --files [<path> ...]
       rg [options] --type-list
       rg [options] --help
       rg [options] --version
km²~/temp/:proj/SOMEPRJ> \rg --no-ignore-vcs '\-b' 
Error parsing regex near '\-b' at character offset 1: Unrecognized escape sequence: '\-'.
km²~/temp/:proj/SOMEPRJ>
```


---

_Comment by @kaushalmodi on 2016-09-26 14:39_

Found a workaround soon after I posted that.

```
km²~/temp/:proj/SOMEPRJ> \rg --no-ignore-vcs -- '-b' 
foo
1:-bar
```

But this works with `ag`:

```
km²~/temp/:proj/SOMEPRJ> \ag '\-b' 
foo
1:-bar
```

Is that a bug? Or a limitation of the Rust regex engine? Feel free to close this issue if it's not possible to support the escaped hyphen syntax.


---

_Comment by @BurntSushi on 2016-09-26 16:24_

`\-` doesn't work because `-` doesn't need to be escaped. Rust's regex engine doesn't let you willy nilly escape any character.

There are two work arounds. Either use `rg -- -a` or `rg -e -a`.


---

_Closed by @BurntSushi on 2016-09-26 16:24_

---

_Comment by @kaushalmodi on 2016-09-26 16:25_

Got it, thanks. 


---
