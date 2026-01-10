---
number: 459
title: Add support for using enums in place of strings
type: issue
state: closed
author: hgrecco
labels: []
assignees: []
created_at: 2016-03-26T15:29:50Z
updated_at: 2018-07-22T02:41:02Z
url: https://github.com/clap-rs/clap/issues/459
synced_at: 2026-01-10T01:26:29Z
---

# Add support for using enums in place of strings

---

_Issue opened by @hgrecco on 2016-03-26 15:29_

I have been using `clap-rs` for a while now and I find it very useful. The only annoying thing is that most things are `stringly typed`. For example, notice how we need to check for strings in the following example:

``` rust
enum Mode {
    A,
    B,
    C,
}

let matches = App::new("example")
                        .arg(Arg::with_name("ModeA")
                                    .short("a"))
                        .arg(Arg::with_name("ModeB")
                                    .short("b"))
                        .arg(Arg::with_name("ModeC")
                                    .short("c"))
                        .group(ArgGroup::with_name("mode")
                                            .required(true)
                                            .args(&["ModeA", "ModeB", "ModeC"]))
                        .get_matches();

    let mode = if matches.is_present("ModeA") {
        Mode::A
    } else if matches.is_present("ModeB") {
        Mode::B
    } else {
        Mode::C
    };

    // Do something
```

Here we are using strings not only in the part related to the arguments but also to deal with them afterwards. I would like to keep all the strings (which are prone to errors) in one part. I think there is no way, right?

I would like something like (syntax is debatable):

``` rust
let matches = App::new("example")
                        .arg(Arg::with_enum_value(Mode::A)
                                    .short("a"))
                        .arg(Arg::with_enum_value(Mode::B)
                                    .short("b"))
                        .arg(Arg::with_enum_value(Mode::C)
                                    .short("c"))
                        .group(ArgGroup::with_name("mode")
                                            .required(true)
                                            .enum(Mode)
                        .get_matches();

    let mode =  matches.value_of('mode');

    // Do something
```

Where the `enum_args` check that all enum values exhausted and `matches.value_of('mode')` returns the chosen enum value (not a string). If you do not want all values as arguments, you could do something like:

``` rust
        .enum_values(&[Mode::A, Mode::B])
```

Something similar could be done for subcommands.


---

_Comment by @kbknapp on 2016-03-26 16:32_

This is a problem ("stringly typed") I've been thinking about a lot, and have always leaned towards an auto serialized version to solve it although wasn't sure of a good way to do this.

Your way, using enums is a very cool idea that I hadn't thought of! On top of that, it could absolutely be added without breaking backwards compatibility. 

I'm really excited to try this out and see how it plays out. My free time is a little limited over the next week, but I'll test out some ideas and see how it all plays out. I'll post back here for discussion on the topic.

Thanks for submitting this :+1: 


---

_Label `T: new feature` added by @kbknapp on 2016-03-26 16:33_

---

_Label `D: intermediate` added by @kbknapp on 2016-03-26 16:33_

---

_Label `P3: want to have` added by @kbknapp on 2016-03-26 16:33_

---

_Label `W: 2.x` added by @kbknapp on 2016-03-26 16:33_

---

_Renamed from "Using Enums all over" to "Add support for using enums in place of strings" by @kbknapp on 2016-03-26 16:33_

---

_Assigned to @kbknapp by @kbknapp on 2016-03-26 16:34_

---

_Comment by @joshtriplett on 2016-03-27 03:42_

I'm quite interested in this as well.  In particular, when I have a `matches m.subcommand()` block, I'd like rustc to tell me if I've added a new subcommand but failed to add the corresponding implementation.  If the subcommands correspond to an enum, then rustc's existing warning for non-exhaustive matches of an enum will handle that.


---

_Comment by @kbknapp on 2016-03-27 04:28_

I think I can have an initial implementation of this done tomorrow if all goes as planned. I've played with some simple ideas and really like how it works thus far. 

The hard part will be doing this in a way that isn't breaking if it changes the `ArgMatches` struct (i.e. `ArgMatches<'a>` -> `ArgMatches<'a, A>` is technically breaking. But I have some ideas for a way around this where all existing code should work as is...worse case scenario I bump to `clap` v3 as this is a big enough win that I'm willing to do so for all the reasons @joshtriplett and @hgrecco have mentioned, along with other such instances adding args, or changing args, etc (i.e. all the downsides of "stringly typed" things).


---

_Comment by @joshtriplett on 2016-03-27 04:57_

@kbknapp Sounds awesome!

In the same spirit as docopt's macro version, it'd be nice if clap could write the enum for me: I write the subcommands, and clap gives me back a freshly minted enum of subcommands.  Ditto for argument groups as @hgrecco suggested.  That said, this would be great even if I have to write the enum.


---

_Added to milestone `2.3` by @kbknapp on 2016-03-27 20:04_

---

_Comment by @kbknapp on 2016-03-28 02:57_

Good news, I've got this working with subcommands thus far. Next to implement Args and should be just as straight forward. 

Anywhere you used to use a string to access a subcommand, you can now use a enum variant if it implements the proper traits. Or you can use a macro implement said traits automatically.

You can see #465 for details, but this is what it looks like:

``` rust
#[macro_use]
extern crate clap;
use clap::{App, SubCommand};

// Note lowercase variants, the subcommand will be exactly as typed here 
// (because of this, cannot contain hyphens, or rust keywords...for those see below)
subcommands!{
    enum MyProg {
        show,
        delete,
        make
    }
}

// Alternatively, if you wish to have variants which display
// differently, contain hyphens ("-"), or use Rust keywords, one can use this variation of
// the macro
subcommands!{
    enum MyProgAlt {
        Show => "show",
        Delete => "delete",
        DoStuff => "do-stuff"
    }
}

fn main() {
    let m = App::new("myprog")
        .subcommand(SubCommand::with_name(MyProg::show))
        .subcommand(SubCommand::with_name(MyProg::delete))
        .subcommand(SubCommand::with_name(MyProg::make))
        .get_matches();

    match m.subcommand() {
        (MyProg::show, _) => println!("'myprog show' was used"),
        (MyProg::delete, _) => println!("'myprog delete' was used"),
        (MyProg::make, _) => println!("'myprog make' was used"),
        (MyProg::None, _) => println!("No subcommand was used"), // The "None" variant is automatically added to denote "No subcommand used"
    }
}
```

The macro does a good bit, but it can all be done manually if one so chooses. Things one would need to do in order to use an enum as the name
- Implement `clapp::SubCommandKey` which defines two methods used internally by `clap`. One to denote the "none" variant, and one to convert from a "&str" to the enum cheaply.
- Implement `std::convert::AsRef<str>` which does the opposite of enum->&str conversion

The macro actually does a little more than that, but it's not required. The macro also:
- defines a `variants()` function which returns an array of `&'static str`s which contains the variant names
- Implements `std::fmt::Display` for each variant
- Adds the `None` variant.

I expect `Args` to similar and should be done soon. Once all is good, docs are updated, examples added, etc. I'll merge the PR.

This _does_ contain a _very slight_ breaking change that should affect very few in the wild, only require a single line change. So I'm debating just doing a bump to 2.3 and giving fair notice to anyone it would affect.


---

_Comment by @joshtriplett on 2016-03-28 03:56_

Nice!  That looks really promising.

A few things that jump out at me:

Rather than adding a None variant, might it make sense to return an Option instead?  Then, subcommand_name or subcommand can return Some(show) or None.  (Likely possible manually via the trait, but defaults matter, and it seems odd to add a None to an enum rather than wrapping it in Option.  Plus, Option has a pile of existing methods and helper functions.)

Also, could `clap_app!` and `clap_yaml!` automatically generate the enum?


---

_Comment by @kbknapp on 2016-03-28 15:05_

@joshtriplett 

> Rather than adding a None variant, might it make sense to return an Option instead? 

`subcommand_name()` returns an option, so the `None` value isn't really possible from there. It will return `Option::None` if no subcommand was used. Unfortunately `subcommand()` returns a tuple for convenience sake, and changing to an `Option` would be a much larger breaking change.

The more I think about it though, the more I kind of just want to bump to 3x and it correctly. i.e. `Option` the wrap the tuple, and remove the `Option` from the `ArgMatches` since it can't really ever be a `Option::None` value. And remove the `None` variant from the subcommand. We'll see....

> could clap_app! and clap_yaml! automatically generate the enum?

I don't think so. I mean they _technically_ could, but the enum would be limited to the scope of the function they're declared in so the usefulness is somewhat limited.


---

_Comment by @hgrecco on 2016-03-29 00:00_

I agree with @kbknapp about the enum and the version. 

About the enum, I think that explicit is better than implicit (Sorry about my python bias!).

Regarding the version,  My opinion is that even if the new version was fully backwards compatible, bumping to 3x is the right thing to do. This is a powerful new feature which will change the way we build big apps (and even small ones). Using enums will finally allow us to put all the strings in just one part (when we say that, eg, `.short("c")`) bringing Rust safety to argument parsing. Notice that `clap` already provides a lot of safety that is more annoying to implement by hand (for example with `required`). This feature closes the last hole.


---

_Label `W: 3.x` added by @kbknapp on 2016-03-29 01:27_

---

_Label `W: 2.x` removed by @kbknapp on 2016-03-29 01:27_

---

_Added to milestone `3.0` by @kbknapp on 2016-03-29 01:27_

---

_Removed from milestone `2.3` by @kbknapp on 2016-03-29 01:27_

---

_Comment by @joshtriplett on 2016-06-09 19:56_

In the same spirit of attempting to provide more structured types, would it make sense to support (but not require) filling in an argument structure?  For instance:

``` rust
subcommands!{
    enum MyProg {
        log { verbose: bool, patch: bool, branch: String },
        diff { format: DiffFormat, context: usize, stat: bool, from: String },
    }
}
```

`subcommands!` could then parse the parameters and fill in the structure.

(It might make sense to write the structure and generate the options from that, or it might make sense to write the options and let clap generate the structure; the latter would make it easier to provide help, short names, and other data.  It might also make sense to have separate structures, and in any case to have a separate structure for the top-level non-subcommand options.  Either way, though, I'd love to have structured parameters.)


---

_Comment by @kbknapp on 2016-12-27 04:34_

Waiting on Macros 1.1 in order to just use a `custom_derive`

---

_Referenced in [clap-rs/clap#818](../../clap-rs/clap/issues/818.md) on 2017-01-18 14:13_

---

_Referenced in [clap-rs/clap#817](../../clap-rs/clap/issues/817.md) on 2017-01-30 04:50_

---

_Comment by @drusellers on 2017-05-02 12:31_

Stoked to see this coming along. Type safety FTW

---

_Comment by @WaDelma on 2017-12-06 20:58_

I would also be happy to have this. Custom derives are even stable now!

---

_Comment by @kbknapp on 2017-12-06 23:13_

See kbknapp/clap-derive#6 for related discussion

---

_Removed from milestone `3.0` by @kbknapp on 2018-02-02 01:51_

---

_Added to milestone `v3-alpha1` by @kbknapp on 2018-02-02 01:51_

---

_Comment by @kbknapp on 2018-07-22 02:41_

Closing this issue in favor of #1104 

This issue still serves as good back-reading though for the problem.

---

_Closed by @kbknapp on 2018-07-22 02:41_

---

_Referenced in [clap-rs/clap_derive#23](../../clap-rs/clap_derive/pulls/23.md) on 2019-12-20 19:35_

---

_Referenced in [drogue-iot/drg#4](../../drogue-iot/drg/pulls/4.md) on 2021-03-17 05:27_

---
