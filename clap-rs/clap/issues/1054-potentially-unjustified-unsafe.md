```yaml
number: 1054
title: "Potentially unjustified `unsafe`"
type: issue
state: closed
author: H2CO3
labels:
  - C-enhancement
  - E-medium
assignees: []
created_at: 2017-09-27T08:08:29Z
updated_at: 2018-08-02T03:30:11Z
url: https://github.com/clap-rs/clap/issues/1054
synced_at: 2026-01-12T16:14:10Z
```

# Potentially unjustified `unsafe`

---

_@H2CO3_

There are a couple of uses of `unsafe` in the code that I think are not strictly necessary. The prime example is this:

https://github.com/kbknapp/clap-rs/blob/6e948994a61e389c8f3b6726435d3d14bdd328bb/src/app/parser.rs#L1350-L1362

I understand that this instance of `unsafe` might have been introduced for performance reasons, however I don't think giving up safety is a clear win here. When printing help text for the user, I/O time probably dominates UTF-8 validation time, and help texts aren't enormously long either, usually (typically at most in the tens of kilobytes).

Similarly, in https://github.com/kbknapp/clap-rs/blob/6e948994a61e389c8f3b6726435d3d14bdd328bb/src/app/help.rs#L850-L855
it seems *highly* unlikely to me that a debug build would experience a bottleneck in the UTF-8 validation of this one method.


---

_Comment by @kbknapp on 2017-10-04 02:53_

Agreed! I think those were either left over from old code, or under assumptions that only ascii code would be supported. Both of which seem wrong now.

The debug code one I actually want to look up `git blame` and see if that's something I did :stuck_out_tongue_winking_eye: as I *normally* don't care at all about debug performance. Even if I didn't write it, I still accepted it, so there's no escaping the blame :laughing: 

I'd be good with a PR removing these uses if someone wants a quick PR!

---

_Label `D: easy` added by @kbknapp on 2017-10-04 02:54_

---

_Label `M: mentored` added by @kbknapp on 2017-10-04 02:54_

---

_Label `P3: want to have` added by @kbknapp on 2017-10-04 02:54_

---

_Label `T: enhancement` added by @kbknapp on 2017-10-04 02:54_

---

_Label `W: 2.x` added by @kbknapp on 2017-10-04 02:54_

---

_Comment by @H2CO3 on 2017-10-04 09:21_

Awesome, in this case I'll open a PR soon!


---

_Comment by @H2CO3 on 2017-10-04 09:57_

There are two more instances of `unsafe` I'd like to address separately, if possible. First one is:

https://github.com/kbknapp/clap-rs/blob/1ef6e62e1e05f3a1c5dee4d946eb85c9778e4065/src/macros.rs#L430-L442

If this is `static` for performance reasons, it could be realized using the dedicated `lazy_static` crate instead to avoid reinventing the wheel.

*However,* better yet, `static`ness could also be removed altogether, with the following additional advantages:
1. This gives more control to the user (if they wanted `static`, they could wrap it into a `lazy_static`, and if they preferred no `unsafe`, they could move it to a place in their code so that no `static` is necessary). This is in accordance with the common Rust design guideline that the consumer of an API should be able to decide how they want to use it, potentially based on context (for example, functions that need a value should generally take it by move, not by reference and then magically `clone()`ing inside).
2. The current implementation of the macro is such that when called multiple times, it will always return a result that respects the separator used in the first call. This might be counter-intuitive when it's called more than once with different separator arguments. I wouldn't necessarily call it a *bug* as long as it's documented, but it can be surprising.


---

_Comment by @H2CO3 on 2017-10-04 10:05_

The second one is:

https://github.com/kbknapp/clap-rs/blob/53f3b65d2469a00eefa3cbad5a2650d602dbbfc8/src/osstringext.rs#L26-L32

Wouldn't be it better to, instead of creating our own `from_bytes` method, use the already-existing one provided by `OsStr` itself, and use `OsStr`s instead of `&OsStr`s? There aren't many places this is being used (https://github.com/kbknapp/clap-rs/search?utf8=%E2%9C%93&q=from_bytes&type=), so it should also be a relatively painless change.


---

_Comment by @kbknapp on 2017-10-04 14:30_

> [crate authors]

I'm good trying this method if it can be implemented in a non-breaking manner. Even as unfortunate as it is, if we can't do this in a non-breaking way it'll have to wait.

> [OsStrExt3]

This was written before the actual `from_bytes` was stablized. Now that we're well beyond the stable minus two releases from when `from_bytes` was stablized, I'm good with updating it so long as we ensure we bump the minor version of this lib and document that a newer version of Rust is required. 

---

_Comment by @H2CO3 on 2017-10-04 14:39_

> `[crate authors]`

"Non-breaking" meaning retaining the incorrect behavior with regards to the separator, or leaving it `static`? And if the former, wouldn't this classify as a bugfix rather than a breaking change? (AFAIK semver allows breaking changes without major version bump if they are actually bugfixes.)

> `[OsStrExt3]`

Sure, will try to do that as well, then!


---

_Comment by @kbknapp on 2017-10-04 14:52_

> "Non-breaking" meaning retaining the incorrect behavior with regards to the separator, or leaving it static? And if the former, wouldn't this classify as a bugfix rather than a breaking change? (AFAIK semver allows breaking changes without major version bump if they are actually bugfixes.)

I mean non breaking, meaning change of user code *with the exception* of correcting a bugfix like you stated. Specifically, I'm talking about removing `static`ness altogether. For the bug about using crate_authors multiple times, it seems unlikely to me. I agree, its a bug per-se but only due to implementation or lack of being documented. I have yet to have anyone actually complain about this bug. The only thing I'm trying to avoid (*not* that you're advocating this, I'm just making my position clear 😄 ) is justifying a breaking change behind this one bug might be a stretch of allowable semver.

Thanks for tackling all this by the way!


---

_Comment by @H2CO3 on 2017-10-04 15:13_

Oooh, alright. Sure thing – of course I'm advocating for making these changes without user code having to change. My suggestion about "giving users the control over `static`ness" might have sounded contradictory to that, in fact.

And indeed there may be no non-breaking way to remove `static`ness since the macro yields a pointer. I'll see — but I'll try very hard to do it anyway.

And, as always — you are welcome! I'm happy to improve libraries that I love.


---

_Referenced in [clap-rs/clap#1058](../../clap-rs/clap/pulls/1058.md) on 2017-10-04 22:29_

---

_Comment by @H2CO3 on 2017-10-07 19:04_

This has been resolved via https://github.com/kbknapp/clap-rs/pull/1058.


---

_Closed by @H2CO3 on 2017-10-07 19:04_

---

_Referenced in [clap-rs/clap#1594](../../clap-rs/clap/issues/1594.md) on 2019-11-03 15:25_

---
