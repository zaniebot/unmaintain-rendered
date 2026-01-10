---
number: 1125
title: Multi valued flags and required argument do not work together very well
type: issue
state: open
author: rsertelon
labels:
  - C-enhancement
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2017-12-08T23:13:17Z
updated_at: 2023-09-08T20:33:27Z
url: https://github.com/clap-rs/clap/issues/1125
synced_at: 2026-01-10T01:26:43Z
---

# Multi valued flags and required argument do not work together very well

---

_Issue opened by @rsertelon on 2017-12-08 23:13_

### Rust Version

* 1.22.1

### Affected Version of clap

* 2.29.0

The [habitat project](https://github.com/habitat-sh/habitat) uses clap for its command line parsing. There are currently two issues opened related to how clap generates help message and/or how it parses the command line arguments.

Issues are:
* https://github.com/habitat-sh/habitat/issues/2221
* https://github.com/habitat-sh/habitat/issues/3106

In the source code of habitat, here's the [interesting bit](https://github.com/habitat-sh/habitat/blob/master/components/sup/src/main.rs#L329).

The subcommand `load` has both a required `@arg PKG_IDENT_OR_ARTIFACT + required + takes_value` and a multivalued `@arg BIND: --bind +takes_value +multiple`

The problem is that the help message prints:

```
USAGE:
    hab-sup load [FLAGS] [OPTIONS] <PKG_IDENT>
```

But if you try to actually follow the help message while using the `--bind` flag and specify the `<PKG_IDENT>` as the last argument, it won't work as clap thinks it's still a `--bind` value. However, if `--bind` argument is placed as the last one, it'll work as expected.

So there's either a discrepancy in the help message, or/and a multivalue parsing that may be to greedy for when there's an expected required argument.

I understand this problem is not really trivial, I've [seen an issue](https://github.com/kbknapp/clap-rs/issues/782) that might address this, but it feels weird to have to add a terminator to the `--bind` flag from a user perspective.

Hope the problem has been made clearly, don't hesitate if you need further information!

---

_Comment by @kbknapp on 2018-01-09 16:33_

@rsertelon sorry for the long delay! Holidays and travel have had me gone for a while.

Although this isn't a trivial problem (how to tell clap when `--bind` is done and `<PKG_IDENT>` starts), there is a trivial fix *if it works with your use case*.

* Limit `--bind` to a single value *per occurrence*. So for example, `--bind value1 --bind value2` is legal, but `--bind value1 value2` is not. This is the easiest fix and my recommended solution. To do this, declare `--bind` like so: `@arg BIND: --bind +takes_value +multiple number_of_values(1)`

There are some other things you could try like `Arg::last(true)` (or `+last` for the macro version). Which will change your usage string to `hab-sup load [FLAGS] [OPTIONS] [--] <PKG_IDENT>` the downside it that it *requires* `--` to be used to access `<PKG_IDENT>`.

Finally, you could also just set a custom usage string for that one subcommand, to make it `hab-sup load [FLAGS] <PKG_IDENT> [OPTIONS]`.

---

_Label `T: RFC / question` added by @kbknapp on 2018-01-09 16:33_

---

_Comment by @kbknapp on 2018-01-09 16:35_

I have some ideas on things that could fix this...but I'm far from implementing them yet and they wouldn't probably happen until v3. I'm thinking of things like filters, where one could say, "This particular arg only accepts values that meet some criteria...if they don't fit, look at other args."

---

_Comment by @rsertelon on 2018-01-09 21:09_

Hey @kbknapp no problem! Thanks for your answer. I'll discuss this with the dev team at habitat. The workaround might be a good solution, filters might be nice too! :)

---

_Referenced in [habitat-sh/habitat#4519](../../habitat-sh/habitat/issues/4519.md) on 2018-01-30 15:58_

---

_Label `T: RFC / question` removed by @pksunkara on 2020-04-01 18:18_

---

_Label `C: args` added by @pksunkara on 2020-04-01 18:18_

---

_Label `D: hard` added by @pksunkara on 2020-04-01 18:18_

---

_Label `P3: want to have` added by @pksunkara on 2020-04-01 18:18_

---

_Label `T: new feature` added by @pksunkara on 2020-04-01 18:18_

---

_Label `W: 3.x` added by @pksunkara on 2020-04-01 18:18_

---

_Added to milestone `3.0` by @pksunkara on 2021-02-21 13:01_

---

_Label `W: 3.x` removed by @pksunkara on 2021-05-26 14:36_

---

_Removed from milestone `3.0` by @pksunkara on 2021-05-26 14:37_

---

_Referenced in [epage/clapng#83](../../epage/clapng/issues/83.md) on 2021-12-06 16:37_

---

_Label `C: args` removed by @epage on 2021-12-08 20:29_

---

_Label `A-parsing` added by @epage on 2021-12-08 20:29_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:17_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:17_

---

_Label `E-hard` removed by @epage on 2021-12-09 18:00_

---

_Label `P3: want to have` removed by @epage on 2021-12-09 18:00_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-09 18:00_

---

_Comment by @marcospb19 on 2023-09-08 01:38_

+1, I'd like to share the code I expected to succeed.

Here's the minified example for [the issue I found in `Ouch`](https://github.com/ouch-org/ouch/issues/485):

```rust
use std::{ffi::OsString, path::PathBuf};

use clap::Parser;

#[derive(Parser, Debug)]
pub enum Subcommand {
    Compress {
        #[arg(required = true)]
        files: Vec<PathBuf>,

        #[arg(required = true)]
        output: PathBuf,

        #[arg(short, long)]
        format: Option<OsString>,
    },
}

fn main() {
    let inputs = [
        "ouch compress a b c output --format tar.gz",
        "ouch compress a b c --format tar.gz output",
        "ouch compress a b --format tar.gz c output",
        "ouch compress a --format tar.gz b c output",
        "ouch compress --format tar.gz a b c output",
    ];
    for input in inputs {
        let result = Subcommand::try_parse_from(input.split_whitespace());
        let result = result.map(|_| ()).map_err(|_| ());
        println!("{input}, {result:?}");
    }
}
```

I was expecting it all to be `Ok(())`, but instead some `Err(())`s:

```rust
"ouch compress a b c output --format tar.gz", Ok(())
"ouch compress a b c --format tar.gz output", Err(())
"ouch compress a b --format tar.gz c output", Err(())
"ouch compress a --format tar.gz b c output", Err(())
"ouch compress --format tar.gz a b c output", Ok(())
```

---

To my surprise, the exact same applies for flags that don't take a value:

```rust
"ouch compress a b c output --verbose", Ok(())
"ouch compress a b c --verbose output", Err(())
"ouch compress a b --verbose c output", Err(())
"ouch compress a --verbose b c output", Err(())
"ouch compress --verbose a b c output", Ok(())
```

Is there a way around this?

(I'd consider this to be `C-bug` instead of `C-enhancement` :thinking: but that's just because I expect CLI tools in general to extract flags out of the way of positional arguments, so you never have to worry where you insert a flag)

---

_Referenced in [ouch-org/ouch#485](../../ouch-org/ouch/issues/485.md) on 2023-09-08 01:39_

---

_Comment by @epage on 2023-09-08 02:06_

@marcospb19 I suspect your case's root cause is independent of this issue and I'd recommend making your own and following the template.

---

_Comment by @marcospb19 on 2023-09-08 20:33_

:+1: I made the example smaller and created #5115.

---

_Referenced in [clap-rs/clap#5361](../../clap-rs/clap/issues/5361.md) on 2024-02-17 12:43_

---
