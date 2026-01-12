```yaml
number: 908
title: echo shell variable, ripgrep quit in the middle
type: issue
state: closed
author: ncaq
labels: []
assignees: []
created_at: 2018-05-03T10:36:09Z
updated_at: 2018-05-03T12:18:52Z
url: https://github.com/BurntSushi/ripgrep/issues/908
synced_at: 2026-01-12T16:13:22Z
```

# echo shell variable, ripgrep quit in the middle

---

_@ncaq_

#### What version of ripgrep are you using?

* ripgrep 0.8.1 -SIMD -AVX
* ripgrep 0.8.1 (rev 6ffb4b7466) -SIMD -AVX

I checkd in both version.

#### How did you install ripgrep?

* emerge ripgrep
* cargo build --release

#### What operating system are you using ripgrep on?

Gentoo GNU/Linux

#### If this is a bug, what are the steps to reproduce the behavior?

~~~txt
2018-05-03T19:18:02 ncaq@strawberry/pts/6(0) ~/Downloads
% ps aux > ps-aux.txt
2018-05-03T19:18:07 ncaq@strawberry/pts/6(0) ~/Downloads
% rg root ps-aux.txt|wc -l
347
2018-05-03T19:18:12 ncaq@strawberry/pts/6(0) ~/Downloads
% ps_result=$(cat ps-aux.txt)
2018-05-03T19:18:20 ncaq@strawberry/pts/6(0) ~/Downloads
% echo $ps_result|rg root|wc -l
203
2018-05-03T19:18:52 ncaq@strawberry/pts/6(0) ~/Downloads
% echo $ps_result|ag root|wc -l
347
~~~

[ps-aux.txt](https://github.com/BurntSushi/ripgrep/files/1970488/ps-aux.txt)

#### If this is a bug, what is the actual behavior?

~~~txt
echo $ps_result|rg root|wc -l
~~~

~~~txt
203
~~~

#### If this is a bug, what is the expected behavior?

~~~txt
echo $ps_result|rg root|wc -l
~~~

~~~txt
347
~~~

---

_Comment by @BurntSushi on 2018-05-03 10:58_

This is a very strange bug report, and I don't really understand what it is you're doing. In any case, I can't reproduce this:

```
$ rg root ps-aux.txt | wc -l
347
$ ag root ps-aux.txt | wc -l
347
$ grep root ps-aux.txt | wc -l
347
```

```
$ ps_result=$(cat ps-aux.txt)
$ echo $ps_result | rg root | wc -l
1
$ echo $ps_result | ag root | wc -l
1
$ echo $ps_result | grep root | wc -l
1
```

Please provide more details on how to reproduce this.

---

_Comment by @ncaq on 2018-05-03 11:55_

Oh, I use zsh.

~~~txt
% zsh --version
zsh 5.5.1 (x86_64-pc-linux-gnu)
~~~

I use bash, `wc -l` result to be 1.

I will little research this.

---

_Comment by @ncaq on 2018-05-03 12:15_

change result case of shell echo or /bin/echo.

~~~
2018-05-03T21:08:26 ncaq@strawberry/pts/9(0) ~/Downloads
% echo $ps_result|rg root|wc -l
203
2018-05-03T21:08:56 ncaq@strawberry/pts/9(0) ~/Downloads
% /bin/echo $ps_result|rg root|wc -l
347
~~~

---

_Comment by @BurntSushi on 2018-05-03 12:18_

I am able to reproduce this in zsh, and this isn't a problem with ripgrep. Here's the giveaway:

```
[\u@\h \W] echo $ps_result | grep root
Binary file (standard input) matches
```

I narrowed it down to this by running `echo $ps_result | xxd` and looking for `NUL` bytes (which trip the binary file detection in both grep and ripgrep):

```
$ echo "/sbin/agetty -o -p -- \u --noclear tty1 linux" | grep linux
Binary file (standard input) matches
```

Changing `\u` to `\` causes this to work:

```
$ echo "/sbin/agetty -o -p -- u --noclear tty1 linux" | grep linux
/sbin/agetty -o -p -- u --noclear tty1 linux
```

I don't know anything about zsh, but from the looks of it, it's interpreting `\u` as an escape sequence of some sort and writing out a `NUL` byte.

According to `man zshbuiltins`, `\u` is used to insert a Unicode codepoint. `echo`'s `-E` flag can be used to disable escape sequences:

```
$ echo -E "/sbin/agetty -o -p -- \u --noclear tty1 linux" | grep linux
/sbin/agetty -o -p -- \u --noclear tty1 linux
```

You can try other things, e.g.,

```
$ echo "linux \u" | xxd
00000000: 6c69 6e75 7820 000a                      linux ..
```

This very clearly shows that `zsh` is replacing `\u` with `NUL`.

---

_Closed by @BurntSushi on 2018-05-03 12:18_

---

_Comment by @BurntSushi on 2018-05-03 12:18_

Note that in general, the silver searcher should not be used as an oracle. It has oodles of bugs. Use grep instead. In this case, ag doesn't do binary detection.

---
