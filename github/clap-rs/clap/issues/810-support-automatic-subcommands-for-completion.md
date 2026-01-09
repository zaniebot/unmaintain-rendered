---
number: 810
title: Support automatic subcommands for completion
type: issue
state: closed
author: colemickens
labels:
  - C-enhancement
  - A-completion
  - S-waiting-on-design
assignees: []
created_at: 2017-01-07T23:48:52Z
updated_at: 2024-08-10T00:31:54Z
url: https://github.com/clap-rs/clap/issues/810
synced_at: 2026-01-07T13:12:19-06:00
---

# Support automatic subcommands for completion

---

_Issue opened by @colemickens on 2017-01-07 23:48_

It seems to be something of a pattern for apps to support completion via the following:

```shell
$ source <(app completion zsh)
# or
$ source <(app completion bash)
```

It'd be nice if there were a helper on `App` so that we could call `.add_completion_subcommands()` and have this wired up automatically.

---

_Comment by @kbknapp on 2017-01-08 03:14_

This would be an easy addition, although it'd probably get implemented as an `AppSettings` variant.

The one thing that gives me pause, is that I've seen this done in a few different ways by different applications, such as `$ prog completion[s] [--shell] <shell>`. 

Is the `prog completion <shell>` (singular form of "completion") a standard? Because if it were only up to me, I'd implement it as `prog completions <shell>` (plural form of "completion" with a single positional argument accepting the shell name), but if there's a written standard I haven't seen I'd rather follow the standard.

---

_Label `C: settings` added by @kbknapp on 2017-01-08 03:15_

---

_Label `D: easy` added by @kbknapp on 2017-01-08 03:15_

---

_Label `P4: nice to have` added by @kbknapp on 2017-01-08 03:15_

---

_Label `T: new setting` added by @kbknapp on 2017-01-08 03:15_

---

_Label `T: RFC / question` added by @kbknapp on 2017-01-08 03:15_

---

_Label `W: 2.x` added by @kbknapp on 2017-01-08 03:15_

---

_Comment by @colemickens on 2017-01-08 04:26_

I don't know if one is more popular/"standard" than another. I thought I had other examples up my sleeve, but the only one I can think of now is `kubectl` which takes the form `kubectl completion zsh`. I have only a tiny preference for the subcommand style, rather than flag, but it seems to just be cosmetic.

---

_Comment by @kbknapp on 2017-01-08 05:11_

In all honesty it'd be possible to give options for both, which is how I think I'll tackle this.

* `AppSettings::GenerateCompletionsSubcommand`
  * Will generate `prog completions <shell>`
* `AppSettings::GenerateCompletionsArg`
  * Will generate `prog --shell-completions <shell>`

---

_Added to milestone `2.21.0` by @kbknapp on 2017-01-30 05:11_

---

_Removed from milestone `2.21.0` by @kbknapp on 2017-03-27 02:57_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2018-02-15 14:51_

---

_Label `W: 3.x` added by @kbknapp on 2018-07-22 01:05_

---

_Label `T: RFC / question` removed by @kbknapp on 2018-07-22 01:05_

---

_Label `W: 2.x` removed by @kbknapp on 2018-07-22 01:05_

---

_Added to milestone `v3.0` by @kbknapp on 2018-07-22 01:06_

---

_Referenced in [rust-lang/cargo#6645](../../rust-lang/cargo/issues/6645.md) on 2019-04-23 19:28_

---

_Comment by @rharriso on 2019-07-04 22:06_

Is this possible anymore?

The clap_generate package depends on clap. Won't this be a circular package dependency?
Also. I'm having trouble using `clap_generate`

working in this branch:
https://github.com/rharriso/clap/tree/v3-master

---

_Comment by @pksunkara on 2020-02-14 14:42_

@rharriso Try the clap_generate now, it should work. But yeah, this would become a circular dependency. I am not sure we want this.

Maybe we could do some kind of macro.

---

_Comment by @pksunkara on 2020-03-03 12:45_

@CreepySkeleton Would love your input here.

---

_Comment by @CreepySkeleton on 2020-03-03 15:43_

That's possible. We can make `clap_generate` just another module in `clap` (i.e it would be no longer a separate crate), possibly behind a feature flag.
  
  **Pros:** 
  * Very convenient for end users.
  * We will no longer need the `#[doc(hidden)] pub` things. 

    Let me catch your attention here: currently, a lot of stuff in clap is `doc(hidden)` *precisely and only* because `clap_generate` needs it. It's much more that you might have thought; many things (`struct`s and `enum`s) are public only because they are parts of such a "public" field, or a part of some function's signature (also `pub hidden` one). The point is: this pseudo-publicity *expands* onto other things transitively, pulling them into the club even though they have no need to be publich. 

    In my estimation, about 30% of code is `pub hidden` now, and those erase the line between "internal API used by `clap_generate`" and really "public API". Once/if we embrace the generate into the `clap` itself, we'll be able to change them to `pub(crate)`, *as it should be*. A big improvement if you ask me.

  **Cons:**
    * `clap`'s own codebase would grow a bit. Well, we're the ones who's gonna have to maintain it one way or another 🤷‍♂ .
  

---

_Comment by @TeXitoi on 2020-03-03 15:47_

@CreepySkeleton You mean `clap_generate` instead of `clap_derive` ?

---

_Comment by @pksunkara on 2020-03-03 15:51_

There must have been a reason Kevin wanted to make it a separate crate.

On Tue, Mar 3, 2020, 16:47 Guillaume P. <notifications@github.com> wrote:

> @CreepySkeleton <https://github.com/CreepySkeleton> You mean clap_generate
> instead of clap_derive ?
>
> —
> You are receiving this because you commented.
> Reply to this email directly, view it on GitHub
> <https://github.com/clap-rs/clap/issues/810?email_source=notifications&email_token=AABKU35NWB52OG4OZCVHKO3RFURBVA5CNFSM4C3WHVVKYY3PNVWWK3TUL52HS4DFVREXG43VMVBW63LNMVXHJKTDN5WW2ZLOORPWSZGOENUAHZY#issuecomment-594019303>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AABKU3ZGNHAF4PG3EOK2DJ3RFURBVANCNFSM4C3WHVVA>
> .
>


---

_Comment by @CreepySkeleton on 2020-03-03 15:53_

@TeXitoi right 😨 

---

_Comment by @CreepySkeleton on 2020-03-03 15:54_

I don't see any reason, honestly. Not any I would call a good one.

---

_Comment by @TeXitoi on 2020-03-03 16:47_

The advantage is that you will not compile it if you don't use it. The other solution is features, and I personally prefer to keep features to the bare minimum as they complexify the code considerably.

But, having this implementation hole is quite dirty. I think the clean way would be:
 - having the introspection API used by clap_generate public and documented
 - having clap_generate in the clap crate

The second way seems easier now. But that's just my humble opinion, I didn't even seen the code.

---

_Comment by @pksunkara on 2020-03-03 16:55_

I made as much of introspection API public. The remaning stuff are `doc(hidden)` because they are macro helpers for the introspection API. If the whole system isn't macros, they would have been private.

---

_Comment by @CreepySkeleton on 2020-03-03 17:00_

> The other solution is features, and I personally prefer to keep features to the bare minimum as they complexify the code considerably.

That's gonna be one module level `cfg` and one another `cfg` somewhere in `AppSettings`. Sounds simple enough.

I've seen the code, I've been trying to implement an introspection API of sorts (you just read my mind 😃, ha!), and I feel like I'm failing. `calp_generate` and `clap` are *so* closely tied to each other that we may as well make them all public. I'll show you the result of this attempt soon, but I vote for pulling it into `clap`.

> I made as much of introspection API public.

You did? Mind elaborate?

---

_Comment by @CreepySkeleton on 2020-03-03 17:05_

> The remaning stuff are doc(hidden) because they are macro helpers for the introspection API. If the whole system isn't macros, they would have been private.

I hate to be rude, but that's actually not true at all. Just grep for `#[doc(hidden)]`: 140 matches across 17 files. Ridiculous.


---

_Comment by @CreepySkeleton on 2020-03-03 17:24_

@TeXitoi They seem to be implementing explicit support for features in docs.rs, see https://github.com/dtolnay/syn/pull/734 . We might as well do that too.

Looks neat unless you have lots of cfg'ed items (we don't): https://docs.rs/tokio/0.2.13/tokio/index.html#modules

---

_Referenced in [clap-rs/clap#1736](../../clap-rs/clap/pulls/1736.md) on 2020-03-10 16:22_

---

_Label `C: generator` added by @pksunkara on 2020-04-10 13:59_

---

_Label `C: settings` removed by @pksunkara on 2020-04-10 13:59_

---

_Comment by @CreepySkeleton on 2020-07-01 02:27_

@pksunkara @kbknapp So can we finally decide whether we want this or not? More specifically, are we open to inclusion `clap_generate` as a clap module? If we aren't, than we can close this as impossible.

---

_Assigned to @pksunkara by @pksunkara on 2020-07-01 08:14_

---

_Comment by @pksunkara on 2020-07-01 08:15_

I will take care of this. I have a few ideas on how to fix this.

---

_Comment by @epage on 2021-07-21 19:58_

btw another way of solving this is instead of having a setting for this, to provide types that implement `Args` and `Subcommand` (through derive or not).  A user could then choose to flatten those into their arguments.

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [clap-rs/clap#2720](../../clap-rs/clap/pulls/2720.md) on 2021-08-18 17:15_

---

_Removed from milestone `3.0` by @epage on 2021-10-16 18:12_

---

_Added to milestone `3.1` by @epage on 2021-10-16 18:12_

---

_Referenced in [epage/clapng#68](../../epage/clapng/issues/68.md) on 2021-12-06 16:31_

---

_Label `C-enhancement` added by @epage on 2021-12-08 20:38_

---

_Label `T: new setting` removed by @epage on 2021-12-08 20:38_

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 17:00_

---

_Label `D: easy` removed by @epage on 2021-12-09 17:00_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 17:00_

---

_Removed from milestone `3.1` by @epage on 2021-12-09 17:00_

---

_Unassigned @pksunkara by @pksunkara on 2023-04-13 23:48_

---

_Comment by @epage on 2024-08-10 00:31_

While not automatic, we are exposing this as [`clap_complete::dynamic::shells::CompleteCommand`](https://docs.rs/clap_complete/latest/clap_complete/dynamic/shells/enum.CompleteCommand.html).

For stabilization of this feature, see #3166

---

_Closed by @epage on 2024-08-10 00:31_

---

_Referenced in [clap-rs/clap#5947](../../clap-rs/clap/issues/5947.md) on 2025-03-13 14:18_

---
