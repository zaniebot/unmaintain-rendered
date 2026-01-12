```yaml
number: 65
title: "--files doesn't correctly handle .gitignore'd directories when supplied paths are absolute"
type: issue
state: closed
author: nickstenning
labels: []
assignees: []
created_at: 2016-09-24T14:45:38Z
updated_at: 2016-09-24T20:19:35Z
url: https://github.com/BurntSushi/ripgrep/issues/65
synced_at: 2026-01-12T18:23:11Z
```

# --files doesn't correctly handle .gitignore'd directories when supplied paths are absolute

---

_@nickstenning_

It seems that specifying a path in a `.gitignore` which has a trailing slash, such as

```
a/
```

(as opposed to simply `a`) causes `rg --files` to fail to correctly ignore files within that directory. This only happens when an absolute path is supplied as an argument.
### Steps to reproduce

Here's a shell session that shows the problem. In it I create a directory, ignore it, and then show that the files within that directory are correctly ignored if I run `rg --files` or `rg --files ignored .` (see https://github.com/BurntSushi/ripgrep/issues/64 for why I've included the word `ignored` in that invocation), but not if I run `rg --files ignored $PWD`.

If I either specify the path as `.`, or remove the trailing slash from the line in `.gitignore`, the directory is correctly ignored.

```
$ uname -mrsv
Darwin 15.6.0 Darwin Kernel Version 15.6.0: Mon Aug 29 20:21:34 PDT 2016; root:xnu-3248.60.11~1/RELEASE_X86_64 x86_64
$ rg --version
0.1.17
$ mkdir a
$ touch x y a/x a/y
$ echo a/ > .gitignore
$ ag . -l -g ''
x
y
$ ag $PWD -l -g ''
/Users/redacted/mess/current/example/x
/Users/redacted/mess/current/example/y
$ rg --files ignored .
./x
./y
$ rg --files ignored $PWD
/Users/redacted/mess/current/example/a/x
/Users/redacted/mess/current/example/a/y
/Users/redacted/mess/current/example/x
/Users/redacted/mess/current/example/y
$ echo a > .gitignore
$ rg --files ignored .
./x
./y
$ rg --files ignored $PWD
/Users/redacted/mess/current/example/x
/Users/redacted/mess/current/example/y
```


---

_Comment by @BurntSushi on 2016-09-24 20:19_

Nice find. This is a dupe of #16.


---

_Comment by @BurntSushi on 2016-09-24 20:19_

(Fix incoming.)


---

_Closed by @BurntSushi on 2016-09-24 20:19_

---
