```yaml
number: 4554
title: template documentation seems hard to find and possibly incomplete
type: issue
state: closed
author: ehuss
labels: []
assignees: []
created_at: 2022-12-14T23:28:59Z
updated_at: 2023-01-31T22:42:27Z
url: https://github.com/clap-rs/clap/issues/4554
synced_at: 2026-01-12T16:14:16Z
```

# template documentation seems hard to find and possibly incomplete

---

_@ehuss_

I was trying to find some documentation that would explain what `{n}` means in a help string. I ran across several issues:

* I wasn't able to find any mention of `{n}`, but I assume it adds a newline in the output. 
* It's not clear to me what the difference is between that and an actual newline.
* I did find a list of replacement strings at [`help_template`](https://docs.rs/clap/latest/clap/builder/struct.Command.html#method.help_template), but I don't know in what circumstances those strings can be used. In my case, it was being passed to the `help` function.
* [`help`](https://docs.rs/clap/latest/clap/builder/struct.Arg.html#method.help) takes an `impl IntoResettable<StyledStr>`. It's not clear to me what that means. The current documentation for `StyledStr`, `IntoResettable`, and `Resettable` don't really tell me what they are for, or what kind of values it accepts and how they get interpreted.

It would be helpful to have some more information, something along the lines of:
* Add `{n}` unless I missed it somewhere.
* For each place that takes some kind of templated text, provide a link to some documentation that explains what placeholders it accepts.
* Provide a little more information on what `StyledStr` and `Resettable` are, what kinds of values can be typically used, and why one would use them.

Clap version: 4.0.29


---

_Referenced in [clap-rs/clap#4555](../../clap-rs/clap/pulls/4555.md) on 2022-12-15 01:21_

---

_Comment by @epage on 2022-12-15 01:29_

`{n}` is something we've been wanting to remove for a whole but its kept around solely for the derive API to workaround some limitations in how we process doc comments (#3230).  As such, we don't have it documented as it should only be used on an exception basis.   Granted, that doesn't help people who are looking at old code ported from clap 2, wondering what it is.  That is always a challenge with deprecations, balancing between new uses and legacy uses.

As such, `{n}` works differently than the other replacement strings which only work when passed to `help_template`.

I've clarified the role of `Resettable` in #4555.  `StyledStr` is in transition atm.  For users of clap, its effectively a `Str` atm.  We put it in the API now for us to expose to the user terminal styling.  Unfortunately, some work on toml/toml_edit has derailed me from getting that terminal styling work done.  I guess I note on this could be useful, so I've added it to #4555

---

_Comment by @epage on 2023-01-31 22:12_

@ehuss any thing further or are you ok with this being closed out?

---

_Comment by @ehuss on 2023-01-31 22:42_

Sure, it sounds like if `{n}` will get removed at some point, it's not really worth bring back docs for it.



---

_Closed by @ehuss on 2023-01-31 22:42_

---
