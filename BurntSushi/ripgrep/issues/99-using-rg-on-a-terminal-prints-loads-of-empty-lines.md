```yaml
number: 99
title: Using rg on a terminal prints loads of empty lines
type: issue
state: closed
author: dersebi
labels:
  - question
assignees: []
created_at: 2016-09-26T06:52:42Z
updated_at: 2016-09-27T06:33:02Z
url: https://github.com/BurntSushi/ripgrep/issues/99
synced_at: 2026-01-12T18:23:11Z
```

# Using rg on a terminal prints loads of empty lines

---

_@dersebi_

Using ripgrep on a Linux terminal prints loads of empty lines. I tried searching a big directory for a word without any additional options and there is pages of empty lines.
When redirecting the oputput to a file it does not include the empty lines.

I tested with a smaller directory and there are less empty lines.

As output redirection does not have a problem this might be related to special terminal characters?

My test system is an Ubuntu 14.04 LTS and I tested with uxterm and gnome-terminal.
I built ripgrep using cargo in release mode. My Rust version is 1.11.0.


---

_Comment by @BurntSushi on 2016-09-26 09:42_

Could you provide some input, preferably a single file, where this happens?
A screen shot or dump of the output would be helpful.

On Sep 26, 2016 2:52 AM, "dersebi" notifications@github.com wrote:

> Using ripgrep on a Linux terminal prints loads of empty lines. I tried
> searching a big directory for a word without any additional options and
> there is pages of empty lines.
> When redirecting the oputput to a file it does not include the empty lines.
> 
> I tested with a smaller directory and there are less empty lines.
> 
> As output redirection does not have a problem this might be related to
> special terminal characters?
> 
> My test system is an Ubuntu 14.04 LTS and I tested with uxterm and
> gnome-terminal.
> I built ripgrep using cargo in release mode. My Rust version is 1.11.0.
> 
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/99, or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34kiUf_g054LJJNefay4e5CC9P8SNks5qt2u7gaJpZM4KGMR_
> .


---

_Comment by @dersebi on 2016-09-26 12:54_

A simple test case would be the following:

```
me@ubuntu:~ $ mkdir test
me@ubuntu:~ $ cd test
me@ubuntu:~/test $ echo "Hello World">test1
me@ubuntu:~/test $ rg World
test1
1:Hello World
me@ubuntu:~/test $ touch test2
me@ubuntu:~/test $ rg World
test1
1:Hello World

me@ubuntu:~/test $
me@ubuntu:~/test $ touch test3
me@ubuntu:~/test $ rg World
test1
1:Hello World


me@ubuntu:~/test $
```

I haven't verified this, but is seems to add an empty line for each file without a match.


---

_Comment by @BurntSushi on 2016-09-26 13:07_

Weird, I can't reproduce:

```
[andrew@Serval 99] l
total 0
[andrew@Serval 99] echo "Hello World" > test1
[andrew@Serval 99] rg World
test1
1:Hello World
[andrew@Serval 99] touch test2
[andrew@Serval 99] rg World
test1
1:Hello World
[andrew@Serval 99] touch test3
[andrew@Serval 99] rg World
test1
1:Hello World
```

Could you try your test case again with `--color=never`? Also, what is the output of `echo $TERM`?


---

_Comment by @dersebi on 2016-09-26 13:51_

`--color=never` yields the same result with the empty lines, except that it is not colored (obviously)
`TERM` is set to `xterm`


---

_Comment by @BurntSushi on 2016-09-26 20:57_

Hmm, I'm not sure what's causing this and I can't reproduce it. :-/

Probably my next step is firing up an Ubuntu 14.04 VM, and it may take me a while to get to that.


---

_Label `question` added by @BurntSushi on 2016-09-26 20:57_

---

_Comment by @nextlogic-ono on 2016-09-26 23:12_

I can reproduce this on Ubuntu 16.04 64bit (single core) on bash with 0.2.0

> $ echo yes >a.txt
> $ echo no >b.txt
> $ touch c.txt
> $ rg yes
> a.txt
> 1:yes
> (blank line here)
> (blank line here)

But I can't reproduce this on CentOS 6.8 (4 cores).


---

_Comment by @BurntSushi on 2016-09-26 23:34_

@windware AH! Dammit. This is another single threaded bug. I really wish I could DRY that up. Thank you so much for looking into this because you just saved me a bunch of time!

(P.S. Try running `rg -j1 ...` on Centos and it should repro.)


---

_Comment by @nextlogic-ono on 2016-09-26 23:40_

-j1 did it on CentOS but it only produced a single blank line with my above example.
-j2 also made a single blank line but -j3 and -j4 removed it.


---

_Closed by @BurntSushi on 2016-09-26 23:57_

---

_Comment by @BurntSushi on 2016-09-26 23:57_

OK, this should be fixed. This was a regression I introduced last night. :-(


---

_Comment by @dersebi on 2016-09-27 06:33_

Fixed indeed. Much better, thanks! 


---
