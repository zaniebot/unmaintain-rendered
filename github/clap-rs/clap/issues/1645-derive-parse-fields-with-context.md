---
number: 1645
title: "[Derive] Parse fields with context"
type: issue
state: closed
author: Ayplow
labels: []
assignees: []
created_at: 2020-01-26T19:53:56Z
updated_at: 2020-02-10T12:32:18Z
url: https://github.com/clap-rs/clap/issues/1645
synced_at: 2026-01-07T13:12:19-06:00
---

# [Derive] Parse fields with context

---

_Issue opened by @Ayplow on 2020-01-26 19:53_

Currently, the `Clap` macro deserializes options with `FromStr`. While fine for simple use cases, this doesn't provide the parser the context that the option came from, and precludes common patterns that make use of this information.

The most common is short options; Without knowing whether an value belongs to a long `--type` or short `-t` option, we can't know its' correct representation.

AFAICT this could be solved simply with a new enum, but the design should be given thought.

> This raises the question of whether the presence of `=` should also be part of the context, but I'm not aware of any use case for it

```rust
use myclap::*;
mod myclap {
  pub enum Opt<'a> {
    Short(char),
    Long(&'a str)
  }
  use std::fmt;
  impl fmt::Display for Opt<'_> {
      fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
          match self {
              Self::Short(chr) => write!(f, "{}", chr),
              Self::Long(s) => write!(f, "{}", s),
          }
      }
  }
  pub trait FromOpt: Sized {
    type Err;
    fn from_opt(opt: Opt) -> Result<Self, Self::Err>;
  }
}

enum Type {
  Frizzy,
  Floaty
}
impl FromOpt for Type {
  type Err = String;
  fn from_opt(opt: Opt) -> Result<Self, Self::Err> {
    match opt {
      Opt::Long("frizzy") | Opt::Short('z') => Ok(Self::Frizzy),
      Opt::Long("floaty") | Opt::Short('f') => Ok(Self::Floaty),
      _ => Err(format!("Unrecognised type '{}'", opt))
    }
  }
}
#[derive(Clap)]
struct Opts {
  #[clap(short, long, parse(from_opt))]
  like: Type
}
```

---

_Comment by @pksunkara on 2020-01-30 19:33_

@CreepySkeleton, Would like your feedback here.

---

_Comment by @CreepySkeleton on 2020-01-30 23:25_

> Without knowing whether an value belongs to a long --type or short -t option, we can't know its' correct representation.

Could you please elaborate on this? I'm pretty sure it's simply impossible to distinguish between `-f` and `--foo` in clap, and I also think there's no need for such a thing. Short options are supposed to be nothing but shortcuts for long options, and if they aren't, they should be *different* options.



---

_Comment by @CreepySkeleton on 2020-02-01 10:13_

I'm closing this since I really don't think that distinguishing between short and long options is a good idea. Feel free to reopen if you have a concrete use case that absolutely requires it and can't be solved by other means (like creating separate options).

---

_Closed by @CreepySkeleton on 2020-02-01 10:13_

---

_Label `W: wont do` added by @pksunkara on 2020-02-01 10:14_

---

_Label `C: args` added by @pksunkara on 2020-02-01 10:14_

---

_Label `T: RFC / question` added by @pksunkara on 2020-02-01 10:15_

---

_Comment by @Ayplow on 2020-02-10 05:53_

As an example, here's an option from the `readelf` cli -

```
  -w[lLiaprmfFsoRtUuTgAckK] or
  --debug-dump[=rawline,=decodedline,=info,=abbrev,=pubnames,=aranges,=macro,=frames,
               =frames-interp,=str,=loc,=Ranges,=pubtypes,
               =gdb_index,=trace_info,=trace_abbrev,=trace_aranges,
               =addr,=cu_index,=links,=follow-links]
```

For the short form of the option, the value also has a 'short' representation. As a user, this seems intuitive - I use short options when I want to quickly use a feature of the CLI, and needing to write out the full value defeats that.

I think you're right that separate flags would be ideal, but that only leaves us with 26 flags 😄(realistically less, we don't want to use the whole alphabet)   

Today, this could be implemented with clap by ignoring the context and matching the short and long forms. But this *is* a workaround, and leaves the door open for user error.

> It's also worth noting that limiting the `-w` option to single characters means `readelf` can use the same 'grouping' mechanism used for flags - `-wprmf` enables all four modes.

That said, this is quite niche and one could argue that a CLI with so many flags is badly designed. After some thought there also isn't any clear space for it in clap's API, and in hindsight I think it's probably out of scope

---

_Comment by @pksunkara on 2020-02-10 09:53_

Hmm.. We can use utf8 for short flags to increase the range. 💭 

---

_Comment by @Ayplow on 2020-02-10 09:58_

Just in case you're not joking, that sounds terrible for UX. You'd either have to copy the scalar or learn its code to manually type into your terminal. Even if we don't care about terminal support, no keyboards have these keys.

---

_Comment by @pksunkara on 2020-02-10 10:00_

😄 

You can currently use 52 characters for sure for short flags (both uppercase and lowercase).

---

_Comment by @Ayplow on 2020-02-10 10:03_

At best we have 52 characters available - the upper- and lowercase alphabet - but it's important that the flags are memorable, and using the same letter makes that even harder. By breaking up the grouping into meaningful sections, users are given prompts for remembering the tool they want to use

---

_Comment by @CreepySkeleton on 2020-02-10 12:31_

@Ayplow If I'm not mistaken (and I very well can be), this design can be easily implemented with only two args:
```rust
use clap::{Arg, App};

fn validate_short(val: String) -> Result<(), String> {
    let valid = "lLiaprmfFsoR";

    for ch in val.chars() {
        if !valid.contains(ch) {
            return Err(format!("valid characters are `{}`", valid))
        }
    }

    Ok(())
}

fn main() {
    let matches = App::new("MyApp")
    .arg(
        Arg::with_name("short-w")
            .short("w")
            .takes_value(true)
            .validator(validate_short)
    )
    .arg(
        Arg::with_name("long-w")
            .long("debug-dump")
            .takes_value(true)
            .possible_values(&[
                "rawline",
                "decodedline",
                "info",
                "abbrev",
                "pubnames",
                "aranges",
                "macro",
                "frames",
                "frames-interp",
                "str",
                "loc",
                "Ranges",
            ])
    )
    .get_matches();

    println!("{:?}", matches);
}
```

No need to introduce tons of flags ¯\\_(ツ)_/¯

---
