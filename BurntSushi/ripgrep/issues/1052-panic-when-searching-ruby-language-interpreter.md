```yaml
number: 1052
title: Panic when searching Ruby language interpreter source code
type: issue
state: closed
author: jackc
labels:
  - bug
assignees: []
created_at: 2018-09-13T17:08:52Z
updated_at: 2018-09-25T20:56:05Z
url: https://github.com/BurntSushi/ripgrep/issues/1052
synced_at: 2026-01-12T16:13:22Z
```

# Panic when searching Ruby language interpreter source code

---

_@jackc_

#### What version of ripgrep are you using?

```
jack@happy:~/Downloads/ruby-2.5.1$ rg --version
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`cargo install ripgrep`

#### What operating system are you using ripgrep on?

Ubuntu 18.04.1

```
jack@happy:~/Downloads/ruby-2.5.1$ uname -a
Linux happy 4.15.0-33-generic #36-Ubuntu SMP Wed Aug 15 16:00:05 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
```

#### If this is a bug, what are the steps to reproduce the behavior?

Download Ruby source code and search in it's directory.

```
jack@happy:/tmp$ curl --silent https://cache.ruby-lang.org/pub/ruby/2.5/ruby-2.5.1.tar.gz > ruby-2.5.1.tar.gz
jack@happy:/tmp$ tar xf ruby-2.5.1.tar.gz 
jack@happy:/tmp$ cd ruby-2.5.1/
jack@happy:/tmp/ruby-2.5.1$ rg foobarbazquz
thread '<unnamed>' panicked at 'index out of bounds: the len is 1 but the index is 1', /home/jack/.cargo/registry/src/github.com-1ecc6299db9ec823/encoding_rs-0.8.6/src/handles.rs:309:21
note: Run with `RUST_BACKTRACE=1` for a backtrace.
^C
```

Process then is hung.

#### If this is a bug, what is the actual behavior?

Command: `rg --debug foobarbazquz`

Output: https://gist.github.com/jackc/06e3cd8ce8ae238e6762249564cc1a76

#### If this is a bug, what is the expected behavior?

Not panic.


---

_Comment by @BurntSushi on 2018-09-13 17:30_

Interesting! It looks like the panic is coming from inside `encoding_rs`, but it could still be ripgrep's fault. I'll need to dig into this and come up with a smaller reproduction. Thanks for reporting this!

---

_Label `bug` added by @BurntSushi on 2018-09-13 17:30_

---

_Comment by @BurntSushi on 2018-09-25 01:05_

A smaller reproduction of this bug:

```
$ rg ZZZZZ ruby-2.5.1/test/rexml/data/t63-2.svg
```

It turns out that this svg file starts with a UTF-16LE BOM, but does not actually appear to be UTF-16. In any case, this trips over a corner case in the streaming transcoder that in turn causes a panic. The fault is either in my implementation of the streaming transcoder (by not upholding some precondition of the transcoder) or in the implementation of the encoding handling itself. Either way, I filed a bug to get to the bottom of it: https://github.com/hsivonen/encoding_rs/issues/34

For now, I [patched this via a work-around in the streaming transcoder](https://github.com/BurntSushi/encoding_rs_io/commit/8405b0f9aeacd35dfdb3f02a522454eb25ee3773), although it's not clear that the root cause has been fixed. PR #1065 brings in the workaround to ripgrep.

Thanks so much for reporting this!

---

_Closed by @BurntSushi on 2018-09-25 20:56_

---
