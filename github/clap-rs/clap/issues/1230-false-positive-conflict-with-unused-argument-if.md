---
number: 1230
title: "False positive: conflict with unused argument if in required group"
type: issue
state: closed
author: ghost
labels:
  - C-bug
assignees: []
created_at: 2018-03-25T02:05:38Z
updated_at: 2020-04-21T17:13:50Z
url: https://github.com/clap-rs/clap/issues/1230
synced_at: 2026-01-07T13:12:19-06:00
---

# False positive: conflict with unused argument if in required group

---

_Issue opened by @ghost on 2018-03-25 02:05_

### Rust Version
```
rustc 1.26.0-nightly (f5631d9ac 2018-03-24)
```
### Affected Version of clap
```
"checksum clap 2.31.2 (registry+https://github.com/rust-lang/crates.io-index)" = "f0f16b89cbb9ee36d87483dc939fe9f1e13c05898d56d7b230a0d4dff033a536"
```
### Expected Behavior Summary
There is one requred group.
The requirement is fulfilled with `"-a"`.

Only one of `"arg"` and `"unused"` is specified.
I expected no conflict.

`ArgGroup::required`
> A required ArgGroup simply states that one argument from this group must be present at runtime (unless conflicting with another argument).

and `Arg::conflicts_with`
> Sets a conflicting argument by name. I.e. when using this argument, the following argument can't be present and vice versa.

### Actual Behavior Summary
Arguments get parsed without error.
Interestingly, if the group is not required, there is no error.

### Steps to Reproduce the issue
Enter code below.

### Sample Code or Link to Sample Code
```rust
extern crate clap;
use clap::{App, Arg, ArgGroup};

fn main() {
    let matches = App::new("name")
        .group(ArgGroup::with_name("group")
            .required(true) // no error iff false
            .arg("arg")
            .arg("unused")
        )
        .arg(Arg::with_name("arg")
            .short("a")
            .conflicts_with("unused"))
        .arg(Arg::with_name("unused"))
        .get_matches_from(vec![
            "-a",
        ]);
    println!("{:?}", matches);
}
```

### Debug output
```
DEBUG:clap:Parser::propagate_settings: self=name, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::remove_overrides:[];
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Parser::add_defaults:iter:unused: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:unused: doesn't have default vals
DEBUG:clap:Validator::validate_blacklist;
DEBUG:clap:Validator::validate_required: required=["group"];
DEBUG:clap:Validator::validate_required:iter:group:
DEBUG:clap:Validator::missing_required_error: extra=None
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Validator::missing_required_error: reqs=[
    "group"
]
DEBUG:clap:usage::get_required_usage_from: reqs=["group"], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["group"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=["arg", "unused"]
DEBUG:clap:Colorizer::error;
DEBUG:clap:Validator::missing_required_error: req_args="\n    \u{1b}[1;31m<-a|unused>\u{1b}[0m"
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::get_required_usage_from: reqs=["group"], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["group"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=["arg", "unused"]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=arg;
DEBUG:clap:Parser::groups_for_arg: name=arg
DEBUG:clap:Parser::groups_for_arg: Searching through groups...
        Found 'group'
DEBUG:clap:usage::needs_flags_tag:iter:iter: grp_s=group;
DEBUG:clap:usage::needs_flags_tag:iter:iter: Group is required
DEBUG:clap:usage::needs_flags_tag:iter: f=hclap_help;
DEBUG:clap:usage::needs_flags_tag:iter: f=vclap_version;
DEBUG:clap:usage::needs_flags_tag: [FLAGS] not required
DEBUG:clap:usage::get_args_tag;
DEBUG:clap:usage::get_args_tag:iter:unused:
DEBUG:clap:Parser::groups_for_arg: name=unused
DEBUG:clap:Parser::groups_for_arg: Searching through groups...
        Found 'group'
DEBUG:clap:usage::get_args_tag:iter:unused:iter:group;
DEBUG:clap:usage::create_help_usage: usage=-a <-a|unused>
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Colorizer::error;
DEBUG:clap:Colorizer::good;
error: The following required arguments were not provided:
    <-a|unused>

USAGE:
    -a <-a|unused>

For more information try --help
```

---

_Label `T: bug` added by @kbknapp on 2018-03-25 02:49_

---

_Label `P2: need to have` added by @kbknapp on 2018-03-25 02:49_

---

_Label `D: intermediate` added by @kbknapp on 2018-03-25 02:49_

---

_Label `W: 2.x` added by @kbknapp on 2018-03-25 02:49_

---

_Label `C: arg groups` added by @kbknapp on 2018-03-25 02:49_

---

_Comment by @kbknapp on 2018-03-25 02:49_

Thanks for reporting, I should be able to check into this and get a fix out tomorrow.

---

_Added to milestone `3.0` by @pksunkara on 2020-04-09 07:20_

---

_Label `W: 2.x` removed by @pksunkara on 2020-04-09 07:21_

---

_Comment by @CreepySkeleton on 2020-04-21 11:14_

@jrakow It's been a long tomorrow, but, as all things do, it has come to an end. 

Turns out, in the end, it was you who made a mistake: `get_matches_from` expects the name of the binary to be passed as the first argument, as `std::env::args()` does. In other words, your reproducer should have been `get_matches_from("app", "-a")`. And let me inform you: it works rather fine. Everything is working as expected, both in `master` and `2.33`.

For the record: I'd been hunting this ghost for almost an hour until a gave up and finally paid extra attention to the reproducer. Remember, maintainers: **make sure the reproducer is correct first**, otherwise you are liable to start spitting acid at your monitor, as I almost did.

---

_Closed by @CreepySkeleton on 2020-04-21 11:14_

---

_Comment by @Dylan-DPC-zz on 2020-04-21 11:24_

We are not writing a book here so can you drop the philosophy 

---

_Comment by @spacekookie on 2020-04-21 12:16_

@CreepySkeleton is there a chance you can keep the vitriol out of your comments in the future? Getting angry at reporters, issues, or code is just childish and makes the project's atmosphere a lot less pleasant to engage with for everybody. Also...please don't talk _about_ people like they're not in the room, even though they can clearly read what you're saying.

---

_Comment by @pksunkara on 2020-04-21 12:25_

I think there's some miscommunication here. From what I understand of his post, he didn't write anything angry at anyone. He was only angry at himself. Just to clarify, @spacekookie Are you saying that he shouldn't be angry at even himself when commenting?

---

_Comment by @Dylan-DPC-zz on 2020-04-21 12:25_

if someone wants to be angry at themselves, they can do it in private, dont need to rant on any issue. And reading the comment, it is clear it is not a case of a self-rant. Don't try to justify it, the intention is pretty clear on this one 

---

_Comment by @spacekookie on 2020-04-21 12:30_

@pksunkara There's a difference between saying "Oh I did a bad, couldn't figure it out" or whatever and "I wasted time because I assumed the report was correct". One is targeted at nobody, the other is clearly targeted at the reporter.

And I feel the fact that we keep having to have these conversations shows that there's a problem. This is not how Rust projects communicate and it goes against the atmosphere in the rest of the project.

---

_Comment by @pksunkara on 2020-04-21 12:33_

@spacekookie Thanks, I understand what you mean now.

---

_Comment by @CreepySkeleton on 2020-04-21 17:13_

@Dylan-DPC @spacekookie 

The sky on a sunny day is clear. Intentions rarely are.
 
If I may clarify the intention behind my comment: 

* First sentence: a slight ironic apology for the time it took us to get to the issue. A matter of politeness, nothing more, nothing less.
* Second paragraph: explanation why the report was false positive. I believe it consist of facts that everybody can check.
* Third paragraph: a note to myself (and remainder to other maintainers) that I should check correctness of the reproducer before I try to confirm the bug, not after, and also a bit of joking to put emphasis on it. Contrary to your (apparent) beliefs, it wasn't supposed to be perceived as angry or bitter, it was supposed to be read in a very neutral tone. I apologize if it made somebody out there feel uncomfortable, I honestly do.

If I worded it wrong, I would appreciate your explanation in form of "The `X` clause may be considered impolite\offensive because of \<explanation>". The explanation part is very important here because without it it's just your personal perception.

Please remember that the way things are being perceived may not to be the way they were intended to be perceived. There's absolutely no need to see me as always angry/upset person who is eager to leave a bitter comment in public; I am not, in fact. Very few of the comments I left are intended to be offensive, and none of those were left on programming forums. I'd be very thankful it you could kindly explain which parts of my comment left such a horrible impact and why they did so.

Please also keep in mind that people are different: what is perceived as an offense by one person may fly as a harmless note for another. For instance, "I wasted time because I assumed the report was correct" does sound totally neutral to me. Who made the wrong assumption? I did. Who wasted time because of the wrong assumption? I did. Whose time was wasted because of the wrong assumption? Mine. Whose to blame here? The person who harshly made the wrong assumption: me. I don't see what's wrong with the sentence, but I also understand it makes you uncomfortable somehow. This is why I'm asking you to kindly take some of your time to _explain_ it to me. 

So I ask of you: is there a chance you stop defaulting to viewing my comments as bitter or angry or childish or directed on somebody instead of what they really are? I'm ready to work on my writing style, definitely!

---
