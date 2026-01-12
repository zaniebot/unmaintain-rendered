```yaml
number: 501
title: path separator on windows is not always \
type: issue
state: closed
author: albfan
labels:
  - question
assignees: []
created_at: 2017-06-02T09:15:52Z
updated_at: 2017-11-15T08:48:50Z
url: https://github.com/BurntSushi/ripgrep/issues/501
synced_at: 2026-01-12T16:13:22Z
```

# path separator on windows is not always \

---

_@albfan_

running under mingw output shows paths with \ but mingw (running bash) uses /

Were is the code related with path separator?, maybe mingw can be detected to fix that

---

_Comment by @BurntSushi on 2017-06-02 11:35_

There is no automatic detection, but you can use the `--path-separator` flag to override the path separator in any environment. I imagine sticking it in an alias might work for you?

---

_Label `question` added by @BurntSushi on 2017-06-02 11:35_

---

_Comment by @albfan on 2017-06-02 12:08_

On cmd it works, but on mingw bash (the one that git for windows provides if you're curious) it outputs:

    $ rg foo --path-separator '/'
    A path separator must be exactly one byte, but the given separator is 20 bytes.



---

_Comment by @albfan on 2017-06-02 12:08_

same without quotes

---

_Comment by @BurntSushi on 2017-06-02 12:09_

That error message is baffling to me. Someone will need to debug it. I'll do it eventually.

---

_Comment by @albfan on 2017-06-02 12:18_

Ok, count on me for that

Seems there's window detecting no?
https://github.com/BurntSushi/ripgrep/blob/13235b596f93f7f0e7ce76e7967996787147e96e/src/printer.rs#L412

and docs say it too:
https://github.com/BurntSushi/ripgrep/blob/master/doc/rg.1#L390

Have to check what is behind cfg!(windows)

maybe that can be a better fix. By now options would work

If you want to go for OS detection or want to detect that problem with bytes just open a bug and comment here




---

_Closed by @albfan on 2017-06-02 12:18_

---

_Comment by @BurntSushi on 2017-06-02 12:21_

@albfan Why did you close this? If `--path-separator` isn't working, then that's a bug.

---

_Comment by @albfan on 2017-06-02 13:33_

My problem with choose a separator is resolved:

    $ rg foo --path-separator '\x2F'

There's another problem that maybe you want to solve. But feels that is a different issue

Do you want me to reopen this and work that here?

---

_Reopened by @BurntSushi on 2017-06-02 13:34_

---

_Comment by @BurntSushi on 2017-06-02 13:35_

@albfan I understand that you found a work-around, but if `rg foo --path-separator /` is giving your error messages about the path separator being 20 bytes, then that's a bug.

If there's a distinct issue, then I'd say open a new ticket...

---

_Comment by @albfan on 2017-06-02 13:46_

Here some test:

```
$ rg foo --path-separator /
A path separator must be exactly one byte, but the given separator is 20 bytes.

#this works
$ rg foo --path-separator //

$ rg foo --path-separator aa
A path separator must be exactly one byte, but the given separator is 2 bytes.

$ rg foo --path-separator aaa
A path separator must be exactly one byte, but the given separator is 3 bytes.

$ rg foo --path-separator aaa/
A path separator must be exactly one byte, but the given separator is 4 bytes.

#double do not increase faulty bytes
$ rg foo --path-separator aaa//
A path separator must be exactly one byte, but the given separator is 5 bytes.

$ rg foo --path-separator /a
A path separator must be exactly one byte, but the given separator is 3 bytes.

#what???
$ rg foo--path-separator /aa
A path separator must be exactly one byte, but the given separator is 23 bytes.

$ rg foo--path-separator /aaa
A path separator must be exactly one byte, but the given separator is 24 bytes.
```

Have to learn how to debug rust, any hint on windows?



---

_Comment by @BurntSushi on 2017-06-02 14:06_

@albfan Ah, I wonder if the problem here is your shell. Maybe the shell is expanding `/` to something like `/c/cygdrive/` or something similar?

---

_Comment by @albfan on 2017-06-02 15:02_

I think so

---

_Comment by @albfan on 2017-06-03 08:24_

http://www.mingw.org/wiki/Posix_path_conversion

Here we are. / gets translated into a absolute path on mingw. Double // prevents conversion.

That's why 20 bytes

This is solved then

---

_Closed by @albfan on 2017-06-03 08:24_

---

_Comment by @albfan on 2017-11-15 08:48_

> Definitive solution

```
$ cat ~/.bashrc
...
alias rg='rg --path-separator="//"'
...
```

---
