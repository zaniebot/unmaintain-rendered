```yaml
number: 1891
title: Sub-optimal help wrapping with long messages by default
type: issue
state: closed
author: kbknapp
labels:
  - C-bug
assignees: []
created_at: 2020-05-01T19:36:44Z
updated_at: 2020-05-30T17:24:21Z
url: https://github.com/clap-rs/clap/issues/1891
synced_at: 2026-01-12T16:14:11Z
```

# Sub-optimal help wrapping with long messages by default

---

_@kbknapp_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closes issues

### Code

```rust
use clap::{App, Arg};
fn main() {
    let m = App::new("fake")
        .arg(Arg::with_name("f")
            .long("fake")
            .short('f')
            .takes_value(true)
            .help("something really short")
            .long_help("something less than 120 chars long but longer than just your average few word message"))
        .arg(Arg::with_name("r")
            .long("rake")
            .short('r')
            .takes_value(true)
            .long_help("something over 120 chars in width which is what clap tries to wrap the help at by default if wrap_help isnt enabled or no other terminal wrapping settings are set"))
        .arg(Arg::with_name("t")
            .long("take")
            .short('t')
            .takes_value(true)
            .help("something really short"))
        .arg(Arg::with_name("d")
            .long("dake")
            .short('d')
            .takes_value(true)
            .help("something really short"))
        .arg(Arg::with_name("p")
            .long("pake")
            .short('p')
            .long_help("something over 120 chars in width which is what clap tries to wrap the help at by default if wrap_help isnt enabled or no other terminal wrapping settings are set"))
        .arg(Arg::with_name("l")
            .long("lake")
            .short('l')
            .help("something really short"))
        .arg(Arg::with_name("m")
            .long("make")
            .short('m')
            .long_help("something less than 120 chars long but longer than just your average few word message")
            .help("something really short"))
        .get_matches();
}
```

### Steps to reproduce the issue

#### Default (no `wrap_help`)

First the short help

```
$ cargo run -- -h
fake

USAGE:
    fake [FLAGS] [OPTIONS]

FLAGS:
    -h, --help       Prints help information
    -l, --lake       something really short
    -m, --make       something really short
    -p, --pake       something over 120 chars in width which is what clap tries to wrap 
the help at by default if
                     wrap_help isnt enabled or no other terminal wrapping settings are set
    -V, --version    Prints version information

OPTIONS:
    -d, --dake <d>    something really short
    -f, --fake <f>    something really short
    -r, --rake <r>    something over 120 chars in width which is what clap tries to wrap 
the help at by default if
                      wrap_help isnt enabled or no other terminal wrapping settings are 
set
    -t, --take <t>    something really short
```

Next long_help

```
$ cargo run -- --help
fake

USAGE:
    fake [FLAGS] [OPTIONS]

FLAGS:
    -h, --help
            Prints help information

    -l, --lake
            something really short

    -m, --make
            something less than 120 chars long but longer than just your average few word message

    -p, --pake
            something over 120 chars in width which is what clap tries to wrap the help at by default if wrap_help isnt
            enabled or no other terminal wrapping settings are 
set
    -V, --version
            Prints version information


OPTIONS:
    -d, --dake <d>
            something really short

    -f, --fake <f>
            something less than 120 chars long but longer than just your average few word message

    -r, --rake <r>
            something over 120 chars in width which is what clap tries to wrap the help at by default if wrap_help isnt
            enabled or no other terminal wrapping settings are set
    -t, --take <t>
            something really short
```

#### With `wrap_help` enabled

Short help:

```
$ cargo run -- -h
fake

USAGE:
    fake [FLAGS] [OPTIONS]

FLAGS:
    -h, --help       Prints help information
    -l, --lake       something really short
    -m, --make       something really short
    -p, --pake       something over 120 chars in width which is what clap tries to wrap the help at by
                     default if wrap_help isnt enabled or no other terminal wrapping settings are set
    -V, --version    Prints version information

OPTIONS:
    -d, --dake <d>    something really short
    -f, --fake <f>    something really short
    -r, --rake <r>    something over 120 chars in width which is what clap tries to wrap the help at by
                      default if wrap_help isnt enabled or no other terminal wrapping settings are set
    -t, --take <t>    something really short
```

Long help:

```
$ cargo run -- --help
fake

USAGE:
    fake [FLAGS] [OPTIONS]

FLAGS:
    -h, --help
            Prints help information

    -l, --lake
            something really short

    -m, --make
            something less than 120 chars long but longer than just your average few word message

    -p, --pake
            something over 120 chars in width which is what clap tries to wrap the help at by default if
            wrap_help isnt enabled or no other terminal wrapping settings are set
    -V, --version
            Prints version information


OPTIONS:
    -d, --dake <d>
            something really short

    -f, --fake <f>
            something less than 120 chars long but longer than just your average few word message

    -r, --rake <r>
            something over 120 chars in width which is what clap tries to wrap the help at by default if
            wrap_help isnt enabled or no other terminal wrapping settings are set
    -t, --take <t>
            something really short
```

### Version

* **Rust**: `rustc 1.44.0-nightly (b2e36e6c2 2020-04-22)`
* **Clap**: `3.0.0-beta.1 (master HEAD 99e86294)`

### Actual Behavior Summary

Notice the weird line breaks

### Expected Behavior Summary

No weird line breaks

### Additional context

May relate to #1642 because if all `long_help` items are less than 120 chars in width, the issue linked there goes away.

---

_Label `T: bug` added by @kbknapp on 2020-05-01 19:36_

---

_Comment by @kbknapp on 2020-05-01 19:40_

Forgot to mention, actual terminal width must be less than 120.

And I know it's due to us inserting hard line breaks at 120, but then attempting to re-calculate where to start printing again. 

The motivating factor behind this issue is to discuss if there is a better way to handle small terminals by default.

---

_Comment by @kbknapp on 2020-05-01 19:48_

Full disclosure, I'm perfectly OK if we say, "This behaviour isn't perfect but it's better than nothing." Or even, "This behaviour isn't perfect, but the alternative is too complex or not worth the effort."

---

_Referenced in [clap-rs/clap#1642](../../clap-rs/clap/issues/1642.md) on 2020-05-01 19:49_

---

_Added to milestone `3.1` by @pksunkara on 2020-05-01 21:24_

---

_Comment by @pksunkara on 2020-05-01 21:27_

I think we should not wrap at all if `wrap_help` is not enabled.

---

_Comment by @kbknapp on 2020-05-01 21:48_

> I think we should not wrap at all if wrap_help is not enabled.

That was the original functionality, and only changed later as people requested a smaller default. I can't remember if the `max|min_term_width` came before or after those requests though. It's possible those didn't exist at the time and picking some sane-ish default (120) is what we landed on.

The counter argument to no wrapping is that if your help string is over whatever the average full screen width is (of course will vary by font, size, and resolution) such that the words will wrap no matter what, then no wrapping at all looks bad as well. 

In fact, even help lines slightly over 120 don't look great. So wrapping *does* help.

The other option we have is that we could *lower* the default to 100 or even 80 so that most programs look good. If your terminal is less than 80 chars, you probably expect some kind of oddity to be honest. We lose a decent number of effective characters just through the flag/option (20-40 chars on average?), so 80 might actually be too small without having help messages that just always wrap. 100 might be the sweet spot.

A quick look at grep.app shows nearly all uses of `max_term_width` are `max_term_width(100)`.

Side note, I think I'm just starting to hit this more personally because I have a vertical monitor and primarily use i3. So it's not uncommon for me to have a terminal window that is less than 120 characters wide (...it's 104 characters wide, I just counted...).

---

_Comment by @CreepySkeleton on 2020-05-01 23:03_

It had me thinking: why don't we just query the terminal width and use it to insert breaks instead of a preset/hardcoded default? Oh right, `wrap_help` is optional... which raises the question of why it's optional. 

Compilation time? Nope, it was the same with and without the feature on the build server (release build).

Code size? Lol, enabling the feature _decreased_ the size by ~ 0.1Kb.

That said, my call is simple: ditch the feature as pretty useless and remove the hardcoded breaks, using terminal size instead.

---

_Comment by @kbknapp on 2020-05-01 23:16_

While I'm *very* inclined to agree with you, there is a non-negligible portion of the Rust community that is anti-dependency, or at least wants their dep graph as small as possible. I don't really agree with that line of thinking, other than cutting out dependencies that actually can cause bloat for little gain.

Based off those stats, perhaps we add it to the default list of features, which still allows those to disable if they so choose We could lower the default max term size to 100 at the same time.

---

_Comment by @kbknapp on 2020-05-01 23:19_

Along those same lines but also a separate issue entirely, at some point I would like to have the discussion about aggressive cargo feature use. Ideally, all non-essential sections of clap could be disabled (but enabled by default) to allow small binary sizes when desired. I've had quite a few people / companies approach me about clap's binary size being their only issue. In fact, I believe that is the number one factor that led Google to work on `argh`, was binary size is their top concern (for Fuschia) and don't need a lot of the "extra" features that clap provides.

---

_Comment by @CreepySkeleton on 2020-05-02 00:34_

On the other hand, people from this anti-dependency camp likely see clap as bloat-full and having too much allegedly unneeded deps irrespective of `term_size`. Clap already pulls 7 crates in `--no-default-features` mode, 22 in `default-features` mode, and 27 in `all-features` mode; one extra crate - very tiny and very useful btw - won't change their opinion. It's kind of all-or-nothing situation, 8 deps (including transitive) kind of count as "as small as possible". I really doubt any of them would become our user if we feature-gate one or two tiny crates. 

That's not to mention that clap itself takes as much time to build as (almost) all of the deps combined, and the compilation time is a _much_ more common complaint among users (in my view). Along with code bloat, which would only decrease without this feature.

___

Regarding modularization of clap.

I've been thinking about it too, for some time, and I've come to conclusion that, while it's doable - to some extent, it would require full-scale refactoring. And by full-scale I mean "rewrite the parser and the validator entirely" which is quite an endeavor.

But yeah, we can talk about it, it's not impossible.

---

_Comment by @Dylan-DPC-zz on 2020-05-02 00:37_

Given how popular clap is, the number of dependencies is definitely a huge concern. I've been planing of tackling a lighter clap in some form or the other post the 3.0 release. That discussion is beyond the scope of this issue.

---

_Referenced in [clap-rs/clap#1365](../../clap-rs/clap/issues/1365.md) on 2020-05-02 01:19_

---

_Comment by @kbknapp on 2020-05-02 01:47_

Moved the discussion about code size, deps, and modularity to #1365 so this issue can stay on topic about what we do with wrapping the help message by default.

---

_Comment by @pickfire on 2020-05-02 02:46_

Shouldn't this be the default? My terminal is just lightly over 80 characters width, lesser than 90 characters. T_T

---

_Referenced in [clap-rs/clap#1398](../../clap-rs/clap/issues/1398.md) on 2020-05-05 19:25_

---

_Comment by @kbknapp on 2020-05-30 03:24_

[Linus Torvalds the issue](https://lkml.org/lkml/2020/5/29/1038)

---

_Comment by @CreepySkeleton on 2020-05-30 15:10_

> 
> 
> [Linus Torvalds the issue](https://lkml.org/lkml/2020/5/29/1038)

Hmm... Hmm... Uhuh.

Well, the `grep` argument isn't really applicable to clap's help output because help descriptions tend to be multi-line anyway. The only thing people grep from the `--help` output would be the name of an option or subcommand or whatever, and you _do_ expect the description to be multi-line in many cases, wrapping or not.

I pretty much agree with everything else.

Honestly, I feel like we're running in circles here. Given that we aren't subscribing to using `wrap_help` unconditionally (I disagree, but it's not so important to be arguing about), can we just decide on the default number? 120 is good for me personally, and I think 100 is a sane choice.

---

_Comment by @pickfire on 2020-05-30 15:36_

Can we make it depends on the terminal width user have rather than giving our own opinion of default? We could look into `$COLUMNS` for this. But of course, we wouldn't want anyone using long terminals to suffer. But still, even if I have long terminals, I would prefer to read short lines since it is hard to read a line that is 120 characters long, I might easily jump on to the next or previous line.

Sadly, I am still using 106 or 87 columns width so application from nodejs that expects even a mere 100 do wrapping that makes my eye bleed. I am probably similar to one of the guy that he mentioned complained about compiling the kernel from raspberry pi 4 with 4GB of RAM, I compile rust from a ~10 years old laptop (thinkpad x220) and complained that it took 5+ hours.

---

_Comment by @pksunkara on 2020-05-30 15:39_

> Can we make it depends on the terminal width user have rather than giving our own opinion of default?

That's when `wrap_help` is enabled. This issue is about a sane default when it is disabled. Of course we can use `$COLUMNS` but it's not always available. This issue is about the default when it's not available.

---

_Comment by @pickfire on 2020-05-30 15:58_

Then I believe 100 is good, especially when rust uses 100 too haha.

---

_Comment by @kbknapp on 2020-05-30 16:23_

> Linus Torvalds the issue

Sorry, it was late and I should have clarified I didn't post that link because I agree or disagree, just because it was some recent discussion on the topic :stuck_out_tongue_winking_eye: 

---

_Referenced in [clap-rs/clap#1950](../../clap-rs/clap/pulls/1950.md) on 2020-05-30 16:39_

---

_Comment by @CreepySkeleton on 2020-05-30 16:42_

It doesn't matter, I guess. It's just the problem is pretty much non-pressing, it generates much more discussion than it worth, and it's not like we're making some sort of hard decision here - we can always revise it in future. We're all in agreement that 100 is a good choice, so let's just get it over with.

---

_Closed by @bors[bot] on 2020-05-30 17:24_

---
