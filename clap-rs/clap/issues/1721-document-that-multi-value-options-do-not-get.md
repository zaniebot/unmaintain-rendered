```yaml
number: 1721
title: Document that multi-value options do not get along with trailing subcommands
type: issue
state: closed
author: nagisa
labels:
  - A-docs
assignees: []
created_at: 2020-03-04T16:51:10Z
updated_at: 2020-04-21T17:05:31Z
url: https://github.com/clap-rs/clap/issues/1721
synced_at: 2026-01-12T16:14:11Z
```

# Document that multi-value options do not get along with trailing subcommands

---

_@nagisa_

### Rust Version

rustc 1.43.0-nightly (7760cd0fb 2020-02-19)

### Code

```rust
use clap::*;

fn main() {
    let matches = App::new("My Super Program")
                      .subcommand(SubCommand::with_name("test"))
                      .arg(Arg::with_name("config")
                           .long("config")
                           .value_name("FILE")
                           .multiple(true)
                           .takes_value(true))
                      .get_matches();
    println!("{:?}", matches);
}
```

### Steps to reproduce the issue

1. `cargo run -- --config 'banana=true' test`
2. `cargo run -- --config='banana=true' test`
3. Note how clap parses the last argument.

### Affected Version of `clap*`

2.33

### Actual Behavior Summary

clap will greedily consume values into space-separated `--config` flag.

### Expected Behavior Summary

I’m not sure how common place this behaviour (`--long arg1 arg2` parsing as `--long arg1 --long arg2`) is. Definitely was unexpected to me.

---

_Label `T: bug` added by @nagisa on 2020-03-04 16:51_

---

_Comment by @CreepySkeleton on 2020-03-04 16:56_

Yeah, this is [a known problem](https://github.com/clap-rs/clap/issues/1707#issuecomment-592920247), but `clap` simply doesn't know where the options end and where the subcommand starts. Even if there's a way to rule this out, I'm not aware of it, so I doubt this will change. Take it as limitation.

I think we should document it better though.

[We also consider to improve error reporting in such cases](https://github.com/clap-rs/clap/issues/1708).



---

_Renamed from "`multiple(true)` top-level flags do not interact well with subcommands." to "Document that multi-value options do not get along with trailing subcommands" by @CreepySkeleton on 2020-03-04 16:57_

---

_Label `T: bug` removed by @CreepySkeleton on 2020-03-04 16:57_

---

_Label `C: docs` added by @CreepySkeleton on 2020-03-04 16:57_

---

_Comment by @pksunkara on 2020-03-04 16:58_

Check this for more info, https://github.com/clap-rs/clap/issues/1610 and please give us feedback if that explains your issue. I am thinking of pointing to that issue in the docs for `multiple`.

---

_Comment by @nagisa on 2020-03-04 17:01_

Is there a way to implement an option that would require each value for the multiple flag to be accompanied with the flag? That is to disable this implicit token consumption? I’m sure most of the users will be plenty happy with `--flag val1 val2...` not parsing the `val2` into `--flag`.

EDIT: That is, I originally expected `multiple` to mean that it would require something along the lines of `--flag val1 --flag val2...` and if I wanted to supply multiple values without repeating the `--flag` I would be responsible for setting `use_delimiter`.

EDIT: Ah, I see, its `require_delimiters`. Cool. Definitely very difficult to discover.

---

_Comment by @CreepySkeleton on 2020-03-04 17:06_

The more I think about it, the more I get assured that we need **two different methods for that**: `multiple_values` and `multiple_occurrences`.

* `multiple_values(true)`: `--foo val1 val2` but not `-foo val1 --foo val2`
* `multiple_occurrences(true)`: `-foo val1 --foo val2` but not `--foo val1 val2`
* `multiple_values(true).multiple_occurrences(true)`: enables both

---

_Comment by @nagisa on 2020-03-04 17:49_

I got mostly the behaviour I want by setting `require_delimiters(true).value_delimiter("\0")`, so this is not critical by any means.

---

_Comment by @CreepySkeleton on 2020-03-04 19:18_

But it should be documented better nonetheless.

---

_Referenced in [TeXitoi/structopt#367](../../TeXitoi/structopt/issues/367.md) on 2020-03-31 12:58_

---

_Added to milestone `3.0` by @pksunkara on 2020-04-09 08:20_

---

_Comment by @davidMcneil on 2020-04-15 20:26_

> Yeah, this is a known problem, but clap simply doesn't know where the options end and where the subcommand starts. Even if there's a way to rule this out, I'm not aware of it, so I doubt this will change. Take it as limitation.

@CreepySkeleton could we check if the next token matches a subcommand and if it does stop parsing the argument and instead parse the subcommand? Essentially, remove this [line](https://github.com/clap-rs/clap/blob/4805de02edd10454b7bb9819fdb4ce306c841b2b/src/app/parser.rs#L941). It looks like this used to be the behavior, but was changed due to #1031.

My personal opinion is that handling (1) "multi-value options with subcommands" is more important than (2) "argument value taking precedence over subcommand with the same name". The reasons being:

* 1 occurs more frequently than 2. How often do arg values and subcommands with the same name occur next to each other?
* 2 the user of the cli can easily fix 2 by simply adding an `=`. There is no good way for the user to get around 1 without the author of the app already taking special action.

---

_Comment by @pksunkara on 2020-04-15 22:02_

We are happy to add support for something like this with an AppSetting. But it would have to be something for later and not a priority for 3.0

---

_Comment by @CreepySkeleton on 2020-04-16 00:58_

If we remove that line, positional with multiple values would stop working at all; the logic over there looks far more complicated.

1. I believe, when it comes to defaults, this kind of decisions should not be based on personal opinions, yours and mine all the same. For example, my experience is exactly contrary here.

2. *This* is a good argument. 

I'd go for changing the default behavior (because 2) and introducing an `AppSetting` or something so developer could revert it back.

---

_Comment by @pksunkara on 2020-04-16 06:14_

I wouldn't change the default but introduce an `AppSetting`. I feel the default should be how it is behaving now.

---

_Comment by @CreepySkeleton on 2020-04-16 07:24_

Maybe we should conduct a survey of sorts? Ask on users forum or something, "Here's the problem, which default would you like and why?". Our opinions... well, they are *our* opinions, and we are not the majority here, despite we are indeed the greatest!

---

_Comment by @pksunkara on 2020-04-16 07:57_

Adding `--` after multi value options is the standard in CLI tools. We had said this many times in the past to people who came to us with this issue. And this is also the main reason we should keep it as a default.

---

_Comment by @CreepySkeleton on 2020-04-16 09:30_

No no, these are different problems. 

* `--` is about "everything after this delimiter belongs to a single clap Arg, no matter what it starts from (subcommand name, `-`, whatever). It is used as *the last* option in CLI, no option can possibly follow it because it would be considered as a value.
  ```
  app --option subcommand --option2 -- --option3 abc subcommand
      -----------------------------    ------------------------
       normal options and subcmds      this is not parsed at all
                                       just a list of values
  ```
* This problem is about "if an option taking unrestrained number of values contains something that is a valid subcommand among these values, the subcommand (and everything following) must not be considered as value". In other words, such option *can* be followed by something else. This is possible today via
  ```
  app --option val1 val2 val3 subcommand arg
               -------------- ---------- ---
                    values     sub name  sub argument
  ```

To be honest, I personally don't like this design. I consider this design error prone and pretty non-intuitive, I would not write such a CLI and I would try to dissuade people from this sort of desing. But, even thought I'm admittedly a very wise person, I'm not the _whole_ community. This is why I'm proposing to _ask_. Just to hear people out.

If we find their argument not so compelling or opinions will differ significantly, we will just pull our Dictatorship card and chant "Succumb to our will!".

---

_Label `T: new setting` added by @pksunkara on 2020-04-16 10:04_

---

_Referenced in [clap-rs/clap#1834](../../clap-rs/clap/pulls/1834.md) on 2020-04-16 13:33_

---

_Closed by @pksunkara on 2020-04-21 17:05_

---

_Referenced in [clap-rs/clap#1915](../../clap-rs/clap/issues/1915.md) on 2020-05-08 03:08_

---
