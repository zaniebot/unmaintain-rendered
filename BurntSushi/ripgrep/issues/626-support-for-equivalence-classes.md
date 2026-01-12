```yaml
number: 626
title: Support for equivalence classes
type: issue
state: closed
author: albfan
labels:
  - enhancement
  - icebox
assignees: []
created_at: 2017-10-07T09:53:08Z
updated_at: 2017-11-05T23:46:44Z
url: https://github.com/BurntSushi/ripgrep/issues/626
synced_at: 2026-01-12T16:13:22Z
```

# Support for equivalence classes

---

_@albfan_

Is there any chance to support equivalence classes?

https://www.regular-expressions.info/posixbrackets.html#eq

here is a scce test case:
```
$ cat >test <<EOF
Búsqueda global
EOF
$ grep -RiH "B[[=u=]]squeda global" *
test:Búsqueda global
$ ag -i "b[[=u=]]squeda global"
ERR: Bad regex! pcre_compile() failed at position 2: POSIX collating elements are not supported
If you meant to search for a literal string, run ag with -Q
```

---

_Label `enhancement` added by @BurntSushi on 2017-10-07 11:52_

---

_Label `icebox` added by @BurntSushi on 2017-10-07 11:52_

---

_Comment by @BurntSushi on 2017-10-07 11:56_

I'm not in principle against it, but this enhancement requires enhancements to the regex engine itself. If this ever happens, it will not be for a very long time.

Anywho, this issue isn't really for ripgrep but for the regex engine, so I've filed the issue there instead: https://github.com/rust-lang/regex/issues/404

---

_Closed by @BurntSushi on 2017-10-07 11:56_

---

_Comment by @albfan on 2017-10-07 20:18_

Fair enough. Thanks for take time to do fill that. Let's see if I can provide a patch on that

---

_Comment by @BurntSushi on 2017-10-08 00:07_

Please sketch out an implementation strategy before doing the work. It is a
*significant* task.

On Oct 7, 2017 4:18 PM, "Alberto Fanjul" <notifications@github.com> wrote:

> Fair enough. Thanks for take time to do fill that. Let's see if I can
> provide a patch on that
>
> —
> You are receiving this because you modified the open/close state.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/626#issuecomment-334963138>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34k0G2-lOHplWu_063isreiwCqWJHks5sp9ytgaJpZM4PxTAP>
> .
>


---

_Comment by @albfan on 2017-11-05 23:39_

As a resume for anyone looking at this

rust tries to be agnostic about charsets, so this concept of equivalence classes is not avaliable. The only thing remotely similar is the normalization form for chars, so `ò` can be decomposed on `o` and ``` so searching for `o` equivalences could detect `ò`as an equivalence. That implies a file preprocess. That can be done with https://crates.io/crates/unicode-normalization, but presumably it will slow down all searches

provide custom config for this

    [equivalence]
    a = aá
    e = eé
    i = ií
    o = oó
    u = uúü
    n = nñ

seems a better option, but I didn't see it as a useful contribution to ripgrep, seing #196, but for rust regex 

---
