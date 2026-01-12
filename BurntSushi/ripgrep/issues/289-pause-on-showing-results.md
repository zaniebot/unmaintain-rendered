```yaml
number: 289
title: pause on showing results
type: issue
state: closed
author: ruizander
labels:
  - question
assignees: []
created_at: 2016-12-23T19:07:06Z
updated_at: 2018-02-20T12:46:07Z
url: https://github.com/BurntSushi/ripgrep/issues/289
synced_at: 2026-01-12T16:13:21Z
```

# pause on showing results

---

_@ruizander_

it would be good to have a option to pause the search maintaining the colors and format... if we use "| more" the colors are gone....

---

_Comment by @BurntSushi on 2016-12-23 19:12_

Use the -p flag.

On Dec 23, 2016 2:07 PM, "ruizander" <notifications@github.com> wrote:

> it would be good to have a option to pause the search maintaining the
> colors and format... if we use "| more" the colors are gone....
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/289>, or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34h5YzxYvrUTy_CkibosIX4eHMWPrks5rLBvagaJpZM4LVA5U>
> .
>


---

_Comment by @BurntSushi on 2016-12-23 20:16_

I was on mobile before. Here's a more useful workaround:

```
$ cat $HOME/bin/rgp
#!/bin/sh

exec rg -p "$@" | less -RFX
```

Then you can just use `rgp` whenever you want to have a pager pause results for you.

---

_Closed by @BurntSushi on 2016-12-23 20:16_

---

_Comment by @ibotty on 2018-02-12 08:16_

Can there be a flag to (more intelligently) do that with ripgrep alone? Akin to what `git grep` does: forking `$PAGER` and connecting stdout to `$PAGER`'s stdin, but only with interactive sessions. Would you accept a patch that does that?

---

_Comment by @BurntSushi on 2018-02-12 09:48_

@ibotty I think that has already been requested. I'm on mobile, but could you please find that issue, read it, and respond to the arguments given against doing that? I would in particular like to hear what a wrapper script is insufficient.

---

_Comment by @ibotty on 2018-02-12 14:19_

That is the issue I am responding to.

A wrapper script is not as comfortable as command line flags. I do use such a wrapper script (as `rg`), but whenever I want to pipe rg's output I have to remember using `\rg`. It's no big deal, but not as nice as a built-in mechanism.

---

_Comment by @BurntSushi on 2018-02-12 14:27_

@ibotty Sorry, this was the issue I was referring to: https://github.com/BurntSushi/ripgrep/issues/86 --- It would help if you addressed the concerns in that issue (is this a cross platform feature?) and why specifically you can't pipe when using `less -RFX`.

---

_Comment by @ibotty on 2018-02-12 15:35_

Oh, I can and do pipe to `less -RFX`. I'll comment on #86.

---

_Label `question` added by @BurntSushi on 2018-02-20 12:46_

---
