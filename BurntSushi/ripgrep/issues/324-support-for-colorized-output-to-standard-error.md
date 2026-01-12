```yaml
number: 324
title: Support for colorized output to standard error
type: issue
state: closed
author: pkgw
labels:
  - question
assignees: []
created_at: 2017-01-16T00:43:01Z
updated_at: 2017-02-10T01:57:25Z
url: https://github.com/BurntSushi/ripgrep/issues/324
synced_at: 2026-01-12T16:13:21Z
```

# Support for colorized output to standard error

---

_@pkgw_

As per pull request [#323](https://github.com/BurntSushi/ripgrep/pull/323). It should be possible to write colorized output to standard error, as well as standard output.

The pull request above took the tack of adding some helpers that are generic over the two types `io::Stdout` and `io::Stderr`, but @BurntSushi does not like that approach. I took that approach to avoid having to duplicate code between the two cases. I think that @BurntSushi's suggestion of using macros is probably the best alternative.

I still think that it would be nice for crate users to be able to abstract between stdout and stderr more easily, though. Perhaps involving something like:

```
pub enum StandardStreamType { Stdout, Stderr };
```

But the key struct in `termcolor` is named `Stdout`, so it would be tricky to something that added genericity in this way that also maintained compatibility with existing code.

---

_Comment by @BurntSushi on 2017-01-16 01:24_

FWIW, making a breaking change is okay. We can just increase the minor version. The aren't many people using this crate yet, so we should feel free to be a bit liberal.

With that said, I would rather take cues from std, which defines Stdout and Stderr concrete types. Programmers can write generic code over Write.

For termcolor, I don't see why the same can't be done. There is already a WriteColor trait that Stdout satisfies, and which Stderr should also satisfy. Can you explain why you think this is insufficient?

In general, for generic APIs, I think we should have concrete examples motivating the use of generics.

---

_Comment by @pkgw on 2017-01-16 01:54_

Point 1: I think `std` just contains a poor design decision. IMO it would have been better if the `std::io` interface had used an enum like the `StandardStreamType` I suggested and a single function returning a single type rather than the pairs of `stdout() -> Stdout` and `stderr() -> Stderr`. The underlying functionality is complete identical except for a different FD/HANDLE number; but because the functions' return values are two separate types, all code that could work with both (that does anything beyond the Write trait) needs to special-case both.

Point 2: As for the value of adding some ability to abstract between the two, I think it's like you said in the PR: concrete types are preferable when possible. Consider the `BufferWriter` struct, for instance. It currently starts with:

```
pub struct BufferWriter {
   stdout: LossyStdout<io::Stdout>,
   ...
}
```

How to you adapt it to handle either stdout or stderr? You can stick an enum in there .... or you can create parallel `StdoutBufferWriter` and `StderrBufferWriter` structs ... but it seem to me that it'd be vastly preferable to be able to keep the structure non-generic, and not start creating parallel families of structs that only differ in `s/stdout/stderr/`. It's too late to undo the choices in `std::io`, but they can at least be covered up.

---

_Label `question` added by @BurntSushi on 2017-01-18 00:44_

---

_Comment by @pkgw on 2017-02-01 23:29_

Any thoughts about what approach you think would work best? I think the `BufferWriter` example above is a nice demonstration of why it would be good to provide a single struct that can write to either stdout or stderr under the hood.

---

_Comment by @BurntSushi on 2017-02-01 23:59_

I haven't had time to really dig into this, but if I ignore what you've said already, this is what I'd expect to see:

1. A new constructor, `stderr`, added to `BufferWriter`.
2. Two new types, `Stderr` and `StderrLock` that mirror the `Stdout` and `StdoutLock` types.
3. Implementations of `WriteColor` for `Stderr` and `StderrLock`.

Unless I've missed something else, I don't think there should be any other new public parts of the API. If you want to write code that is generic over `Stdout` and `Stderr`, then you can use `W: WriteColor`.

It seems like using an enum internally is probably the simplest approach.

---

_Comment by @pkgw on 2017-02-05 15:32_

If breaking the API is OK, I want to make one last argument for having a single type named something like `StandardStream` having two constructors, rather than separate `Stdout` and `Stderr` types. (Plus `Lock` variants.) My claim is that the design in `std` is just not well thought-through and is not worth emulating. Ignoring the example of `std` I think every API designer would say that "stdout" and "stderr" should be different *instances* of a type but not different *types*. I cannot think of a case where having the types be different buys you anything at all, and it definitely leads to hassles in some situations. If the API can be broken then I can avoid exposing inessential helper traits as I did in the first PR.

In this model the list of changes is more concise in a way that I think demonstrates its advantages:

1. Rename `Stdout{,Lock}` to `StandardStream{,Lock}`
2. Add `stderr` constructors to `StandardStream` and `BufferWriter`.

The particular hassle I've encountered is that I want to port `clap` to use `termcolor` so that it can do colorized help on Windows, and there would have to be enum-related ugliness if `Stdout` and `Stderr` were different types.

---

_Comment by @BurntSushi on 2017-02-05 15:47_

@pkgw I see. I didn't realize you were trying not to break the existing API (or I've forgotten). The `termcolor` crate is young enough that breaking changes would be welcome IMO if it leads to a better API.

I can't immediately poke any holes in your proposal, and I like its simplicity. I say go for it.

---

_Comment by @pkgw on 2017-02-05 15:55_

OK, I'll take a stab at actually implementing the idea!

---

_Comment by @pkgw on 2017-02-05 17:22_

OK, implementation was even more straightforward than I was hoping. See PR #350.

---

_Comment by @kbknapp on 2017-02-09 19:26_

So funny story - Since #353 I've began working on getting `clap` working on `termcolor` and ran into similar issues as discussed here. I remembered reading about this PR/Issue about adding `Stderr` to `termcolor` so I came looking. And I find it's coming from porting `clap` to `termcolor`. I got a good giggle out of this.

@pkgw once/if this is merged please feel free to take a stab at porting `clap` to `termcolor`. Even if I get an initial port done, I'm always open to new/better ideas! And with my current work schedule, you might beat me to it anyways :wink: I just didn't want you seeing kbknapp/clap-rs#836 and getting discouraged!

---

_Comment by @BurntSushi on 2017-02-09 19:38_

@kbknapp Aye, thanks for the ping. I've had the flu the past few days, so I've fallen a bit behind. #350 looks good, just needs a squash. Once merged, I'll do a release.

---

_Comment by @pkgw on 2017-02-09 19:52_

Ah @kbknapp clearly great minds are thinking alike. Do you have a public branch with work-in-progress on the `termcolor` port of `clap`? It looked like some fairly invasive changes would be required so I'm not keen to duplicate work.

---

_Closed by @BurntSushi on 2017-02-10 01:57_

---
