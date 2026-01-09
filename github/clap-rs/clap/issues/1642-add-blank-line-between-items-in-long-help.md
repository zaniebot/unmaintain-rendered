---
number: 1642
title: Add blank line between items in long help
type: issue
state: closed
author: casey
labels:
  - C-bug
  - A-help
  - E-help-wanted
  - E-easy
assignees: []
created_at: 2020-01-23T22:35:25Z
updated_at: 2020-08-12T04:39:30Z
url: https://github.com/clap-rs/clap/issues/1642
synced_at: 2026-01-07T13:12:19-06:00
---

# Add blank line between items in long help

---

_Issue opened by @casey on 2020-01-23 22:35_

Currently, items in a long help message sometimes have a blank line between them, and sometimes run together. Compare this output from clap:

```
FLAGS:
    -h, --help
            Print help message

    -u, --unstable
            Enable unstable features. To avoid premature stabilization and excessive version churn,
            unstable features are unavailable unless this flag is set. Unstable features are not
            bound by semantic versioning stability guarantees, and may be changed or removed at any
            time.
    -V, --version
            Print version number
```

With this output from `man man`:

```
       -B     Specify  which  browser  to  use  on  HTML files.  This option overrides the
              BROWSER environment variable. By default, man uses /usr/bin/less-is,

       -H     Specify a command that renders HTML files as text.   This  option  overrides
              the HTMLPAGER environment variable. By default, man uses /bin/cat,

       -S  section_list
              List  is  a  colon separated list of manual sections to search.  This option
              overrides the MANSECT environment variable.
```

It might be easier to visually parse if the items in a long help message were always separated by a blank line.

---

_Comment by @CreepySkeleton on 2020-01-23 22:40_

Not sure about the default behavior, but sounds like we can make a global setting for it.

---

_Referenced in [TeXitoi/structopt#344](../../TeXitoi/structopt/issues/344.md) on 2020-01-26 20:40_

---

_Comment by @casey on 2020-01-30 09:30_

Hmm, I just thought that, regardless of the default, it should probably be consistent, so if `AppSettings::BlankLinesBetweenItems` (just making up a name) isn't passed, then there should be no blank lines between items. I think the current behavior depends on if `long_help` is passed or not. Items with `long_help` in my help message don't have blank lines after them, and items without `long_help` have blank lines.

---

_Comment by @pksunkara on 2020-02-01 19:21_

I don't think there should be a setting for this. Blank line between flags and options should exist since it's a standard.

---

_Label `C: help message` added by @pksunkara on 2020-02-01 19:22_

---

_Label `D: easy` added by @pksunkara on 2020-02-01 19:22_

---

_Label `good first issue` added by @pksunkara on 2020-02-01 19:22_

---

_Label `help wanted` added by @pksunkara on 2020-02-01 19:22_

---

_Label `T: enhancement` added by @pksunkara on 2020-02-01 19:22_

---

_Label `W: 3.x` added by @pksunkara on 2020-02-01 19:22_

---

_Added to milestone `3.1` by @pksunkara on 2020-02-01 19:22_

---

_Comment by @CreepySkeleton on 2020-02-02 02:45_

For `--help` - maybe, but what if all of your descriptions are short and fit in one line?

---

_Comment by @pksunkara on 2020-02-02 06:11_

Hmm.. Doing `enquirer select --help` or `enquirer select -h` is giving the same thing because the long help messages are just one line.

```
enquirer-select 0.3.0
Prompt that allows the user to select from a list of options

USAGE:
    enquirer select [FLAGS] [OPTIONS] --message <message> [items]...

FLAGS:
    -h, --help     Prints help information
    -p, --paged    Enables paging. Uses your terminal size

OPTIONS:
    -m, --message <message>      Message for the prompt
    -s, --selected <selected>    Specify number of the item that will be selected by default

ARGS:
    <items>...    Items that can be selected
```

Maybe this is occurring when long help messages are multi line only. Wonder what happens with a mix of them.

---

_Comment by @CreepySkeleton on 2020-02-02 06:16_

I propose:

* For one-line helps only - no blank line by default
* If at least one of help messages doesn't fit in one line - blank lines between all options just to be consistent
* `AppSetting::AlwaysBlankLIne` and `AppSetting::NeverBlankLine` (I suck at naming things) so developers are able to manually chose the style they want

---

_Comment by @pickfire on 2020-03-07 15:59_

`AppSetting::AlwaysBlankLine` and `AppSetting::NeverBlankLine` does not seemed to play well with one-line helps only - no blank line by default.

Shouldn't there be 3 options? I think I propose an additional `AppSetting::AutoBlankLine`, where it blank lines only if it does not fit, for the one-line helps only. `AppSetting::AlwaysBlankLine` should always blank line even thought it fits and `AppSetting::NeverBlankLine` to never blank lines like how it is currently.

As well, how should we define one-line helps? Is it according to the terminal width? Is it according to 80 characters width? Or according to `$MANWIDTH` (I set this)?

It may looks bad too if the terminal is very wide and one-line helps without blank lines that is too long may hard readability (looking at `youtube-dl` help).

Having it too short without blank lines is bad too (looking at `aria2c` help) especially when the short and long option takes so many characters.

`you-get` help is probably what we want here, it even aligns the description across groups (flags / options).

---

_Comment by @pksunkara on 2020-03-07 16:51_

I agree regarding the auto.

I think we are checking terminal width somewhere in the code.

---

_Comment by @pksunkara on 2020-03-14 15:26_

@pickfire If you want something easy to pick up, this issue should be good to go 😄 

---

_Comment by @pickfire on 2020-03-16 01:30_

@pksunkara Yes, this issue seemed easy but the discussion is not complete so I am thinking to take it later once things become clearer.

---

_Comment by @CreepySkeleton on 2020-03-17 21:29_

@pickfire I believe both @pksunkara and I agree on the design you described in a comment above. Regarding to aligning to the terminal's width - let's just start working on it and see what tools we have available at our disposal.

---

_Referenced in [clap-rs/clap#1790](../../clap-rs/clap/issues/1790.md) on 2020-04-10 01:10_

---

_Comment by @pickfire on 2020-05-01 16:59_

```rust
    struct Flags: u64 {
        const BLANK_LINE_ALWAYS              = 1 << 42;
        const BLANK_LINE_AUTO                = 1 << 43;
```
Can we only keep two flag since we can use just two flags to represent 3 of the real flags which can save up some bits for the future? Similar this can be applied to `COLOR_ALWAYS`, `COLOR_AUTO`, `COLOR_NEVER` as well.

@CreepySkeleton I planned to change it to `BlankLineAlways`, `BlankLineAuto` and `BlankLineNever` since that can make these in the documentation side by side and easier to discover.

---

_Comment by @CreepySkeleton on 2020-05-01 18:05_

> I planned to change it to BlankLineAlways, BlankLineAuto and BlankLineNever since that can make these in the documentation side by side and easier to discover.

That would be cool.

Regarding to concrete representation, I truly believe that the decision here should not be based on "let's save a few bits/bytes" considerations. This bitfield is not part of public API, if we decide that `u64` is not enough at some point, we can simply bump it to `u128`. *128 bits ought to be enough for anybody*.

I kind of don't know what would be the best solution here. The problem here is: bits are wery suitable for representing binary yes/no values, but the setting in question has three values, yes/no/detect. Bitflags don't really _fit_. Maybe a separate enum? @kbknapp @pksunkara any ideas?

---

_Comment by @kbknapp on 2020-05-01 19:26_

Perhaps I'm under-thinking it, but I don't think this should be consumer configurable. These rules seem pretty simple and rely on already implemented rules of just determining if multi-line help should be displayed or not:

* If clap decides something should have a multi-line help, it should insert a blank line directly following the message.
* If at least one argument in a heading is determined to need mult-line help, all arguments in that heading should have mult-line help (and thus a blank line inserted directly after)

The issue described by @casey seems to only appear if the `long_help` is over 120 chars, otherwise it doesn't seem to appear and all seems to work (blank lines are where they should be).

So I think this is actually a bug.

While testing for this I found what I would *maybe* deem a bug when the `wrap_help` is not enabled (will post a new issue an link here momentarily).

Edit: See the last example of #1891 to see the bug from this issue in action. Make the `long_help` less than 120 chars, and this issue goes away.

---

_Referenced in [clap-rs/clap#1891](../../clap-rs/clap/issues/1891.md) on 2020-05-01 19:36_

---

_Comment by @pickfire on 2020-05-02 02:41_

> I kind of don't know what would be the best solution here. The problem here is: bits are wery suitable for representing binary yes/no values, but the setting in question has three values, yes/no/detect. Bitflags don't really fit. Maybe a separate enum? @kbknapp @pksunkara any ideas?

That three values could be fitted into two bytes since only one of of the three can be used, so it is either the following options:

- `10` auto
- `01` always
- `00` never

And also, we can just read 2 bytes, of course it is already in memory but I don't see why we do that since it is not possible to have auto but with the other flag set.

---

_Label `T: enhancement` removed by @pksunkara on 2020-05-02 06:23_

---

_Label `T: bug` added by @pksunkara on 2020-05-02 06:23_

---

_Comment by @pksunkara on 2020-05-02 06:24_

Since this is now a bug and not an enhancement, we don't need the flags at all.

---

_Comment by @pickfire on 2020-05-02 08:02_

@pksunkara Err, so what should I do here? Anything that I can help with?

---

_Comment by @pksunkara on 2020-05-02 11:31_

Try to fix the bug? In the test case of first comment of this issue, there should always be an empty line.

---

_Comment by @pickfire on 2020-05-16 04:20_

@casey Can we see the source code for the output? Are you perhaps using `long_help`?

```rust
        Arg::new("arg")
            .long_help("help help help help")
```

Or do you do it?

```rust
        Arg::new("arg")
            .long_help("help help help help
")
```

---

_Comment by @casey on 2020-05-16 04:57_

I no longer use `long_help`, but I think this is the last version of my code which had separate `help` and `long_help`: https://github.com/casey/intermodal/commit/4fffa777b4af5afee208cfffb7d6de5b4972aaf6

---

_Comment by @pickfire on 2020-05-16 13:53_

I believe the issue is related to `long_help`. I cannot seemed to reproduce this, is this still reproducible?
```rust
fn blank_lines_between_long_help() {
    let app = App::new("myapp").arg(Arg::new("unstable").short('u').about(
        "Enable unstable features. To avoid premature stabilization and
excessive version churn, unstable features are unavailable unless this flag is
set. Unstable features are not bound by semantic versioning stability
guarantees, and may be changed or removed at any time.",
    ));
    assert!(utils::compare_output(
        app,
        "myapp --help",
        ISSUE_1642_LONG_HELP,
        false
    ));
}
```

---

_Comment by @mkantor on 2020-08-11 20:05_

Any updates on this issue?

@pickfire it sounded like you were working on it, but the discussion here trailed off and I can't find any related pull requests so I'm assuming it's up for grabs? @pksunkara / @kbknapp / @CreepySkeleton is the consensus still that this is a bug and that a fix without a setting would be acceptable?

---

BTW, the [`long_help` doc](https://docs.rs/clap/2.33.2/clap/struct.Arg.html#method.long_help) is currently incorrect; it describes the behavior as if this issue were fixed.

It says the output for the example code looks like this:

```
FLAGS:
   --config
        The config file used by the myprog must be in JSON format
        with only valid keys and may not contain other nonsense
        that cannot be read by this program. Obviously I'm going on
        and on, so I'll stop now.

-h, --help
        Prints help information

-V, --version
        Prints version information
```

But if you try that exact code with clap 2.33.2, what you actually see is this:

```
FLAGS:
        --config     
            The config file used by the myprog must be in JSON format
            with only valid keys and may not contain other nonsense
            that cannot be read by this program. Obviously I'm going on
            and on, so I'll stop now.
    -h, --help       
            Prints help information

    -V, --version    
            Prints version information
```

(Without an empty line before `-h, --help`.)

---

_Referenced in [mkantor/clap#1](../../mkantor/clap/pulls/1.md) on 2020-08-11 22:39_

---

_Comment by @pksunkara on 2020-08-11 22:57_

@mkantor Go ahead and work on the issue. Yes, it's still a bug. Regarding the long_help doc, please check master instead of v2.33 on docs.rs.

---

_Referenced in [clap-rs/clap#2058](../../clap-rs/clap/pulls/2058.md) on 2020-08-11 23:05_

---

_Comment by @mkantor on 2020-08-11 23:05_

Bam! #2058

---

_Closed by @bors[bot] on 2020-08-12 00:01_

---

_Comment by @pickfire on 2020-08-12 04:38_

@mkantor I was working on it but I couldn't reproduce it so I stopped working on it.

---

_Referenced in [TeXitoi/structopt#471](../../TeXitoi/structopt/issues/471.md) on 2021-03-25 11:11_

---
