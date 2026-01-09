---
number: 836
title: Refactor color support (+Add Windows Support)
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - A-help
  - E-hard
assignees: []
created_at: 2017-01-30T21:00:09Z
updated_at: 2020-04-17T08:21:47Z
url: https://github.com/clap-rs/clap/issues/836
synced_at: 2026-01-07T13:12:19-06:00
---

# Refactor color support (+Add Windows Support)

---

_Issue opened by @kbknapp on 2017-01-30 21:00_

The formatting really needs some love, and adding Windows support would be a dream.

This is the tracking issues for The Great Color Formatting Refactor as it will henceforth be thou known.

I plan to take all cues from [BurntSushi/ripgrep](https://github.com/Burntsushi/ripgrep)'s handling of color formatting.

---

Another thing that'd be great to add, but is not the focus for this issue is the ability to change the colors and formats. Just something to keep in mind while working.

It'd also be nice to get rid of the `Format` enums and ~~use [`atty`](https://crates.io/crates/atty)~~ (Done in #862)

---

_Label `C: errors` added by @kbknapp on 2017-01-30 21:00_

---

_Label `C: help message` added by @kbknapp on 2017-01-30 21:00_

---

_Label `D: hard` added by @kbknapp on 2017-01-30 21:00_

---

_Label `P3: want to have` added by @kbknapp on 2017-01-30 21:00_

---

_Label `T: enhancement` added by @kbknapp on 2017-01-30 21:00_

---

_Label `W: 2.x` added by @kbknapp on 2017-01-30 21:00_

---

_Referenced in [BurntSushi/ripgrep#353](../../BurntSushi/ripgrep/issues/353.md) on 2017-02-09 16:50_

---

_Comment by @kbknapp on 2017-02-09 17:43_

I've began work on this issue - I'm hoping to have something mergable within the week.

---

_Referenced in [clap-rs/clap#847](../../clap-rs/clap/issues/847.md) on 2017-02-09 17:43_

---

_Referenced in [BurntSushi/ripgrep#324](../../BurntSushi/ripgrep/issues/324.md) on 2017-02-09 19:26_

---

_Comment by @kbknapp on 2017-02-09 20:12_

@pkgw I don't have a public branch yet. I've just been mocking it out, I literally just started messing with it today ;) 

Moving the conversation here so as not to clog up ripgrep traffic.

---

_Added to milestone `2.21.0` by @kbknapp on 2017-02-12 16:27_

---

_Comment by @pkgw on 2017-02-19 16:04_

I started taking a look at this. I've seen at least a few places where `String` types would need to be replaced with `termcolor::Buffer` types to get proper colorized output. This raises a question: would it be OK to have `clap` always depend on the `termcolor` crate, even when the `color` feature is turned off? If yes, life will be a bit easier for implementing the refactored color support. If it's important for that dependency to be optional, I think the cleanest approach to the no-`color` case would be to create some dummy types that emulate those of `termcolor`.

---

_Referenced in [clap-rs/clap#862](../../clap-rs/clap/pulls/862.md) on 2017-02-19 16:15_

---

_Comment by @BurntSushi on 2017-02-19 16:19_

@pkgw Out of curiosity, why do you need the `Buffer` type? At least as I initially envisioned it, that particular type is most useful in multithreaded contexts.

---

_Comment by @pkgw on 2017-02-19 16:27_

@BurntSushi There are functions in `clap` that return Strings containing buffered, colorized output ... because they bake in the assumption that you can implement colors with ANSI escape sequences. If I'm understanding the `termcolor` architecture right, `Buffer` helps in exactly this sort of situation even if you're not threading. But I don't know the design of `clap` very well, so perhaps it wouldn't be hard to change these functions to print their output directly, rather than buffering things. 

---

_Comment by @BurntSushi on 2017-02-19 16:29_

> because they bake in the assumption that you can implement colors with ANSI escape sequences

Ah, yup, that's the key I was missing. If that's the case, then `Buffer` will, incidentally, help. Neat. (Short of restructuring how clap handles colors, as you say.)

---

_Comment by @kbknapp on 2017-02-19 18:46_

> would it be OK to have clap always depend on the termcolor crate, even when the color feature is turned off?

Of course I'd *prefer* to use dummy types or work around pulling in unneeded deps in that case, but if the dep is small and doesn't bloat a resulting binary excessively I would be willing to consider pulling it in unconditionally.

I guess I'd need a concrete example to really see trade-off between the pain of using dummy types and resulting binary size.

> But I don't know the design of clap very well, so perhaps it wouldn't be hard to change these functions to print their output directly, rather than buffering things.

The current coloring support was built around using `ansi_term`, so if that architecture doesn't fit anymore I'm fine with rebuilding something that fits better so long as it can be done in a non-breaking, net neutral effect way.

---

_Comment by @pkgw on 2017-02-19 19:05_

@kbknapp OK, well, dummy types certainly won't be *that* painful to implement, and they're unconditionally better once implemented, so it sounds like that's the way to go.

---

_Comment by @pkgw on 2017-02-21 16:00_

Hmmm, I took more of a look at this and I think that making the change from `ansi_term` to `termcolor` would require a *lot* more knowledge of clap's internals than I have. @kbknapp, this issue is all yours 😄

---

_Referenced in [clap-rs/clap#942](../../clap-rs/clap/issues/942.md) on 2017-05-06 17:59_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-08-21 22:20_

---

_Referenced in [clap-rs/clap#1155](../../clap-rs/clap/issues/1155.md) on 2018-01-31 01:35_

---

_Removed from milestone `2.21.0` by @kbknapp on 2018-02-02 01:57_

---

_Added to milestone `v3-alpha1` by @kbknapp on 2018-02-02 01:57_

---

_Referenced in [clap-rs/clap#1246](../../clap-rs/clap/issues/1246.md) on 2018-04-13 09:25_

---

_Referenced in [clap-rs/clap#1252](../../clap-rs/clap/issues/1252.md) on 2018-06-05 01:15_

---

_Label `W: 3.x` added by @kbknapp on 2018-07-22 01:06_

---

_Label `W: 2.x` removed by @kbknapp on 2018-07-22 01:06_

---

_Removed from milestone `v3-alpha.1` by @kbknapp on 2018-07-22 01:07_

---

_Added to milestone `v3-beta.1` by @kbknapp on 2018-07-22 01:07_

---

_Comment by @IssueHuntBot on 2018-12-03 08:21_

@issuehuntfest has funded $50.00 to this issue. [See it on IssueHunt](https://issuehunt.io/repos/31315121/issues/836)

---

_Referenced in [denoland/deno#2025](../../denoland/deno/pulls/2025.md) on 2019-04-02 20:13_

---

_Comment by @Berrysoft on 2019-05-26 08:06_

If `ansi_term` couldn't be replaced easily, can we use `enable_ansi_support()` on Windows?

---

_Referenced in [clap-rs/clap#1533](../../clap-rs/clap/pulls/1533.md) on 2019-08-23 23:40_

---

_Removed from milestone `v3-beta.1` by @pksunkara on 2020-02-01 07:44_

---

_Added to milestone `v3.0` by @pksunkara on 2020-02-01 07:44_

---

_Assigned to @pksunkara by @pksunkara on 2020-04-11 14:55_

---

_Comment by @pksunkara on 2020-04-11 23:58_

I made a refactor attempt at https://github.com/clap-rs/clap/tree/color. Wasn't enough.

@BurntSushi I am missing some functions for termcolor's Buffer.

* Appending buffers to each other so that I can combine them in any way
* Getting display width of buffer (not `len()`)

---

_Referenced in [clap-rs/clap#1812](../../clap-rs/clap/pulls/1812.md) on 2020-04-12 08:21_

---

_Label `C: colorizing` added by @pksunkara on 2020-04-12 10:16_

---

_Comment by @BurntSushi on 2020-04-12 11:41_

@pksunkara Sorry, but I don't understand what you're asking. Could you please say more words? Examples and context would help a lot.

---

_Comment by @pksunkara on 2020-04-12 17:59_

What's happening is, when we try to print help, currently clap treats them as ansi strings and creates severals strings and combines them later. A good example is [this](https://github.com/clap-rs/clap/blob/333b993481396d84197bd6e5b54c526a3b04e6a1/src/output/help.rs#L439) function. If I am passing around just 1 buffer for everything to write into, this  will not work. So, I wanted to create buffers (like how the strings were created) and then combine them. I went through the termcolor docs and code and I couldn't understand how I can achieve this. Sorry, If I missed anything.

Secondly, clap tries to wrap text based on terminal size. We try to check if we need to wrap based on the string that was returned. You can see an example [here](https://github.com/clap-rs/clap/blob/333b993481396d84197bd6e5b54c526a3b04e6a1/src/output/help.rs#L296-L298). That basically calculates the length of the colored string which is not possible with Buffer.

Hope I was able to give more clarity.

---

_Comment by @BurntSushi on 2020-04-13 12:20_

@pksunkara Thanks, that helps a bit. It might help for you to read the docs on the internal `WindowBuffer` type to get an idea of what `termcolor` is doing here: https://docs.rs/termcolor/1.1.0/src/termcolor/lib.rs.html#1467

In particular, the whole point of `termcolor` is to provide a _cross platform_ API that works even with the Windows console API. When using the Windows console APIs, `termcolor` doesn't use ANSI escape sequences at all, so you fundamentally cannot treat colorized text as just a normal string. That's why the refactor is hard. You'll have to re-arrange how the output works to stop passing around strings. If you can refactor your code to just write to `termcolor::Write` values instead of strings, then that should work.

> That basically calculates the length of the colored string which is not possible with Buffer.

I'm not following why this isn't possible. `Buffer` exposes its contents via `as_slice`. Why isn't that sufficient?

---

_Comment by @pksunkara on 2020-04-13 13:36_

Yup, I got that part from reading the code. What I was asking was basically if we can support the append somehow. Maybe refactoring the WindowsBuffer to be like:

```rust
struct WindowsBufferItem {
    buf: Vec<u8>,
    colors: Vec<(uszie, Option<ColorSpec>)>,
}

struct WindowsBuffer(Vec<WindowsBufferItem>);
```

(basically `Vec<current WindowsBuffer>`). This would make it easy for us to support appending buffers to each other without recalculating the `colors` field all the time.

> I'm not following why this isn't possible. Buffer exposes its contents via as_slice. Why isn't that sufficient?

Because non-windows buffer has the ansi codes in the string. When I am calculating the length, I want the plain content. We could try to make it possible by changing the `Ansi(_)` to be similar to how `WindowsBuffer` works. That way, we only add the ansi codes when printing. Hopefully I didn't overlook anything while proposing this.

---

_Comment by @BurntSushi on 2020-04-13 13:52_

@pksunkara That's a possibility. I'm open to PRs for that. I don't think there should be any breaking changes. It still seems to me like you should be able to refactor the code so that you don't append to buffers. That is, instead of:

```rust
let buf1 = do_foo();
let buf2 = do_bar();
let buf3 = buf1 + buf2;
```

you would do

```rust
let mut buf = String::new();
do_foo(&mut buf);
do_bar(&mut buf);
```

And then at that point, it should be possible to swap out `String` for `termcolor::Buffer`.

> Because non-windows buffer has the ansi codes in the string. When I am calculating the length, I want the plain content.

Yes. But wasn't that true before? I'm missing something here. Why wasn't this a problem before?

But if you just need the buffer without ANSI codes, then it's pretty easy to just strip them: https://github.com/BurntSushi/tabwriter/blob/2c0c9ff0d7a2bc522dfed55ab64605a0a21b23d2/src/lib.rs#L412-L420

> We could try to make it possible by changing the Ansi(_) to be similar to how WindowsBuffer works.

I see why you might want that, but that's definitely not possible. That's almost certainly going to result in a noticeable loss in performance due to the extra indirection. Windows does it because it has to.

---

_Comment by @pksunkara on 2020-04-13 14:00_

> Yes. But wasn't that true before? I'm missing something here. Why wasn't this a problem before?

It is a problem currently too. I was hoping to fix it with this refactor and thats why I mentioned it originally.

I will give it another attempt and see if I really need the appending. Thanks for letting me bounce my thoughts with you.

---

_Comment by @BurntSushi on 2020-04-13 14:13_

@pksunkara No problem. If it ends up being impossible, then I'm open to making it possible to append `termcolor::Buffer` values.

Note that I would definitely recommend against using a `regex` to strip ANSI formatting because of the weight of that dependency. My link was mostly just meant as an example. I think a small hand written search routine would be best. :-) (Arguably, `tabwriter` should do the same.)

---

_Comment by @pksunkara on 2020-04-13 14:14_

I will probably end up building a non colored string at the same time and using it for calculation

---

_Comment by @CreepySkeleton on 2020-04-13 23:18_

Whenever I see a technical dispute is going on, I can't resist the urge to jump in, you know me.

I would probably end up with storing an error as `Vec<(String, Color)>` on both Unix and Windows instead of melding escapes into it. When the time comes and error is *requested* to be rendered, we just check where it's being rendered to - if it's console/TTY, we're using coloring API/escapes, but if it a normal in-memory buffer, we're just flushing strings into it one by one.

---

_Closed by @bors[bot] on 2020-04-16 13:14_

---

_Comment by @IssueHuntBot on 2020-04-17 08:21_

[@pksunkara](https://issuehunt.io/u/pksunkara) has rewarded $45.00 to [@pksunkara](https://issuehunt.io/u/pksunkara). [See it on IssueHunt](https://issuehunt.io/repos/31315121/issues/836)

- :moneybag: Total deposit: $50.00
- :tada: Repository reward(0%): $0.00
- :wrench: Service fee(10%): $5.00

---

_Referenced in [sharkdp/bat#1306](../../sharkdp/bat/issues/1306.md) on 2020-12-29 08:09_

---
