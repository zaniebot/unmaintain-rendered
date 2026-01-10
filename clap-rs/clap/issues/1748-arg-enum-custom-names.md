---
number: 1748
title: arg_enum Custom Names
type: issue
state: closed
author: erayerdin
labels: []
assignees: []
created_at: 2020-03-14T23:04:19Z
updated_at: 2020-04-22T18:42:00Z
url: https://github.com/clap-rs/clap/issues/1748
synced_at: 2026-01-10T01:27:04Z
---

# arg_enum Custom Names

---

_Issue opened by @erayerdin on 2020-03-14 23:04_

Currently, we can assign enums to [possible_values](https://docs.rs/clap/2.33.0/clap/struct.Arg.html#method.possible_values) with [arg_enum](https://docs.rs/clap/2.33.0/clap/macro.arg_enum.html) macro. While this is good enough, it is usually considered values to be all lower case. Take the example:

```
arg_enum! {
    enum AuthType {
        HOTP,
        TOTP
    }
}
```

In this case, the possible values for an arg referencing this would be "HOTP" or "TOTP" whereas it would be more convenient to have it "hotp" and "totp".

---

_Label `T: new feature` added by @erayerdin on 2020-03-14 23:04_

---

_Comment by @erayerdin on 2020-03-14 23:06_

All in all what I'm saying is *not* to automatically lowercase every char. While this is the desired output, I think a way to cover the both cases (all lowercase preference and cases where you need some or all uppercase value) would be great.

---

_Comment by @lu-zero on 2020-03-16 07:57_

You may like to use [this](https://github.com/lu-zero/arg_enum_proc_macro) to add aliases.

---

_Comment by @erayerdin on 2020-03-16 10:02_

Hmm, let me check it out.

**Edit:** Thanks @lu-zero. It works like a charm.

On Mon, Mar 16, 2020, 10:58 AM Luca Barbato <notifications@github.com>
wrote:

> You may like to use this <https://github.com/lu-zero/arg_enum_proc_macro>
> to add aliases.
>
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/clap-rs/clap/issues/1748#issuecomment-599394418>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AASJW3C4DIAU5Z6TWFZRVN3RHXLYPANCNFSM4LJ5YANA>
> .
>


---

_Added to milestone `3.0` by @pksunkara on 2020-04-09 08:30_

---

_Label `C: arg enums` added by @pksunkara on 2020-04-12 10:25_

---

_Closed by @bors[bot] on 2020-04-22 18:24_

---

_Comment by @pksunkara on 2020-04-22 18:42_

[Single Alias](https://github.com/clap-rs/clap/blob/c1cb0ce3591e16dd05942fc224565649fd3dfa6b/clap_derive/tests/arg_enum.rs#L192-L217) and [Multiple Alias](https://github.com/clap-rs/clap/blob/c1cb0ce3591e16dd05942fc224565649fd3dfa6b/clap_derive/tests/arg_enum.rs#L220-L251)

---
