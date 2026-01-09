---
number: 3268
title: Deprecated structopt macro argument
type: issue
state: closed
author: 1Dragoon
labels:
  - A-derive
  - S-triage
assignees: []
created_at: 2022-01-07T20:18:26Z
updated_at: 2022-01-11T18:21:08Z
url: https://github.com/clap-rs/clap/issues/3268
synced_at: 2026-01-07T13:12:19-06:00
---

# Deprecated structopt macro argument

---

_Issue opened by @1Dragoon on 2022-01-07 20:18_

It looks like 'settings =' has been deprecated. What is the correct way to do this now?

<a href="https://github.com/DominikRusso"><img src="https://avatars.githubusercontent.com/u/50487108?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [DominikRusso](https://github.com/DominikRusso)**
_Thursday Nov 04, 2021 at 23:01 GMT_

----

Try `#[structopt(settings = &[structopt::clap::AppSettings::AllowNegativeNumbers])]` on a top level field.

For example:
```
use structopt::clap::AppSettings;
use structopt::StructOpt;

#[derive(StructOpt, Debug)]
pub enum Subcommand {
    // ...
    
    #[structopt(alias = "bri", settings = &[AppSettings::AllowNegativeNumbers])]
    Brightness {
        brightness: String,
        lights: Vec<String>,
    },

    // ...
}
```

_Originally posted by @epage in https://github.com/clap-rs/clap/issues/3134#issuecomment-990017381_

---

_Comment by @epage on 2022-01-07 20:31_

`App::settings(&[a, b])` has been replaced by `App::setting(a | b)`.  You can check the [docs for the method the attribute maps to to see this](https://docs.rs/clap/latest/clap/struct.App.html#method.settings).

I would have expected a deprecation message to have appeared for this that would have explained this.  I know we mask some deprecated warnings but I've seen a lot of others get through.

---

_Label `S-triage` added by @epage on 2022-01-07 20:31_

---

_Comment by @1Dragoon on 2022-01-08 15:41_

It did appear but I'm not sure how it is saying to implement that in the macro form, only in the non-macro form. I tried removing the slice notation from the parameter, but it doesn't seem to work.

---

_Comment by @epage on 2022-01-10 14:44_

Could you provide a code sample of what isn't working?

The [derive reference](https://github.com/clap-rs/clap/tree/master/examples/derive_ref#raw-attributes) talks some about the relationship between builder functions and attributes.  This is an important topic since its what makes the full power of clap available to the derive.  If you have any questions on it or suggestions for improvement, feel free to share

---

_Comment by @1Dragoon on 2022-01-11 15:09_

Sure, so basically this is what I am trying to do:

```rust
#[derive(Debug, Parser)]
#[clap(name = "Synth", about = "Generate random attributes.", settings = &[AppSettings::AllowNegativeNumbers])]
struct Opt {
    /// Source identity name
    source: String,

    /// Target name
    target: String,

    /// Override the date to today +/- n days
    #[clap(short, long)]
    offset: Option<i64>,
}
```

I per the doc I tried revising it to this:

```rust
#[clap(name = "Synth", about = "Generate random attributes.", settings = AppSettings::AllowNegativeNumbers)]
```

Doesn't appear to work though.

EDIT:

Just noticed it has to be changed to this:

```rust
#[clap(name = "Synth", about = "Generate random attributes.", setting = AppSettings::AllowNegativeNumbers)]
```

(i.e. "settings" becomes "setting" AND you remove the slice notation)

---

_Comment by @epage on 2022-01-11 15:13_

Looks like this works:
```rust
use clap::AppSettings;
use clap::Parser;

#[derive(Debug, Parser)]
#[clap(name = "Synth", about = "Generate random attributes.", setting = AppSettings::AllowNegativeNumbers)]
struct Opt {
    /// Source identity name
    source: String,

    /// Target name
    target: String,

    /// Override the date to today +/- n days
    #[clap(short, long)]
    offset: Option<i64>,
}

fn main() {
    let app = Opt::parse();
    println!("{:?}", app);
}
```
The distinction between `setting` and `settings` in the deprecation message's suggestion is subtle and easy to miss.

---

_Comment by @1Dragoon on 2022-01-11 15:15_

ah yep just noticed that right after I posted it, thanks though :) And if anybody happens to come along this by googling it in the future, multiple settings looks like this:

```rust
#[clap(name = "Synth", about = "Generate random attributes.", setting = AppSettings::AllowNegativeNumbers | AppSettings::AllArgsOverrideSelf )]
```


---

_Comment by @epage on 2022-01-11 15:30_

Glad you got it working!

---

_Closed by @epage on 2022-01-11 15:30_

---

_Label `A-derive` added by @epage on 2022-01-11 18:21_

---
