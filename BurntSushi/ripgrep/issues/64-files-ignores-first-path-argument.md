```yaml
number: 64
title: "--files ignores first path argument"
type: issue
state: closed
author: nickstenning
labels: []
assignees: []
created_at: 2016-09-24T14:27:23Z
updated_at: 2016-09-25T13:20:54Z
url: https://github.com/BurntSushi/ripgrep/issues/64
synced_at: 2026-01-12T18:23:11Z
```

# --files ignores first path argument

---

_@nickstenning_

I'm not sure whether this is a misunderstanding, a documentation bug, or an actual bug, but the `--files` option appears to ignore the first path argument given to it:
### Steps to reproduce

Here's a shell session that shows the problem:

```
$ uname -mrsv
Darwin 15.6.0 Darwin Kernel Version 15.6.0: Mon Aug 29 20:21:34 PDT 2016; root:xnu-3248.60.11~1/RELEASE_X86_64 x86_64
$ rg --version
0.1.17
$ mkdir a b c
$ touch {a,b,c}/{x,y,z}
$ tree
.
├── a
│   ├── x
│   ├── y
│   └── z
├── b
│   ├── x
│   ├── y
│   └── z
└── c
    ├── x
    ├── y
    └── z

3 directories, 9 files
$ rg --files
./a/x
./a/y
./a/z
./b/x
./b/y
./b/z
./c/x
./c/y
./c/z
$ rg --files a
./a/x
./a/y
./a/z
./b/x
./b/y
./b/z
./c/x
./c/y
./c/z
$ rg --files a b
b/x
b/y
b/z
$ rg --files a b c
b/x
b/y
b/z
c/x
c/y
c/z
```

Is this because the first argument is the (ignored) pattern? If so, I think the docs need updating, as the man page says:

```
   rg [options] --files [<path> ...]
```


---

_Comment by @caipre on 2016-09-24 19:12_

Also very confused by this. Does `rg` only search the current working directory?


---

_Comment by @BurntSushi on 2016-09-24 19:27_

The title is likely accurate.


---

_Closed by @BurntSushi on 2016-09-25 00:22_

---

_Comment by @homeworkprod on 2016-09-25 13:08_

Thanks for reporting and fixing this, respectively!

Just stumbled upon this, trying to use ripgrep with the CtrlP plugin for Vim.


---

_Comment by @homeworkprod on 2016-09-25 13:20_

 I can confirm that the patch works for me. Thanks again!

In case somebody else is interested, this is now in my `~/.vimrc`:

``` vim
if executable('rg')
  let g:ctrlp_user_command = 'rg --files %s'
  let g:ctrlp_use_caching = 0
endif
```


---
