```yaml
number: 1107
title: "No info on how `ripgrep` compares to `pcregrep` and `pcre2grep`"
type: issue
state: closed
author: Hermholtz
labels: []
assignees: []
created_at: 2018-11-15T10:26:15Z
updated_at: 2018-11-15T12:05:52Z
url: https://github.com/BurntSushi/ripgrep/issues/1107
synced_at: 2026-01-12T16:13:22Z
```

# No info on how `ripgrep` compares to `pcregrep` and `pcre2grep`

---

_@Hermholtz_

I've just come across this utility. For years I've been using `pcregrep` and then `pcre2grep` which are marvelous. In README there's a lot about comparing `ripgrep` to other tools, but no word on `pcre[2]grep`. Is it possible to fill this gap?

---

_Comment by @BurntSushi on 2018-11-15 10:36_

What questions do you want answered? The primary comparison point with
other tools in the README is mostly performance oriented.

On Thu, Nov 15, 2018, 05:26 chojrak11 <notifications@github.com wrote:

> I've just come across this utility. For years I've been using pcregrep
> and then pcre2grep which are marvelous. In README there's a lot about
> comparing ripgrep to other tools, but no word on pcre[2]grep. Is it
> possible to fill this gap?
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/1107>, or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34oYkY9T2lxP4wdMBfFO3sqjZ_OJ8ks5uvUFJgaJpZM4YfgJJ>
> .
>


---

_Comment by @Hermholtz on 2018-11-15 10:50_

Sorry it was just an impulse feeling of missing a favorite tool in the table. I've read what `ripgrep` is about, I can see the difference, and perf-wise I don't care, so to answer your question: I'm not sure. I'm closing this.

---

_Closed by @Hermholtz on 2018-11-15 10:50_

---

_Comment by @BurntSushi on 2018-11-15 12:05_

All righty. I personally haven't used `pcre2grep` much myself, however, I did use it as a baseline when adding the `-P/--pcre2` flag to ripgrep in terms of performance.

Quickly scanning `pcre2grep`'s man page, nothing jumps out at me as a feature that isn't in ripgrep, other than perhaps POSIX locale support. (ripgrep does support transcoding from a number of different encodings however, including UTF-16, which I don't think `pcre2grep` supports.)

At first glance, it looked like `pcre2grep` didn't have Unicode support, but you can enable it via the special `(*UCP)` directive in the pattern when combined with the `-u` flag (short for "UTF-8 mode"). e.g., `pcre2grep -u '(*UCP)Napol\won'` will match `Napoléon`, but if you remove _either_ of `-u` or `(*UCP)`, then `Napoléon` is not matched. So one place where ripgrep does well here is that it can use PCRE2 and its Unicode features even on input that is not valid UTF-8. For example:

```
$ echo $'\xFFNapoléon' | pcre2grep -u '(*UCP)Napol\won'
pcre2grep: pcre2_match() gave error -23 while matching this text:

Napoléon

$ echo $'\xFFNapoléon' | rg 'Napol\won'
Napoléon
```

---
