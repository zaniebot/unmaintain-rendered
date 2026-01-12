```yaml
number: 98
title: Searching stops when an unreadable file is encountered
type: issue
state: closed
author: nextlogic-ono
labels: []
assignees: []
created_at: 2016-09-26T04:00:58Z
updated_at: 2016-09-26T22:44:25Z
url: https://github.com/BurntSushi/ripgrep/issues/98
synced_at: 2026-01-12T18:23:11Z
```

# Searching stops when an unreadable file is encountered

---

_@nextlogic-ono_

Say, a directory contains

> a.txt
>   b.txt

and if a.txt is not readable by the executing user, even if the pattern matches in the b.txt, rg quits saying,

> Permission denied (os error 13)

I think it should continue searching when encountering these recoverable errors.


---

_Comment by @BurntSushi on 2016-09-26 11:39_

Really? I can't reproduce it. I tried to be pretty careful about not falling for this trap. Here's a test case that works on my end. Could you try to produce something similar so that others can reproduce the bug you're seeing? Thanks!

```
[andrew@Serval 98] echo foo > a.txt
[andrew@Serval 98] echo foo > b.txt
[andrew@Serval 98] rg foo
b.txt
1:foo

a.txt
1:foo
[andrew@Serval 98] chmod 222 a.txt
[andrew@Serval 98] cat a.txt
cat: a.txt: Permission denied
[andrew@Serval 98] rg foo
./a.txt: Permission denied (os error 13)
b.txt
1:foo
```


---

_Comment by @nextlogic-ono on 2016-09-26 22:21_

I tested in Ubuntu 16.04, CentOS 6.8 and OS X 10.11 and could reproduce except on OS X.
They're all run with 64bit 0.2.0 of rg in an empty 755 folder on bash.
The output shown is from Ubuntu 16.04.

> $ echo foo >a.txt
> $ echo foo >b.txt
> $ chmod 000 a.txt
> $ rg foo
> Permission denied (os error 13)

This one actually works in CentOS but failed in Ubuntu.
chmod 644 a.txt will obviously make it work again.
I came across this when searching through a larger set of files but the result got truncated.

Results change if I add c.txt and it will work as expected.
(continued from above)

> $ echo foo >c.txt
> $ chmod 644 a.txt
> $ chmod 000 b.txt
> $ rg foo
> a.txt
> 1:foo
> c.txt
> 1:foo
> Permission denied (os error 13)

but then again, it fails if I explicitly specify the files.

> $ rg foo *.txt
> a.txt
> 1:foo
> Permission denied (os error 13)

Also, this last command gives different result on CentOS.

> $ rg foo *.txt
> b.txt: Permission denied (os error 13)


---

_Comment by @BurntSushi on 2016-09-26 22:32_

Do you happen to have any aliases set? Or are these all machines with a single core? I think I might have found the bug.


---

_Comment by @BurntSushi on 2016-09-26 22:33_

Yup, got it, there's a bug when `rg` runs with a single thread.


---

_Comment by @nextlogic-ono on 2016-09-26 22:34_

No I have no alias set. I have them in /usr/local/bin but in fact, I've downloaded the rg binary onto the same folder just in case and run it with,
./rg
and it did all the same.

Ubuntu is on a single core and CentOS is on 4 cores.


---

_Closed by @BurntSushi on 2016-09-26 22:44_

---
