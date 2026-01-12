```yaml
number: 694
title: Combination of options causes clap to apply validator to both default and user-specified option
type: issue
state: closed
author: ssokolow
labels: []
assignees: []
created_at: 2016-10-18T01:50:32Z
updated_at: 2017-01-03T06:56:17Z
url: https://github.com/clap-rs/clap/issues/694
synced_at: 2026-01-12T16:14:09Z
```

# Combination of options causes clap to apply validator to both default and user-specified option

---

_@ssokolow_

rustc version: rustc 1.12.0 (3191fbae9 2016-09-23)
clap Version: 2.14.0

In one of my new projects, I noticed that it was dying with an error about the default argument value failing "path is readable" validation, even when I was trying to override it with an explicit argument.

I started working on a simplified testcase and found that this bug is very picky. If any of these conditions aren't met, it will act properly and validate either the default or the user-provided value but not both:
- `global(true)` must be specified
- A `default_value` must be specified
- A `validator` must be provided
- A subcommand must be defined and passed on the command line

...but, if all of them are, it insists on validating both the default and the user-provided value.

It's possible there's more I could discover, but I'm sick and it's getting late, so here's how far I managed to minimize and pin down the test case so far:

``` rust
extern crate clap;
use clap::{App,Arg,SubCommand};

const DEFAULT_INPATH: &'static str = "/dev/sr1";

fn main() {
    App::new("testcase")
        .arg(Arg::with_name("inpath")
            .global(true)
            .default_value(DEFAULT_INPATH)
            .validator(|x| {
                print!("Testing {}\n", x);
                if x == DEFAULT_INPATH {
                    print!("ERROR: Validator ran on default string\n");
                }
                Ok(())
            }))
        .subcommand(SubCommand::with_name("test"))
        .get_matches_from(&["testcase", "/dev/sr0", "test"]);
}
```

**EDIT:** I also just noticed that it's validating the default _after_ the user-provided value:

``` sh
ssokolow@monolith testcase % cargo run            
    Finished debug [unoptimized + debuginfo] target(s) in 0.0 secs
     Running `target/debug/testcase`
Testing /dev/sr0
Testing /dev/sr1
ERROR: Validator ran on default string
```


---

_Comment by @ssokolow on 2016-10-18 02:43_

Oh, I almost forgot to mention. If the `Arg` is a flag like `--inpath=[inpath]` rather than a positional argument, the `--inpath=/dev/sr0` must come after the subcommand on the command line to trigger the bug.


---

_Comment by @kbknapp on 2016-10-18 14:14_

Thanks for all the detail! This will certainly help in getting this bug fixed quickly! I should be able to mock up a solution and test tonight when I get home. I'll post back here with the solution. Once this bug is fixed, and the few PRs waiting to be merged are done I'll put out the new version on crates.io (should be 2.14.1).


---

_Label `T: bug` added by @kbknapp on 2016-10-18 14:14_

---

_Label `P2: need to have` added by @kbknapp on 2016-10-18 14:14_

---

_Label `C: parsing` added by @kbknapp on 2016-10-18 14:14_

---

_Label `W: 2.x` added by @kbknapp on 2016-10-18 14:14_

---

_Assigned to @kbknapp by @kbknapp on 2016-10-18 14:14_

---

_Added to milestone `2.14.1` by @kbknapp on 2016-10-18 14:16_

---

_Comment by @ssokolow on 2016-10-18 20:47_

You're welcome. Aside from a fast resolution benefiting me too, it's the least I can do as thanks for offering up an enviable argument parser under a free license.

(And I do mean enviable. I really wish Python's argparse could generate bash completions for my non-Rust projects.)


---

_Comment by @kbknapp on 2016-10-19 17:47_

I was having computer issues last night and wasn't able to test this earlier. But now I've tested it.

As your minimal test is written, clap is functioning _somewhat_ as intended. Here's what's happening in the test case above:
- clap gets an explicit value for `inpath`, and validates it
- clap then reaches the subcommand `test`
- because `inpath` is global, there is effectively a `test <inpath>` as well.
- Since `inpath` isn't required, the subcommand's `inpath` is empty and therefore gets the default value applied to it
- clap then validates the default value of the _subcommand's_ `inpath`

Correct me if I'm wrong, but what you'd like is for the value to propagated down through child subcommands? If so, that's a feature that could probably be added as an optional setting. 

Edit: Incorrect data (fixed now)... 


---

_Comment by @ssokolow on 2016-10-19 17:53_

Hmm. I'll want to offer you another testcase then (probably later tonight), since I don't see how your explanation of the behaviour would make sense for what I observed when using `--inpath=[inpath]` rather than a positional argument.


---

_Comment by @kbknapp on 2016-10-19 17:55_

I haven't tested the `--inpath=[inpath]` yet, that's next :wink:

Edit: Just ran the tests with `--inpath=[inpath]` and it appears to function the same for me.

Using 

``` rust
extern crate clap;
use clap::{App,Arg,SubCommand};

const DEFAULT_INPATH: &'static str = "/dev/sr1";

fn main() {
    App::new("testcase")
        .arg(Arg::with_name("inpath")
            .global(true)
            .long("inpath") // added this
            .takes_value(true) // and this
            .default_value(DEFAULT_INPATH)
            .validator(|x| {
                print!("Testing {}\n", x);
                if x == DEFAULT_INPATH {
                    print!("ERROR: Validator ran on default string\n");
                }
                Ok(())
            }))
        .subcommand(SubCommand::with_name("test"))
        .get_matches_from(&["testcase", "--inpath=/dev/sr0", "test"]); // changed this
}
```


---

_Comment by @kbknapp on 2016-10-19 17:56_

Also, if you compile clap with `features = ["debug"]` it'll spit out tons of data while parsing.


---

_Comment by @ssokolow on 2016-10-19 17:59_

I probably would have thought to look for ways to produce debugging output, but, while I was working on the testcase, I had a very bad cold and wasn't really thinking clearly. (It didn't even occur to me to keep the `--inpath` testcase while reducing the problem in case they turned out to be two related bugs.)

Thankfully, I seem to be past the worst of it now.


---

_Label `T: RFC / question` added by @kbknapp on 2016-10-19 18:57_

---

_Label `C: parsing` removed by @kbknapp on 2016-10-19 18:57_

---

_Label `P2: need to have` removed by @kbknapp on 2016-10-19 18:57_

---

_Label `T: bug` removed by @kbknapp on 2016-10-19 18:57_

---

_Label `W: 2.x` removed by @kbknapp on 2016-10-19 18:57_

---

_Removed from milestone `2.14.1` by @kbknapp on 2016-10-19 18:57_

---

_Comment by @kbknapp on 2016-11-02 02:59_

@ssokolow I'm going to close this. Please let me know if this is still an issue and we can re-open.


---

_Closed by @kbknapp on 2016-11-02 02:59_

---

_Comment by @ssokolow on 2016-11-02 03:37_

Will do. I've been meaning to test for days but more pressing concerns just keep popping up.


---

_Comment by @ssokolow on 2016-11-25 08:05_

Sorry for the wait. What version was this supposed to be fixed in? I'm still getting test failures under 2.19.0.

---

_Comment by @kbknapp on 2016-11-26 20:59_

@ssokolow can you provide your failing test case?

---

_Reopened by @kbknapp on 2016-11-26 20:59_

---

_Comment by @ssokolow on 2016-11-27 03:29_

Here's the "not yet beyond clap boilerplate" project that's failing.

https://github.com/ssokolow/rip_media

`cargo test` will illustrate the problem very clearly.

**EDIT:** ...assuming you're running a Linux system with at least one CD/DVD/BluRay drive. Otherwise, you'll need to replace the hard-coded `/dev/sr0` default path with something else.

---

_Comment by @kbknapp on 2016-11-28 16:34_

I've taken a look at this, and it looks like my original explanation stands. Propagating those values down through the child `--inpath` args would solve this issue. But my hesitation some users may not want that behavior due to conflicting uses of a global argument. Imagine a command which uses a global arg, but the implementation means the context is different depending on which subcommand the value was provided to.

What I would suggest is one of two things:

1. Don't use global args in this instance, and instead confine the `--inpath` to parent command
2. Instead of subcommands, use a single positional argument with predefined values ([`Arg::possible_values`](https://docs.rs/clap/2.19.0/clap/struct.Arg.html#method.possible_values)) since you're not defining additional arguments on those subcommands and merely using them to differentiate between values.

Both of those approaches have their pros and cons. If you were planning on adding additional unique args to those subcommands option 2 isn't as feasible. Option 1 provides a slightly less flexible CLI.

Of course there's also the ability to add another `AppSettings` variant which allows propagating values down through subcommands. The only problem with this, is due to how it's implemented, it's not easily possible to propagate values *up* or *sideways* only down in the tree. So this setting could be a potential point of confusion.

---

_Comment by @ssokolow on 2016-11-28 16:47_

Option 2 is unfeasible for exactly the reason you stated. Processing CleanRip dumps from a Wii SD card is going to have options different from those for `/media/ssokolow/RETRODE` which, again, will be different from optical media, which will have their own special options if they're being dumped as audio or audio+data CDs. I just happened to nail down the common schema first.

As for option 1, it's not an acceptable user experience for when I dogfood it. I'd sooner include two separate argument parsers into my program with the first one's only purpose being to recognize global arguments and reposition them in the command line before passing it on to clap.

(I'm the "good user experience at any cost" type. The whole reason I'm using Rust is because the compile-time checks help me accomplish that goal.)

For comparison with another parser's solution, when I do this with [`argparse`](https://docs.python.org/dev/library/argparse.html) in Python, it's accomplished via a mechanism analogous to OOP inheritance where an `ArgumentParser` is defined for the common arguments and then passed via a `parents=[]` argument when instantiating each subcommand's `ArgumentParser` instance.

---

_Comment by @kbknapp on 2016-11-28 17:58_

Ok, I'm good adding the settings variant which propagates values down, that'd actually be a decently easy addition. I'll play with some implementations once I've got a little bit of time and post back here.

---

_Label `C: settings` added by @kbknapp on 2016-11-28 17:59_

---

_Label `D: easy` added by @kbknapp on 2016-11-28 17:59_

---

_Label `P4: nice to have` added by @kbknapp on 2016-11-28 17:59_

---

_Label `T: new setting` added by @kbknapp on 2016-11-28 17:59_

---

_Label `W: 2.x` added by @kbknapp on 2016-11-28 17:59_

---

_Label `T: RFC / question` removed by @kbknapp on 2016-11-28 17:59_

---

_Added to milestone `2.20.0` by @kbknapp on 2016-12-27 04:36_

---

_Closed by @homu on 2017-01-03 06:56_

---
