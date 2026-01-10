---
number: 1660
title: Update to structopt 0.3 master
type: issue
state: closed
author: CreepySkeleton
labels:
  - A-derive
assignees: []
created_at: 2019-10-02T20:25:15Z
updated_at: 2020-09-22T23:10:15Z
url: https://github.com/clap-rs/clap/issues/1660
synced_at: 2026-01-10T01:27:02Z
---

# Update to structopt 0.3 master

---

_Issue opened by @CreepySkeleton on 2019-10-02 20:25_

`structopt` has released v0.3.0 a month ago. This update brings in a number of breaking changes (most noticeably `raw` attributes removal), cosmetic changes (like span-based error reporting), and a lot of minor improvements and bug fixes. Furthermore, it has been improving further over this month, working both on new features and bug fixes. Also docs have been clarified and greatly improved. 

Summarizing all of it, I propose to use `structopt` v0.3 as `clap_derive`.

---

_Comment by @pksunkara on 2019-10-11 14:54_

@CreepySkeleton Could you link the structopt breaking changes here?

---

_Comment by @TeXitoi on 2019-10-11 15:01_

https://github.com/TeXitoi/structopt/blob/master/CHANGELOG.md#breaking-changes

---

_Comment by @CreepySkeleton on 2019-10-12 08:49_

@pksunkara Also please keep in mind that `structopt` is being improved even now. Since 0.3.0 it got a number of new features and bugfixes, see [changelog](https://github.com/TeXitoi/structopt/blob/master/CHANGELOG.md). So I highly recommend to take the last version, whatever it'll happen to be at the point.

---

_Comment by @pksunkara on 2019-12-13 19:55_

@TeXitoi Is there a document about difference between structopt and clap_derive? Why are not combining the efforts?

---

_Comment by @CreepySkeleton on 2019-12-13 20:56_

> Is there a document about difference between structopt and clap_derive?

When I try to do several things at once it typically results in nothing being done :( I'm going to write such a "moving from `structopt v0.3` to `clap v3` once the transition is done.



> Why are not combining the efforts?

I'd love to!

---

_Label `C: derive macros` added by @pksunkara on 2020-02-03 09:21_

---

_Added to milestone `3.0.0-alpha.3` by @pksunkara on 2020-02-03 09:21_

---

_Renamed from "Update to structopt 0.3" to "Update to structopt 0.3 master" by @pksunkara on 2020-02-03 09:22_

---

_Comment by @pksunkara on 2020-02-06 09:30_

@CreepySkeleton We can close this, the only thing we are missing is:

https://github.com/TeXitoi/structopt/commit/dc7571c85bf8977fa9c1da654c78cda7be961da8

Also, We had decided not to use `paw`.

---

_Comment by @CreepySkeleton on 2020-02-06 09:34_

Yeah, if somebody wants paw support here - well, I'm not totally against it, but let's wait for a feedback first.

---

_Closed by @CreepySkeleton on 2020-02-06 09:34_

---

_Comment by @pksunkara on 2020-02-06 09:35_

What about the error message improvement commit I linked above?

-- 
Pavan Kumar Sunkara

On 6 February 2020 at 10:34:04 AM, CreepySkeleton (notifications@github.com)
wrote:

Closed #1660 <https://github.com/clap-rs/clap/issues/1660>.

—
You are receiving this because you commented.
Reply to this email directly, view it on GitHub
<https://github.com/clap-rs/clap/issues/1660?email_source=notifications&email_token=AABKU34HVYTNELAK7PLK3TLRBPKQXA5CNFSM4KPBV5U2YY3PNVWWK3TUL52HS4DFWZEXG43VMVCXMZLOORHG65DJMZUWGYLUNFXW5KTDN5WW2ZLOORPWSZGOWOMKIHI#event-3013125149>,
or unsubscribe
<https://github.com/notifications/unsubscribe-auth/AABKU32OWVK22PP77T25ORTRBPKQXANCNFSM4KPBV5UQ>
.


---

_Comment by @CreepySkeleton on 2020-02-06 09:37_

That's not much of improvement anyway, see diff for the `.stderr` files. I think it can wait for refactoring.

---

_Removed from milestone `3.0.0-beta.2` by @pksunkara on 2020-04-07 13:59_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-07 13:59_

---

_Comment by @Lucretiel on 2020-09-22 16:34_

Oh, why was `paw` removed? `paw` was one of my favorite feature additions to `structopt`, for basically the same reason that `structopt` one of my favorite feature additions to `clap`, since it makes argument parsing *fully* declarative.

---

_Comment by @CreepySkeleton on 2020-09-22 19:59_

`paw` is downright useless. Let's just compare it with raw clap:
```rust
#[paw::main]
fn main(opts: Opts) {
    //...
}
```
```rust
fn main() {
    let opts = Opts::parse();
    //...
}
```

Identical behavior, two lines difference per program. Is it even worth to talk about? IMO it definitely didn't worth [the fuss it caused in structopt](https://github.com/TeXitoi/structopt/issues/407).

This "fully declarative" argument is meaningless unless you specify your definition of "fully declarative". I think I can count half a dozen of people I've met who had their own definition of declarative, exclusive of course, and obviously the only correct one. I guess it's fair to say that I have come to belief that this kind of "this isn't _real_ magic!" kind of arguments is merely a synonym for "I'm out of arguments, but I still believe my opponent is obviously wrong".

---

_Comment by @Dylan-DPC-zz on 2020-09-22 20:06_

@CreepySkeleton is there a good reason to demean other repositories? this is against the Code of Conduct of Clap and the CLI-WG organisation. 

---

_Comment by @pksunkara on 2020-09-22 20:07_

@Lucretiel I am afraid that I don't understand this part.

> since it makes argument parsing fully declarative.

Can you please explain what it means and why clap is lacking it?

---

_Comment by @Lucretiel on 2020-09-22 20:20_

> Can you please explain what it means and why clap is lacking it?

The idea is that a derive macro replaces a "imperative" definition of your parameter set (via the macro or App builder methods) with a "declarative" definition (a struct definition). `paw` extends this pattern to the acquisition of these arguments by replacing the "imperative" invocation (`get_matches`) with a "declarative one" (function signature). It has the added (admittedly minor) benefit of circumventing the handling of various pre-emptive exit cases (parse failure, --help, --version), which currently have to be done either manually or with an `exit(0)` or `exit(1)` somewhere in the library.

---

_Comment by @Lucretiel on 2020-09-22 20:23_

> Is it even worth to talk about?

With respect, your [comment](https://github.com/clap-rs/clap/issues/1660#issuecomment-582815328) was explicitly inviting discussion on this topic. 

---

_Comment by @pksunkara on 2020-09-22 20:43_

Ah, understood now. I think the question of adding `paw` falls into bikeshedding category (since it changes one line in the whole code) and I personally would want more people to request it before adding support for it with a feature gate.

---

_Comment by @CreepySkeleton on 2020-09-22 23:10_

@Dylan-DPC I don't think you can classify it as a breach of CoC. I was careful to criticize code. not people; I wish the best of luck to the authors of `paw` in their endeavor. When criticizing the code, I made sure to explain why I find it useless (identical behavior, one line difference). I don't think that advocating for not using a crate is a CoC breach if you explained your reasoning. I realize I was being terse, but I don't think I was impolite: I was being terse because I don't think bikesheding is worth spending many words on. I don't see how it's a breach of CoC either. You're overreacting.

@Lucretiel Your comment reads a bit incoherent to me, but I think I can understand it in two ways:
* You realize the behavior is identical, but you see a real ideological difference between using an attribute and a function call, something akin to the difference between `Option<T>` and `Result<T, ()>`. Unlike `Option` that occurs in hundred of places in a typical program, this `paw` problem has extremely low impact: one line per program (or three, depending on how you count). Let's just agree this one is bikesheding at its finest.
* You truly believe there's a functional difference between the examples above (your last sentence hints at it). This is wrong. If you mentally expand the `paw` example, it will result in the "normal derive" example:

  ```rust
  // The struct
  #[derive(Clap)]
  struct Opts {}

  // paw invocation
  #[paw::main]
  fn main(opts: Opts) {
      // code
  }

  // and what it expands to

  impl paw::ParseArgs for Opts {
      type Error = whatever; // irrelevant, never used

      fn parse_args() -> Result<Self, Self::Error> {
          // it just directly does the call, exiting on error immediately
          // because it's what `parse` does
          Ok(Opts::parse())
      }
  }

  fn main() {
      let opts = Opts::parse_args().unwrap();
      // code
  }
  ```

As regards to my earlier comment: it was half a year ago. I used to believe that the feedback from the open source crowd is mostly sound and weighted, even when it's about bikesheding. Now I know better.

---
