```yaml
number: 54
title: "`rg -g foobar` ?"
type: issue
state: closed
author: steckerhalter
labels: []
assignees: []
created_at: 2016-09-24T06:12:36Z
updated_at: 2016-09-25T00:53:57Z
url: https://github.com/BurntSushi/ripgrep/issues/54
synced_at: 2026-01-12T18:23:11Z
```

# `rg -g foobar` ?

---

_@steckerhalter_

I'm used to this from `ag` as a more convenient `find`. Could that be supported? Thanks


---

_Comment by @BurntSushi on 2016-09-24 06:16_

Try `rg -g foo --files`.

On Sep 24, 2016 2:12 AM, "steckerhalter" notifications@github.com wrote:

> I'm used to this from ag as a more convenient find. Could that be
> supported? Thanks
> 
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/54, or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34hAlrJncKSr7U3MY-zR7uMTN0aNFks5qtL9UgaJpZM4KFkTC
> .


---

_Comment by @steckerhalter on 2016-09-24 06:23_

Thanks, a bit more to type though :)

Noted just now that `ag` seems to use wildcards by default:

``` sh
$ ag -g grandshell
etc/grandshell-theme/README.md
etc/grandshell-theme/grandshell-theme-pkg.el
etc/grandshell-theme/grandshell-theme.el
etc/grandshell-theme/grandshell-theme.png
etc/etc/emacs/grandshell-theme.el
```

and it searches the whole path:

``` sh
$ rg -g grandshell --files
$ rg -g *grandshell* --files
./etc/grandshell-theme/grandshell-theme-pkg.el
./etc/grandshell-theme/grandshell-theme.el
./etc/grandshell-theme/grandshell-theme.png
./etc/etc/emacs/grandshell-theme.el
```


---

_Comment by @BurntSushi on 2016-09-24 06:39_

The goal here isn't too do everything exactly like ag. For example, it would be nice to have one glob syntax. If you want the whole path, try `**/grandshell/**`.


---

_Comment by @steckerhalter on 2016-09-24 06:41_

Of course, I'm just noticing and **not** expecting `rg` to be just the same. And maybe you can use this information for some purpose...


---

_Comment by @BurntSushi on 2016-09-25 00:53_

Gotya. Thanks for the bug report, it's useful to be aware of the difference, but I think I'm going to stick with what we have.


---

_Closed by @BurntSushi on 2016-09-25 00:53_

---
