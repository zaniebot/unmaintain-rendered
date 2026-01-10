---
number: 1365
title: Binary size could be smaller
type: issue
state: closed
author: smklein
labels:
  - C-enhancement
  - A-meta
  - S-waiting-on-mentor
assignees: []
created_at: 2018-10-19T21:59:13Z
updated_at: 2025-01-28T17:22:10Z
url: https://github.com/clap-rs/clap/issues/1365
synced_at: 2026-01-10T01:26:50Z
---

# Binary size could be smaller

---

_Issue opened by @smklein on 2018-10-19 21:59_

### Rust Version

rustc 1.31.0-nightly (e7f5d4805 2018-10-18) 

### Affected Version of clap

2.32.0

### Bug or Feature Request Summary

I compared the binary size of a "hello world" application, with and without clap, and came up with the following data:

Using a Cargo.toml:

```toml
  [profile.release]                                                                                                                                                                                                
  opt-level = 3                                                                                                                                                                                                    
  lto = true                                                                                                                                                                                                       
  panic ='abort'      
```

And a main.rs (of either this, or a version which only calls `println!("Hello world")`:

```
#![feature(alloc_system)]
extern crate alloc_system;
extern crate clap;                                                                                                                                                                                                                   
use clap::{App, Arg};

fn main() {
    let matches = App::new("Hello")                                                                                                                                                                              
                    .arg(Arg::with_name("foo")                                                                                                                                                                   
                         .short("f")                                                                                                                                                                             
                         .takes_value(true))                                                                                                                                                                     
                    .get_matches();                                                                                                                                                                              
    let foo = matches.value_of("foo").unwrap();                                                                                                                                                  
    println!("Hello, world: {}", foo);                                                                                                                                                                           }
```

I ran the following:

`$ cargo rustc --release -- -C link-args=-static && strip target/release/hello`

The "Non-clap" version of hello world resulted in the following output from bloaty:

```
     VM SIZE                      FILE SIZE
 --------------                --------------
  78.3%   283Ki .text            283Ki  78.3%
   8.2%  29.8Ki .rodata         29.8Ki   8.2%
   6.0%  21.7Ki .eh_frame       21.7Ki   6.0%
   3.7%  13.6Ki .data.rel.ro    13.7Ki   3.8%
   1.1%  4.16Ki .bss                 0   0.0%
   0.2%     568 [ELF Headers]   2.37Ki   0.7%
   0.6%  2.16Ki .dynsym         2.16Ki   0.6%
   0.0%       0 [Unmapped]      1.91Ki   0.5%
   0.4%  1.36Ki .dynstr         1.36Ki   0.4%
   0.3%  1.17Ki .rela.dyn       1.17Ki   0.3%
   0.2%     712 .got               712   0.2%
   0.2%     672 .rela.plt          672   0.2%
   0.2%     576 .dynamic           576   0.2%
   0.1%     534 [12 Others]        525   0.1%
   0.1%     464 .plt               464   0.1%
   0.1%     416 .gnu.hash          416   0.1%
   0.1%     320 .gnu.version_r     320   0.1%
   0.1%     272 .data              272   0.1%
   0.0%       0 .shstrtab          250   0.1%
   0.0%     184 .gnu.version       184   0.0%
   0.0%     152 .plt.got           152   0.0%
 100.0%   362Ki TOTAL            362Ki 100.0%
```

Where the clap version resulted in the following:

```
     VM SIZE                      FILE SIZE
 --------------                --------------
  82.6%   620Ki .text            620Ki  82.7%
   7.0%  52.6Ki .rodata         52.6Ki   7.0%
   5.4%  40.3Ki .eh_frame       40.3Ki   5.4%
   3.2%  23.8Ki .data.rel.ro    23.9Ki   3.2%
   0.6%  4.29Ki .bss                 0   0.0%
   0.1%     568 [ELF Headers]   2.37Ki   0.3%
   0.3%  2.27Ki .dynsym         2.27Ki   0.3%
   0.2%  1.41Ki .rela.dyn       1.41Ki   0.2%
   0.2%  1.38Ki .dynstr         1.38Ki   0.2%
   0.0%       0 [Unmapped]      1.12Ki   0.1%
   0.1%     760 .got               760   0.1%
   0.1%     576 .dynamic           576   0.1%
   0.1%     576 .rela.plt          576   0.1%
   0.1%     542 [12 Others]        501   0.1%
   0.1%     476 .gnu.hash          476   0.1%
   0.1%     400 .plt               400   0.1%
   0.0%     320 .gnu.version_r     320   0.0%
   0.0%     272 .data              272   0.0%
   0.0%       0 .shstrtab          250   0.0%
   0.0%     194 .gnu.version       194   0.0%
   0.0%     184 .plt.got           184   0.0%
 100.0%   751Ki TOTAL            750Ki 100.0%
```

(Diff for visibility)

```
     VM SIZE                    FILE SIZE
 --------------              --------------
  +119%  +336Ki .text         +336Ki  +119%
   +77% +22.8Ki .rodata      +22.8Ki   +77%
   +85% +18.6Ki .eh_frame    +18.6Ki   +85%
   +75% +10.2Ki .data.rel.ro +10.2Ki   +75%
   +20%    +240 .rela.dyn       +240   +20%
  +3.0%    +128 .bss               0  [ = ]
  +5.4%    +120 .dynsym         +120  +5.4%
   +14%     +60 .gnu.hash        +60   +14%
  +6.7%     +48 .got             +48  +6.7%
   +21%     +32 .plt.got         +32   +21%
   +24%     +32 .tbss              0  [ = ]
  +1.9%     +26 .dynstr          +26  +1.9%
  +5.4%     +10 .gnu.version     +10  +5.4%
 -28.9%     -24 [LOAD [RX]]      -24 -28.9%
 -13.8%     -64 .plt             -64 -13.8%
 -14.3%     -96 .rela.plt        -96 -14.3%
  [ = ]       0 [Unmapped]      -808 -41.2%
  +107%  +388Ki TOTAL         +388Ki  +107%
```

The amount of .text being generated here doubles the size of the binary.

Clap is really useful, and I'd love if I could use it on more constrained environments, where binary size might be expensive.

Chatting with @kbknapp , it seems that there are some changes to clap-rs underway to allow more flexible usage of the crate, potentially disabling some features at compile-time to shrink the binary, but I figured I'd file this bug to track.

Are there currently other ways of toggling clap features, to compare binary sizes for this evaluation?

---

_Referenced in [clap-rs/clap#1384](../../clap-rs/clap/issues/1384.md) on 2020-02-01 15:37_

---

_Label `C: other` added by @CreepySkeleton on 2020-02-01 15:40_

---

_Label `T: refactor` added by @CreepySkeleton on 2020-02-01 15:40_

---

_Label `T: RFC / question` added by @CreepySkeleton on 2020-02-01 15:40_

---

_Label `W: maybe` added by @CreepySkeleton on 2020-02-01 15:40_

---

_Label `T: RFC / question` removed by @pksunkara on 2020-04-01 18:18_

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 07:38_

---

_Comment by @kbknapp on 2020-05-02 01:19_

Re-energizing this topic. Not necessarily for action prior to 3.0 release, but something to keep in our mind and begin to tackle as available.

My comment from #1891 

> Along those same lines but also a separate issue entirely, at some point I would like to have the discussion about aggressive cargo feature use. Ideally, all non-essential sections of clap could be disabled (but enabled by default) to allow small binary sizes when desired. I've had quite a few people / companies approach me about clap's binary size being their only issue. In fact, I believe that is the number one factor that led Google to work on argh, was binary size is their top concern (for Fuschia) and don't need a lot of the "extra" features that clap provides.

@CreepySkeleton's comment from that same issue:

> On the other hand, people from this anti-dependency camp likely see clap as bloat-full and having too much allegedly unneeded deps irrespective of term_size. Clap already pulls 7 crates in --no-default-features mode, 22 in default-features mode, and 27 in all-features mode; one extra crate - very tiny and very useful btw - won't change their opinion. It's kind of all-or-nothing situation, 8 deps (including transitive) kind of count as "as small as possible". I really doubt any of them would become our user if we feature-gate one or two tiny crates.
>
> That's not to mention that clap itself takes as much time to build as (almost) all of the deps combined, and the compilation time is a much more common complaint among users (in my view). Along with code bloat, which would only decrease without this feature.
>
> Regarding modularization of clap.
>
> I've been thinking about it too, for some time, and I've come to conclusion that, while it's doable - to some extent, it would require full-scale refactoring. And by full-scale I mean "rewrite the parser and the validator entirely" which is quite an endeavor.

Finally, @Dylan-DPC's comment from that issue:

> Given how popular clap is, the number of dependencies is definitely a huge concern. I've been planing of tackling a lighter clap in some form or the other post the 3.0 release. That discussion is beyond the scope of this issue.

---

_Comment by @kbknapp on 2020-05-02 01:42_

Just so my thoughts are known, I don't believe we need to remove all dependencies and features for the smallest possible size. What I *do* think is that we should know which dependencies have a low "weight to functionality" ratio.

"Weight" can assessed in two manners, code size added to clap, and compile time.

"Functionality" should be self evident.

I am definitely *not* in the camp of "make sure there are as few deps as possible." However, I am in the camp of I don't want to take on a dep that doesn't provide intrinsic value, or is a maintenance burden.

The harder crates to evaluate will be ones that don't add a lot to clap functionally, but at the same time don't add much if any compile time or code size either. I would propose for those style crates we look at either pulling the code internal, or dropping the crate.

I'm most concerned with the `--no-default-features` case, that case should be as small as possible, with as few high ratio deps as possible. I'm not concerned with the default features, or even `--all-features` cases, because at that point the consumer as chosen to go a heavier weight route, while there are lighter weight options.

This would be a great first issue, as it's not a lot of code changing, and doesn't require too much in depth knowledge of clap. At first, it will primarily be measuring the default crates, or seeing how widespread their use inside clap is.

--- 

Taking a quick look at our default crates I see:

| Crate (+ transitive deps) | Functionality | Notes |
| -- | -- | -- |
| `bitflags` | Underpin of `AppSettings` | |
| `unicode-width` | Heavily used in Help Messages | If we made auto generated help optional, this could become an optional feature |
| `textwrap` | Used for Help Messages | same as `unicode-width` |
| `indexmap` (`autocfg`) | used in `ArgMatcher`/`ArgMatches`| Perhaps we could use `vec_map` instead |
| `os_str_bytes` | used in parsing | clap fundamentally messes with OsStrs or bytes heavily...so this is probably well worth the dep | 
| `vec_map` | Used throughout `Arg` and usage parser | Used to be an optional feature. Has perf gains over BTreeMap...maybe those gains need to be evaluated again to see if they're worth it |



---

_Referenced in [clap-rs/clap#1891](../../clap-rs/clap/issues/1891.md) on 2020-05-02 01:47_

---

_Comment by @kbknapp on 2020-05-02 02:03_

Doing a `--no-default-feautres --features std` build on a simple:

```
App::new("foo").get_matches();
```

Gives us this output via `cargo bloat`  in release mode:

```
cargo bloat --crates --release
[..]
    Analyzing target/release/fake

 File  .text     Size Crate
 9.3%  56.5% 285.5KiB clap
 4.8%  29.3% 148.1KiB std
 1.9%  11.7%  59.2KiB [Unknown]
 0.1%   0.5%   2.6KiB indexmap
 0.1%   0.3%   1.6KiB textwrap
 0.0%   0.1%     307B fake
 0.0%   0.0%      41B os_str_bytes
16.5% 100.0% 505.5KiB .text section size, the file size is 3.0MiB

Note: numbers above are a result of guesswork. They are not 100% correct and never will be.
```

Discounting clap and std, it looks like `textwrap` and `indexmap` need a close eye. If either has better alternatives, or is simple enough to not use/pull internal I'd say those may be on the chopping block.

Also, if we did something drastic like make auto help messages optional, that alone would cut a *huge* portion of the code along with size. Additionally and oddly enough, that may the easiest thing to modularize first.

"But wait!" I can hear you say. "Auto help generation is like THE thing clap is used for, right?" Sure. It's *one* of the things, but I'd argue not the most important at all. In many of the contexts where code size matters the most, help generation is  one the things they don't care about because it's super easy to just write a help string manually for a small CLI.

---

_Comment by @CreepySkeleton on 2020-05-02 02:07_

Before we more any further, would you please test it in release mode 😄? Nobody cares about debug binaries, release the  ones of concern here.


---

_Comment by @kbknapp on 2020-05-02 02:08_

Literally just edited the comment with that info :laughing: 

---

_Comment by @pksunkara on 2020-05-02 06:27_

Did you enable `lto`? I think we should assume with that.

---

_Comment by @CreepySkeleton on 2020-05-02 13:47_

main.rs
```
use clap::*;

fn main() {
    App::new("app").get_matches();
}
```

With lto enabled (release build, no default features)
```
CreepySkeleton@builder:~/workspace/probe$ cargo bloat --crates --release
    [...]

 File  .text     Size Crate
19.4%  60.3% 277.7KiB clap
 9.0%  27.9% 128.5KiB std
 3.0%   9.2%  42.3KiB [Unknown]
 0.3%   1.0%   4.7KiB probe
 0.2%   0.6%   2.5KiB indexmap
 0.0%   0.1%     394B textwrap
32.2% 100.0% 460.3KiB .text section size, the file size is 1.4MiB
```

What interesting here is that the size of the `main.rs` crate increased from `307B` to `4.7KiB`. Basically, some code from std has been inlined and this is the biggest difference.

The overall difference is less than 10Kb (clap and its deps only) and it will thin as more features are actually used. All the further runs will be performed without LTO. 

See also https://stackoverflow.com/a/52297790. TL;DR: LTO is not about reducing code size. `z` opt level is about it.

Another interesting read: https://internals.rust-lang.org/t/rust-staticlibs-and-optimizing-for-size/5746. No TL;DR here, go read, it's worthy.

___

Kevin, just to make sure we're on the same page: I wasn't counting you as a no-deps guy, I was just thinking aloud "how would we sell clap to those guys" and I'm pretty sure the answer is - we wouldn't. 

If their wariness comes from concern that the deps affect binary size, we can righteously tell them that all the deps put together (in `--no-default-features` mode) take less than 5KiB, the next rustc release will likely change much more of that. The same can be said about compilation time: ~2sec difference, essentially a rounding error.

At the very least, if 5KiB do mater to you, you're working on embedded, and clap will likely never suit you. Take a look at getopts/argh or write it yourself.

I agree regarding help messages. It also drags error messages along.

What not clear to me is your logic about "either pulling the code internal, or dropping the crate". It sounds like "Let's copy-paste code from third party crates (we can't just drop them) in order to decrease our maintenance burden". But from that point on, we will be the ones who maintain this copy-pasted code. A contradiction. Could you please elaborate on that?



---

_Comment by @pksunkara on 2020-05-02 15:27_

> At the very least, if 5KiB do mater to you, you're working on embedded, and clap will likely never suit you. Take a look at getopts/argh or write it yourself.

I don't think we should be saying that. We should be using cargo feature flags to make sure all the fields can use clap.

---

_Comment by @TeXitoi on 2020-05-02 15:43_

To make it work on embedded, you'll need do be alloc free (usable wIthout, String, Vec and such kind of collection).

---

_Comment by @Dylan-DPC-zz on 2020-05-02 15:57_

There's a post 3.0 plan for that already which is why added the `no_std` feature

---

_Comment by @CreepySkeleton on 2020-05-02 16:44_

To clarify my statement about embedded:
* On something like Rabspery PI clap will fly marvelously; you won't even need no-std there. It's close to a real PC after all.
* If you have a chip with megabytes of ROM on board, you _may_ be able to squeeze clap in. `no_std`, `alloc`, and accurate fine tuning (see modularization) will be required.
* 128 KiB is all you've got? Too bad! Take a look at getopts (and you probably won't be able to afford even _that_).

To summarize all of that, you either have an extra hundred of KiBs at your disposal (and this is why I think we shouldn't optimize every single KiB) or clap is not for you.

And a bit of personal opinion: as someone with a (very limited) experience with embedded, I can say that allocations are almost tabu in there. Given that clap relies on `Vec` and stuff very heavily... I won't even try it if I need a CLI parser for embed. I'll go grab getopts.

---

_Comment by @TeXitoi on 2020-05-02 17:04_

Even getopts requires `Vec` and `String`. I personally don't see why I would need clap in embedded, that's a very niche usecase.

---

_Comment by @TeXitoi on 2020-05-02 17:05_

(embedded = bare metal on microcontroller)

---

_Comment by @kbknapp on 2020-05-02 23:36_

A lot to reply to here :) I don't think we need to be concerned with embedded (bare metal) right now. Maybe one day, but there are many things we can do that will improve clap before we worry about embedded.

I'm more concerned with embedded *like*, i.e. resource constrained devices, like small arm based (Pi, SBC, phone, etc.). I spoke with some Google employees who were looking at clap for that exact use case (presumably Fuschia), and binary size was *their* concern. Binary size isn't everyone's concern. To be clear, I'm not trying to supplant `argh`, but I do think we should be cognizant of that use case while building clap.

We're in a little bit of a catch-22 scenario. Many people assume argument parsing is such simple task that the deps should be ultra small and no transitive deps. That's understandable why one would think that. However, as all of us know, it turns out argument parsing is somewhat of a hidden beast. There are edge cases EVERYWHERE, and a near endless amount of features to implement.

It turns out argument parsing is complex.

Having said that, the issue we're discussing conflates a few concerns that we should probably be more explicit about:

* Binary size of clap
* Compile times of clap
* Adoption and Perception of clap

Both binary size and compile times are affected by:

  * Deps
  * clap features and code structure
  * Rust and LLVM compile time features (LTO, dead code elimination, monomorphization, etc.)

While adoption and perception can be heavily influenced by binary size, compile times, and the dep graph as it's own construct.

So long as we're doing our due diligence when it comes to crates we take on as dependencies to ensure they're worth the price we pay in terms of binary size and compile times, we're good. Alternatively, if they negatively affect those areas, we should do our best to ensure they're optional.

Meanwhile, we should be thinking about the dep graph as it's own construct because right, wrong, or indifferent people *do* care about how many crates they pull in. Unfortunately, everyone's reason for caring can be different, for some it's binary size, for some it's compile times, for others it's the perception of risk (what happens if a crates has a security flaw, or goes away over night?). 

We're addressing the first two concerns already, but the third (perception of risk) is primarily addressed by either using ultra popular, well supported crates, or crates related to/owned by the parent (which is usually signalled by the name, i.e. `clap_*` for us).

---

> "how would we sell clap to those guys" and I'm pretty sure the answer is - we wouldn't.

You're correct in that part of the answer is, "we don't." But I think we mean different things by this. I mean we "We don't try to sell them on clap, we just do the best we can to care about that scenario." 

We can do things like making clap modular enough, or structured in a way to turn off features the end user doesn't care about. This has an added benefit of potentially being able to make most of our deps optional.

And where they can't be option, oh well. At least we're *trying*. clap isn't going to fit every situation and that's fine. But we should at least do what we can to be an option for even those resource constrained situations.

---

> If their wariness comes from concern that the deps affect binary size, we can righteously tell them that all the deps put together (in --no-default-features mode) take less than 5KiB, the next rustc release will likely change much more of that. The same can be said about compilation time: ~2sec difference, essentially a rounding error.

I totally agree with you. However, the problem is many times we don't get the chance to explain ourselves. They see the dep graph front-and-center and many times simply make assumptions from there. That's the part about the "no dep" crowd I'm not a fan of. I can't count the number of reddit comments, or blog posts I've seen that say, "Can you believe [program X] pulled in 170 crates?! That's absurd!!!" with no backing evidence as to *why* that's absurd. Were those 170 crates not required? How much compile time did they add, etc.? If the compile times were the EXACT same, but [program X] just copy/pasted the code from those crates, you probably wouldn't hear a peep. In fact, you'd probably hear how amazing it was that [program X] has no dependencies! 

...I'm not a fan of that. But it's the world we live in.

---

> What not clear to me is your logic about "either pulling the code internal, or dropping the crate". It sounds like "Let's copy-paste code from third party crates (we can't just drop them) in order to decrease our maintenance burden"

I should have been more clear :stuck_out_tongue_winking_eye: Decreasing our maint burden and decreasing our dep graph are separate concerns that get intertwined. Also taking the code internal is only in the case where we're using a dep for a small well defined  subset of what the crate offers, and we aren't concerned with maint burden part, just the raw number of deps part. This should only be a last resort for us.

This is one of those things that's counter intuitive; copying and pasting a subset of a crate into clap directly, (or even into a new sub crate like `clap_foo` may actually not make *any* difference in compile times or binary size. But it will still make many people "feel" better about using clap.

Is it dumb. Yep. But again, this should be a last resort for us and not something I'm actually looking at doing seriously. The only crate that comes to mind for this type of thing is `term_size` which is technically already a clap owned crate, but again the name doesn't signal that so when people compile clap they just assume it's some rando crate we pulled in as a dep. Am I suggesting re-naming `term_size` to `clap_foo`? Nope. Not unless we were in drastic territory and have exhausted all other options. We're nowhere near that! :smile:

---

_Referenced in [clap-rs/clap#952](../../clap-rs/clap/issues/952.md) on 2020-05-06 16:10_

---

_Comment by @pksunkara on 2020-05-12 13:56_

There are some good arguments regarding inclusion of deps in this reddit thread today, https://www.reddit.com/r/rust/comments/gi7v2v/is_it_wrong_of_me_to_think_that_rust_crates_have/

---

_Comment by @BurntSushi on 2020-05-12 15:18_

@kbknapp [invited me to participate in this issue](https://github.com/clap-rs/clap/commit/739e7048a50d14023ef9ec20cba3cd844f6d4748#r39117357), but I have to say, from reading the comments, it seems like there's an undercurrent of counter-productive "us vs them." If I'm coming from (your perspective) the "anti-dependency" camp and you already believe that you can't sell clap to me

> Kevin, just to make sure we're on the same page: I wasn't counting you as a no-deps guy, I was just thinking aloud "how would we sell clap to those guys" and I'm pretty sure the answer is - we wouldn't.

then is it really worth my time to participate here if I'm treated as a lost cause in the first place?

reddit and low effort comments are a pit, and people love to make sweeping generalizations that makes the world look more extreme than it is. In reality, I think most people exercise good judgment and try to straddle this line as best as they can. Sometimes I'm on the "anti-dependency" side of the fence and [take steps to reduce my dependency tree](https://github.com/rust-lang/regex/pull/613). But other time, I'm on the "yeah dependencies are great" side and [try to resist the urge to remove dependencies](https://github.com/BurntSushi/bstr/issues/53).

I think @kbknapp has the right attitude here. The _struggle_ is the important part. Doing due diligence on dependencies before bringing them in, and really weighing their costs and benefits, is what's important. And to me, that's what I see myself doing when I [notice that a new dependency is about to come into my tree](https://github.com/clap-rs/clap/commit/739e7048a50d14023ef9ec20cba3cd844f6d4748#r39115688). What I see is a prior state in which I could opt out, but now I can't, and I don't know why. The code that was deleted looked pretty small, its commit history suggests it was very lightly edited and its performance was more than good enough for me. Moreover, when I [first dropped the `vec-map` dependency from my use of Clap](https://github.com/BurntSushi/ripgrep/commit/c8e755f11f31b6da04329cdc7433747bba70150f), I noticed some really crazy compile time improvements. I suspect there was a bug somewhere or some weird case, but still, the flexibility to drop dependencies was really nice.

I'm not familiar with Clap internals, and as someone who has written more than one argument parsing library, I am definitely someone who appreciates their complexity. I never take a good argument parser for granted. They are super hard to build because they need to balance so many competing concerns. IMO, Clap does a great job of this already.

So with that said, I'll give some opinions on Clap 3's required dependencies:

* `bitflags` - I would personally just hand write any bit flags logic that I needed. In my view, this is a crate that optimizes for writing code, which I generally think isn't the right thing to be doing in widely used fundamental crates. So there's a bit of a philosophical opinion involved here.
* `textwrap` - I would make this optional but enabled by default. I write all of my help strings and documentation wrapped to 79 columns anyway, which is probably good enough.
* `unicode-width` - I would make this optional but enabled by default. If my help text is all ASCII, then an assumption that each byte is one column is reasonable.
* `indexmap` - I don't have enough context to know what to do here, but it looks like it was recently added. I _assume_ this is justified somehow through a major performance win, but otherwise, I would drop this.
* `os_str_bytes` - This is definitely better than using unsafe code. If I knew more about why Clap needed this, then perhaps there is a way to get rid of this too. But yeah, if Clap needs to be mucking around with OsStrs, then the lack of an `OsStr` string API probably makes this really hard to the point where you pretty much have to do what `os_str_bytes` is doing. (I still wish std would just expose its internal WTF-8 representation, but there are good arguments against doing that.)
* `vec_map` - I don't know the performance story here, but keeping it optional looked easy? But maybe there are some issues I don't know about.

---

_Comment by @kbknapp on 2020-05-12 15:25_

Thank you for the detailed response! Before I can finish reading the comment and add any thoughts, let me address something real quick as a moderation note:

> from reading the comments, it seems like there's an undercurrent of counter-productive "us vs them." If I'm coming from (your perspective) the "anti-dependency" camp and you already believe that you can't sell clap to me

I want to be very clear with everyone reading these messages (especially including the clap team) that this is *not* what I want to portray or the tone I want to encourage. Internet communications via text are hard, especially when we start considering native languages and such. (Edit: to be even more clear. I don't think @BurntSushi is portraying this tone. He's reading and noticing this tone from previous comments *we* made. And I should say thank you for pointing this out! :smile: )

I just want to be clear that the clap team is on the same page of, there is no "us vs them" and all opinions are welcome! We have differences of priorities and how to achieve those priorities, but all opinions are valid within this discussion. Ultimately the clap team will have to make a decision on which priority and implementation strategy to go with, but through discussions and issues like these my goal is to take all opinions in, consider them carefully, and make a decision that is best for clap and all consumers (current and future).

--- 

...now back to reading :smile: 

---

_Comment by @BurntSushi on 2020-05-12 15:35_

Also, one other note: IIRC, `cargo bloat` has an option to list binary size by function. That might also be useful to look at for decreasing binary size. Sometimes you can get big wins by tweaking the inlining strategy used, for example. Or using trait objects is sometimes possible, although I've found it difficult to do in practice in many cases because of object safety rules.

---

_Comment by @pksunkara on 2020-05-12 15:44_

> I just want to be clear that the clap team is on the same page of, there is no "us vs them" and all opinions are welcome!

I strongly agree with this.

Unfortunately, I only started using Rust this year (maybe Dec last year), so I missed all the deps related discussions and am still trying to catch up on understanding the pros and cons related to rust ecosystem. That is why I was asking you the recent question on deps in termcolor @BurntSushi.

In Javascript before, while I do try to reduce our usage of deps, I still try to prefer commonly used packages and using them as deps instead of writing all of them myself which was the convention all across the ecosystem.

If we take the case of `vec_map`, my question is why do we want to make it optional? Shouldn't it be either including it completely or removing it completely? I think this is basic question we want to answer first.

---

_Comment by @kbknapp on 2020-05-12 16:28_

> If we take the case of vec_map, my question is why do we want to make it optional? Shouldn't it be either including it completely or removing it completely? I think this is basic question we want to answer first.

My understanding is there are several concerns that can also overlap with each other, and not everyone has the same concerns. As mentioned above, some of those include:

* Binary Size
* Compile Time
* Transitive Deps (which repeats points 1-2 for each dep)

Adding a dep can have negative impacts on points 1 and 2, or *both*. Some people care about point 1, some people about point 2, and some about both. So if we consider *either* point a deps "cost" we have to weigh that cost against it's benefits (usually performance, or lower maintenance burden for us directly).

This is what I was trying to say earlier, we have to weigh *any* cost (including the subjective ones) against the objective benefit.

The reason for making it optional and not a binary include or not include is because people have various requirements. Some may not care about the binary size, or increased compile time if the feature provided is worth the cost, but since some do care we need a way to allow our consumers the same choices we're making at the library level.

For `vec_map` which is a self contained crate and no transitive deps, I would want to look at exactly what perf gains we get, compared with binary size/compile times. Add to that, since we pulled in `IndexMap`, perhaps we can use that instead and remove one or the other entirely? I haven't looked lately, so that may not be feasible.

---

To address the comments per dep from @BurntSushi :

 * `bitflags` - I hadn't really considered just pulling in the logic of bitflags to be honest. ...but now that you say it I'm sure it's actually simple subset since we don't particularly care about the macros, etc. just the functionality. Would probably worth a look at least.
  * `textwrap` - the hard part with this one is this dep helps us wrap at word boundaries intelligently. But perhaps making it optional for those that will manually wrap their help strings anyways.
   * `unicode-width` - I also like the idea of providing a, "I only use ASCII args/help strings (values excluded), so turn off all extra deps related to handling Unicode logic" flag
   * `indexmap` - Might be able to combine with vec_map, or relook at our exact use case. I need to familiarize myself with the code again.
   * `os_str_bytes` - This is probably staying for the time being. Clap is pretty much written with idea we use OsStr(ing) internally everywhere, and only assume/use UTF-8 at the consumer demarcation point. I've toyed with the idea of `bstr` since all we really care about is bytes, but not done enough research to see what overlap, if any, `bstr` and `os_str_bytes` have or which would be the best fit.
   * `vec_map` - Addressed in other comments. I just want to make sure we're getting a good cost:benefit ratio otherwise we may drop it entirely.


---

_Comment by @BurntSushi on 2020-05-12 17:49_

> I've toyed with the idea of bstr since all we really care about is bytes, but not done enough research to see what overlap, if any, bstr and os_str_bytes have or which would be the best fit.

You _probably_ want to stick with `os_str_bytes`. `os_str_bytes` re-implements WTF-8 and gives you 100% correctness on both Unix and Windows. The only help `bstr` gives you with OS strings are utility functions for converting between `&OsStr` and `&[u8]`, which is zero cost on Unix but does lossy decoding on Windows, so it sacrifices some correctness. (`bstr` does not re-implement WTF-8.)

I don't know how hard it would be to avoid doing the equivalent of `os_str_bytes` while maintaining correctness. It could be impossible. I could imagine some possibilities, like lossily decoding each argument for parsing only, which would work since the "significant" characters in the arg parser I believe are all ASCII. But then making sure you hand back the original `OsString` to the caller. But this is complex and could potentially be a performance regression. Unless std decides to change its tune and expose an OS string's encoding, `os_str_bytes` is probably the right answer.

---

_Comment by @pksunkara on 2020-05-12 17:51_

I think using `indexmap` in place of `vec_map` would make the performance worse. And we need `indexmap` for ordered map IIRC.

There was a PR in the v2 commit history which removed `bitflags` but then you reverted it back @kbknapp. Not sure what happened but it was around 2 yrs ago.

---

_Comment by @kbknapp on 2020-05-12 17:57_

> I think using `indexmap` in place of `vec_map` would make the performance worse. And we need `indexmap` for ordered map IIRC.

That's entirely possible. I haven't done any investigating on the matter yet.


> There was a PR in the v2 commit history which removed `bitflags` but then you reverted it back @kbknapp. Not sure what happened but it was around 2 yrs ago.

That was switching `bitflags` for a `HashSet` which had a significant negative performance impact which was the reasons for the revert.

---

_Comment by @CreepySkeleton on 2020-05-13 07:50_

> Also, one other note: IIRC, cargo bloat has an option to list binary size by function. That might also be useful to look at for decreasing binary size. Sometimes you can get big wins by tweaking the inlining strategy used, for example. Or using trait objects is sometimes possible, although I've found it difficult to do in practice in many cases because of object safety rules.

I actually looked at the list long time ago and found that 
* there are about 5 big (10KiB - 33KiB) functions, they hold ~90KiB of clap. Maybe we can shrink them a bit, but I wouldn't expect any breakthrough here.
* there are *a few dozens* of pretty small but non-negligible functions (500B - 5KiB) that are responsible for the most of the bloat. This is where I'd expect we can get a win.
* clap doesn't use generics internally at all. All (or most of) the generics are placed on public builder's functions and end up inlined.

---

> * textwrap - I would make this optional but enabled by default. I write all of my help strings and documentation wrapped to 79 columns anyway, which is probably good enough.
> * unicode-width - I would make this optional but enabled by default. If my help text is all ASCII, then an assumption that each byte is one column is reasonable.

Agreed.

> * bitflags - I would personally just hand write any bit flags logic that I needed. In my view, this is a crate that optimizes for writing code, which I generally think isn't the right thing to be doing in widely used fundamental crates. So there's a bit of a philosophical opinion involved here.

Counterpoints:
* If you're suggesting to use a set of raw `u64` constants, like
  ```rust
  const REQUIRED:     u64 = 1;
  const MULTIPLE_OCC: u64 = 1 << 1;
  const EMPTY_VALS:   u64 = 1 << 2 | TAKES_VAL;
  const GLOBAL:       u64 = 1 << 3;
  const HIDDEN:       u64 = 1 << 4;
  const TAKES_VAL:    u64 = 1 << 5;
  const USE_DELIM:    u64 = 1 << 6;
  ```

  Then there's a very significant downside to it: you can accidentally OR it with something that is not a bitflag, especially if names are similar and the types happen to match. 

  `bitflags` prevents this by creating a separate type for it that is incompatible with everything else. Of course, you could create a separate type yourself, by hand, but then again, you'll be left to implementing `OR, And, Not...` by hand - and you better make sure you aren't making more errors when you'll be copy-pasting the impls!
* This crate is de-facto standard for bitflags in rust world, judging by download count.

I don't really have _very_ serious objections to importing the macro into clap codbase, but I'd rather avoid it nonetheless ("reinventing the wheel" argument).

As an argument for importing the macro, I'd say that `bitflags` looks mostly unmaintained at this point (four months without updates and a couple of unanswered PRs).

> * indexmap - I don't have enough context to know what to do here, but it looks like it was recently added. I assume this is justified somehow through a major performance win, but otherwise, I would drop this.

This is justified by the need of a specific data structure, and `IndexMap` satisfies it. We need:
* A key-value-like representation with `O(1)` lookup. Probably even `O(log(n))` would be fine, number of arguments (`n`) rarely exceeds a hundred or so. Both `HashMap` and `BTreeMap` could be used here. 
* Iteration order must match the insertion order. Neither `HashMap` nor `BtreeMap` fit.

Yes, we could use a `HashMap` coupled with a `Vec` to trace the insertions, but why reimplement the wheel?

> * os_str_bytes - This is definitely better than using unsafe code. If I knew more about why Clap needed this, then perhaps there is a way to get rid of this too. But yeah, if Clap needs to be mucking around with OsStrs, then the lack of an OsStr string API probably makes this really hard to the point where you pretty much have to do what os_str_bytes is doing. (I still wish std would just expose its internal WTF-8 representation, but there are good arguments against doing that.)

Well, either std decides to expose the `wtf-8` representation - which is very unlikely - or we do what `os_str_bytes` does simply because we _need_ these methods. And I'd rather use one, well-audited implementation (`os_str_bytes` appears to be one) instead of spawning our own unicorn impl with lots of unsafe.

This is core functionality. It breaks, we break. It shines, we shine, too.

`bstr` is great, but not quite what we need. 

> * vec_map - I don't know the performance story here, but keeping it optional looked easy? But maybe there are some issues I don't know about.
> * vec_map - Addressed in other comments. I just want to make sure we're getting a good cost:benefit ratio otherwise we may drop it entirely.

90% sure we can simply replace it with `BTreeMap` or even plain `Vec`. I'll be looking into it. 

---

_Referenced in [uutils/coreutils#1516](../../uutils/coreutils/pulls/1516.md) on 2020-05-23 08:16_

---

_Referenced in [clap-rs/clap#1951](../../clap-rs/clap/pulls/1951.md) on 2020-05-30 17:29_

---

_Comment by @bb010g on 2020-06-12 07:47_

@kbknapp
> "But wait!" I can hear you say. "Auto help generation is like THE thing clap is used for, right?" Sure. It's _one_ of the things, but I'd argue not the most important at all. In many of the contexts where code size matters the most, help generation is one the things they don't care about because it's super easy to just write a help string manually for a small CLI.

Would also exposing static help generation via a build script and/or macro be feasible?

---

_Comment by @ssokolow on 2020-06-12 10:57_

> "But wait!" I can hear you say. "Auto help generation is like THE thing clap is used for, right?" Sure. It's one of the things, but I'd argue not the most important at all. In many of the contexts where code size matters the most, help generation is one the things they don't care about because it's super easy to just write a help string manually for a small CLI.

I'd disagree with that perception. For writing "strongly-typed scripts" that are meant to be tiny, I'd be willing to sacrifice functionality if the gains are proportionate to what's lost, and there do even exist argument parsers designed to be tiny that do `--help` generation in about 1/20th the size of clap. (pico-args has a [comparison chart](https://github.com/RazrFalcon/pico-args#alternatives) and, of them, gumdrop and argh will do `--help` generation.) 

...the deal-breaker is that, last I checked, none of the tiny ones that do `--help` generation support taking `OsString` arguments, and that's non-negotiable. (I actually have some mojibake'd filenames on my drives right now.)

That said, I do consider `--help` generation non-negotiable, because I'm big on DRY in my code and, if the parser didn't have `--help` generation, I'd have to write a procedural macro or some other form of compile-time metaprogramming wheel-reinvention to ensure the `--help` and the parser can't drift out of sync.

---

_Comment by @therealprof on 2020-07-02 13:28_

Just randomly stumbled across this ticket and I do find one argument rather weird in this discussion which is "performance": How is that even a topic worth making size tradeoffs for?

In the end, a command line parser (typically) runs once per application invocation while bloat is evident even when the application is not used at all.

For this kind of crate size should always have a way higher priority than runtime performance.

---

_Comment by @BurntSushi on 2020-07-02 13:32_

@therealprof No, performance matters, a lot. This is a problem `docopt` had (and probably still has), where in certain configurations, its performance could be quite bad because of how its matching algorithm worked.

The main reason why performance matters for CLI parsers is when a lot of arguments are passed to the command, or when the command is repeatedly executed. This can happen pretty easily with things like `xargs`.

---

_Comment by @therealprof on 2020-07-02 13:48_

> The main reason why performance matters for CLI parsers is when a lot of arguments are passed to the command, or when the command is repeatedly executed. This can happen pretty easily with things like `xargs`.

Sure, I wasn't implying that abysmal performance was acceptable. But you don't need superfancy order-preserving hashmaps to get perfectly fine CLI parsing speeds for 99+% of all applications, a regular `Vec` would do just fine. And for the rest we can always have a separate opt-in performance option which uses highly optimized external crates to cater for crazy fast 2 MB CLI arguments parsing needs.

---

_Comment by @BurntSushi on 2020-07-02 13:55_

I see, I didn't realize you were talking at that level of detail. I think at that point, you need a benchmark. I do share your skepticism on that specific point, but would definitely want to subject it to testing.

---

_Comment by @lopopolo on 2020-08-26 15:59_

> > * indexmap - I don't have enough context to know what to do here, but it looks like it was recently added. I assume this is justified somehow through a major performance win, but otherwise, I would drop this.
> 
> This is justified by the need of a specific data structure, and `IndexMap` satisfies it. We need:
> 
> * A key-value-like representation with `O(1)` lookup. Probably even `O(log(n))` would be fine, number of arguments (`n`) rarely exceeds a hundred or so. Both `HashMap` and `BTreeMap` could be used here.
> * Iteration order must match the insertion order. Neither `HashMap` nor `BtreeMap` fit.
> 
> Yes, we could use a `HashMap` coupled with a `Vec` to trace the insertions, but why reimplement the wheel?

A potential reason to "reimplement the wheel" is that `indexmap` depends on `hashbrown` which is duplicative of the `HashMap` in `std`.

---

_Comment by @CreepySkeleton on 2020-08-26 16:11_

> A potential reason to "reimplement the wheel" is that indexmap depends on hashbrown which is duplicative of the HashMap in std.

_This_ is a good point, thank you. I will get rid of it, but it won't shrink the size very much (`30 Kb` at best). 

Incidentally, I've discovered the true reason clap looks so bloaty. I'll follow up with an elaborative commentary a bit later, but for now, spoiler: how many of clap's features you think you really use?

---

_Comment by @Byron on 2020-09-01 02:35_

To me 30kb (despite at best), look like a significant decrease given that clap usually adds 300kb to the binary. Thus I would certainly be looking forward to this. Also I wonder how that would affect compile times - maybe code size does not linearly correspond to compile times, so 'these' 30kb might take longer to produce than 'other' 30kb elsewhere.

In any case I will be looking forward to the more elaborate version, it will be interesting to see where all the perceived 'bloat' is coming from exactly.

---

_Comment by @lnicola on 2020-10-25 19:39_

@CreepySkeleton soft ping for that comment about the binary size :-).

---

_Comment by @mgeisler on 2020-12-03 14:47_

Hi all!

> * `textwrap` - the hard part with this one is this dep helps us wrap at word boundaries intelligently. But perhaps making it optional for those that will manually wrap their help strings anyways.
> * `unicode-width` - I also like the idea of providing a, "I only use ASCII args/help strings (values excluded), so turn off all extra deps related to handling Unicode logic" flag

I'll be happy to make Unicode handling an optional feature in `textwrap` if there is a desire for that?

I've just [merged a PR to `textwrap`](https://github.com/mgeisler/textwrap/pull/234) which makes it handle text in narrow columns better. That is, it will now attempt to minimize the "raggedness" of the wrapped lines and give you better output out of the box. Pleaes see the PR for an example. The feature comes with dependency on a new crate of mine... If that's a show-stopper, then please let me know!

---

_Referenced in [mgeisler/textwrap#257](../../mgeisler/textwrap/pulls/257.md) on 2021-01-01 17:55_

---

_Comment by @mgeisler on 2021-01-01 17:57_

I just wanted to say that I've released a version of textwrap where all dependencies are optional :tada: This means that both unicode-width and smawk can be disabled at compile time: https://github.com/mgeisler/textwrap/releases/tag/0.13.2

---

_Referenced in [clap-rs/clap#2284](../../clap-rs/clap/pulls/2284.md) on 2021-01-02 05:31_

---

_Referenced in [vectordotdev/vector#6603](../../vectordotdev/vector/pulls/6603.md) on 2021-03-03 02:01_

---

_Referenced in [clap-rs/clap#2808](../../clap-rs/clap/issues/2808.md) on 2021-11-04 16:30_

---

_Comment by @epage on 2021-11-04 16:47_

Some updates on this
- We dropped `vec_map`, most of the uses of it were treating it like a regular `Vec`, see https://github.com/clap-rs/clap/pull/2774
- Over at https://github.com/clap-rs/clap/issues/2808, I have a [proposal](https://github.com/clap-rs/clap/issues/2808#issuecomment-961211612) for dropping `IndexMap` by baking in `DeriveDisplayOrder` while still providing ways for getting sorted order
- `text-wrap/unicode-width` and the new `unicase` dependency are on by default and https://github.com/clap-rs/clap/discussions/2839 explores turning them off by default.  It sounds like we shouldn't but thought I'd still share that development
- Clap's current design is closed ended, all policy is baked directly in, making it harder for an optimizer to get rid of code.  In https://github.com/clap-rs/clap/discussions/2832 we have started the conversation on making clap's design more open ended.  https://github.com/clap-rs/clap/issues/2683 is the most fleshed out but least relevant to this thread but what if all of our special `requires` rules and `default_value` rules were implemented as objects that can be passed into clap?  At that point, its a pretty simple dead code elimination optimization done by the linker.  We wouldn't need granular feature flags through the guts of clap to get rid of code.

---

_Referenced in [clap-rs/clap#3008](../../clap-rs/clap/issues/3008.md) on 2021-11-09 14:38_

---

_Referenced in [epage/clapng#107](../../epage/clapng/issues/107.md) on 2021-12-06 17:37_

---

_Referenced in [epage/clapng#210](../../epage/clapng/issues/210.md) on 2021-12-06 22:15_

---

_Label `C: other` removed by @epage on 2021-12-08 20:20_

---

_Label `A-meta` added by @epage on 2021-12-08 20:20_

---

_Label `C-enhancement` added by @epage on 2021-12-08 20:39_

---

_Label `T: refactor` removed by @epage on 2021-12-08 20:39_

---

_Label `S-waiting-on-decision` removed by @epage on 2021-12-09 19:27_

---

_Label `S-waiting-on-mentor` added by @epage on 2021-12-09 19:27_

---

_Referenced in [clap-rs/clap#3267](../../clap-rs/clap/issues/3267.md) on 2022-01-07 14:24_

---

_Referenced in [clap-rs/clap#3378](../../clap-rs/clap/pulls/3378.md) on 2022-02-01 16:41_

---

_Comment by @epage on 2022-02-01 17:25_

To re-cap, strategies for reducing code size
- Look into dropping `indexmap`
- Move more of clap's design to be open-ended where policy gets plugged in at compile time or runtime, rather than all the policy being baked in and just tweaking runtime nobs
  - Theory: compiler can't get rid of a lot of dead code because the decision for what is dead goes through too many layers
  - Validation: https://github.com/clap-rs/clap/issues/2683 and https://github.com/clap-rs/clap/issues/3008
  - Allow code-genned help and usage generation: https://github.com/clap-rs/clap/issues/2914
  - Allow disabling help, usage, validation, etc:
- Reduce type sizes on the stack, especially in return positions
  - I had a theory about this but https://github.com/clap-rs/clap/pull/3378/commits/7774a8ee8a653415b81d3ed794db9f15a3367016 helped confirm it
  - All of our types are large, requiring a lot of instructions for clone, move, spilling from registers to stack for returning, etc
  - `Error` still has data public, so we can't completely shrink it yet
- I also wonder if we'd get compiler optimizations from moving error rendering from the helper functions (at "throw" site) to one single function at report-size.  This would also help us dog food our API for users providing custom error formatting (https://github.com/clap-rs/clap/issues/2628).
  - When users do provide custom error formatting, this means our error formatting might be able to be compiled out

---

_Comment by @mgeisler on 2022-02-05 23:45_

Hi all,

I got curious about the size of textwrap due to the discussion here. So I put together a quick-and-dirty wrapping algorithm and created a trivial program which wraps text given on the command line: https://github.com/mgeisler/textwrap/tree/master/examples/binary-sizes

The simply wrapping function only works on the happy-path: all ASCII, all words are short enough to fit on a line, there is a single `' '` between words, etc. I compared the size of the simple function with textwrap and its various Cargo features:

| Configuration                            |  Binary Size |    Delta |
| :---                                     |         ---: |     ---: |
| quick-and-dirty implementation           |       252 KB |     — KB |
| textwrap without default features        |       268 KB |    16 KB |
| textwrap with smawk                      |       284 KB |    32 KB |
| textwrap with unicode-width              |       276 KB |    24 KB |
| textwrap with unicode-linebreak          |       362 KB |   110 KB |

I hope these numbers help bring some data to the table. The features are described [here](https://docs.rs/textwrap/latest/textwrap/#cargo-features) in case you're curious.

 I would love to hear from you if you have ideas for shrinking textwrap further :-)

---

_Comment by @epage on 2022-02-06 00:08_

Thanks for doing this!

If these are raw binary sizes then I'm assuming they include debug information which explains why they are so large.  Running `size` on the binaries will show the size of the different sections, including `.text` and the various debug sections.

To me, it doesn't look like we saw enough gains to justify having two implementations, one for ascii and one for unicode.

That said, I think this is further showing the importance of a modularization effort.  The cost for wrapping is being paid by everyone but is only needed for automatic, runtime help generation.

---

_Comment by @mgeisler on 2022-02-06 00:27_

> If these are raw binary sizes then I'm assuming they include debug information which explains why they are so large. Running `size` on the binaries will show the size of the different sections, including `.text` and the various debug sections.

Nope, it's the size of the binary compiled with `--release` and after stripping. Furthermore, I use a [Cargo profile](https://github.com/mgeisler/textwrap/blob/master/examples/binary-sizes/Cargo.toml#L11-L13) which I believe is optimized for letting the compiler inline as much as possible:

```
[profile.release]
lto = true
codegen-units = 1
```

This is from https://github.com/johnthagen/min-sized-rust. I did not use `opt-level = "z"` since I wanted to measure the size of a typical program optimized for speed instead of size (when testing with `opt-level = "z"` I actually didn't see any improvements in the delta).

> That said, I think this is further showing the importance of a modularization effort. The cost for wrapping is being paid by everyone but is only needed for automatic, runtime help generation.

Yeah, though for me, having a `--help` that looks good on any terminal is an important part of a program. Unless you target an embedded platform with strict size limitations, I think a well-functioning command line program should be able to handle `--help` nicely.

As an example, `cargo --help` looks weird on my current terminal. This is the output (slightly abbreviated):

```
% cargo --help
Rust's package manager

USAGE:
    cargo [+toolchain] [OPTIONS] [SUBCOMMAND]

OPTIONS:
    -V, --version               Print version info and exit
        --list                  List installed commands
        --explain <CODE>        Run `rustc --explain CODE`
    -v, --verbose               Use verbose output (-vv very verbose/build.rs ou
tput)
    -q, --quiet                 Do not print cargo log messages
        --config <KEY=VALUE>    Override a configuration value (unstable)
    -Z <FLAG>                   Unstable (nightly-only) flags to Cargo, see 'car
go -Z help' for
                                details
    -h, --help                  Print help information
[...]
```

My best guess is that it's outputting a string which is hard-wrapped in the source code.

However, I might care more about well-wrapped text than most since I've been playing with textwrap for ~5 years now :smile: 

---

_Comment by @epage on 2022-04-29 13:52_

btw we recently dropped the dependency on `memchr` in 83f1b165ba9303237cb8ba6878e75aaf9352ce37

---

_Referenced in [clap-rs/clap#3732](../../clap-rs/clap/pulls/3732.md) on 2022-05-17 21:47_

---

_Removed from milestone `3.x` by @epage on 2022-06-08 16:04_

---

_Added to milestone `4.x` by @epage on 2022-06-08 16:04_

---

_Referenced in [clap-rs/clap#3978](../../clap-rs/clap/pulls/3978.md) on 2022-07-23 01:59_

---

_Referenced in [clap-rs/clap#3994](../../clap-rs/clap/issues/3994.md) on 2022-07-26 21:11_

---

_Referenced in [clap-rs/clap#4065](../../clap-rs/clap/pulls/4065.md) on 2022-08-11 19:17_

---

_Referenced in [clap-rs/clap#4111](../../clap-rs/clap/pulls/4111.md) on 2022-08-24 17:29_

---

_Referenced in [clap-rs/clap#4148](../../clap-rs/clap/pulls/4148.md) on 2022-08-30 14:34_

---

_Referenced in [clap-rs/clap#4191](../../clap-rs/clap/issues/4191.md) on 2022-09-07 19:29_

---

_Referenced in [clap-rs/clap#4218](../../clap-rs/clap/issues/4218.md) on 2022-09-15 15:14_

---

_Referenced in [clap-rs/clap#4611](../../clap-rs/clap/issues/4611.md) on 2023-01-09 16:06_

---

_Referenced in [clap-rs/clap#4679](../../clap-rs/clap/issues/4679.md) on 2023-01-27 19:41_

---

_Referenced in [clap-rs/clap#4682](../../clap-rs/clap/issues/4682.md) on 2023-02-23 21:23_

---

_Referenced in [clap-rs/clap#4793](../../clap-rs/clap/issues/4793.md) on 2023-03-27 18:20_

---

_Referenced in [clap-rs/clap#4956](../../clap-rs/clap/issues/4956.md) on 2023-06-09 15:13_

---

_Referenced in [topjohnwu/Magisk#7098](../../topjohnwu/Magisk/pulls/7098.md) on 2023-06-20 13:43_

---

_Referenced in [garikello3d/bigarchiver#7](../../garikello3d/bigarchiver/pulls/7.md) on 2023-12-29 19:04_

---

_Referenced in [clap-rs/clap#5447](../../clap-rs/clap/issues/5447.md) on 2024-04-09 15:59_

---

_Referenced in [nushell/nushell#12466](../../nushell/nushell/issues/12466.md) on 2024-04-10 19:06_

---

_Referenced in [microsoft/sudo#69](../../microsoft/sudo/issues/69.md) on 2024-05-01 21:34_

---

_Comment by @epage on 2025-01-28 17:22_

I'm going to go ahead and close this.  While this is still a priority, we've taken care of a lot of low hanging fruit.  If we people have ideas for further improvements, we should create specific issues/PRs for those.

---

_Closed by @epage on 2025-01-28 17:22_

---
