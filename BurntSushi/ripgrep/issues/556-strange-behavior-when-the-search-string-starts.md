```yaml
number: 556
title: "Strange behavior when the search string starts with '-'"
type: issue
state: closed
author: mhristache
labels: []
assignees: []
created_at: 2017-07-15T16:11:57Z
updated_at: 2017-07-15T16:19:01Z
url: https://github.com/BurntSushi/ripgrep/issues/556
synced_at: 2026-01-12T16:13:22Z
```

# Strange behavior when the search string starts with '-'

---

_@mhristache_

I am seeing some strange behavior when trying to find a string which starts with a `-`. I found it looking for `--release` keyword trying to find the syntax for a cargo command

When the string starts with `--` I get an error immediately (no matter if I use `-F` or not):

```
$ rg -F "--release" lazy/
error: Found argument '--release' which wasn't expected, or isn't valid in this context
	Did you mean --replace?

USAGE:
    
    rg [OPTIONS] <pattern> [<path> ...]
    rg [OPTIONS] [-e PATTERN | -f FILE ]... [<path> ...]
    rg [OPTIONS] --files [<path> ...]
    rg [OPTIONS] --type-list

For more information try --help
```

When the string starts with a `-`, the search takes very long time and it does not find anything:

```
$ time rg -F "-release" lazy/

real	0m0.699s
user	0m1.510s
sys	0m1.811s
```
It works when I look for ` --release` (note the extra space at the beginning):

```
$ time rg -F " --release" lazy/
lazy/test/build
8:OPENSSL_STATIC="" OPENSSL_LIB_DIR=../openssl_latest/lib/ OPENSSL_INCLUDE_DIR=../openssl_latest/include/ cargo build --release

real	0m0.102s
user	0m0.013s
sys	0m0.063s
```
Is this a bug or am I using it wrongly?

Thanks

---

_Comment by @okdana on 2017-07-15 16:17_

This is standard behaviour with UNIX tools. You'll see the same thing with `grep` for example.

You can use the special `--` argument to tell it not to interpret anything that follows as a command-line option:

```
rg -F -- --release lazy/
```

---

_Comment by @BurntSushi on 2017-07-15 16:18_

Or also use the `-e` flag to specify your pattern (which is also standard grep behavior as well): `rg -F -e --release lazy/`.

---

_Closed by @BurntSushi on 2017-07-15 16:18_

---

_Comment by @mhristache on 2017-07-15 16:19_

Great! Thank you!

---
