```yaml
number: 887
title: Cannot grep special devices such as /dev/urandom
type: issue
state: closed
author: kenorb
labels:
  - bug
  - wontfix
assignees: []
created_at: 2018-04-15T01:15:34Z
updated_at: 2018-04-15T12:44:19Z
url: https://github.com/BurntSushi/ripgrep/issues/887
synced_at: 2026-01-12T16:13:22Z
```

# Cannot grep special devices such as /dev/urandom

---

_@kenorb_

#### What version of ripgrep are you using?

`ripgrep 0.8.1`

#### What operating system are you using ripgrep on?

macOS High Sierra

#### Bug

This works:

```
$ rg . </etc/hosts
##
# Host Database
```

----

This doesn't:

`$ rg . < /dev/urandom`

or more practical example:

`$ rg -uuu -o abc < /dev/urandom`

It seems it fallback into searching the current folder.

However this syntax work:

```
$ cat /dev/urandom | rg -uuu -o abc
```

----

I believe it should work similar as other tools, e.g.

```
$ strings </dev/urandom | head
Q)CD
h`m3
+xRF:?
```

```
$ grep . </dev/urandom # GNU grep
Binary file (standard input) matches
```

---

_Label `bug` added by @BurntSushi on 2018-04-15 12:40_

---

_Label `wontfix` added by @BurntSushi on 2018-04-15 12:40_

---

_Comment by @BurntSushi on 2018-04-15 12:44_

This is a bug, but I do not believe it can be fixed in a way that doesn't produce more serious bugs. The simplest work-around is to create a pipe: `cat /dev/urandom | rg .`.

See also:

* https://github.com/BurntSushi/ripgrep/commit/ab0d1c1c797c0464ad4fcdfc641bcf88f9ade304
* #35 
* #81 

---

_Closed by @BurntSushi on 2018-04-15 12:44_

---
