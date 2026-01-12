```yaml
number: 1694
title: Specify defaults in terms of the underlying type rather than strings
type: issue
state: closed
author: emilazy
labels:
  - A-derive
assignees: []
created_at: 2020-02-14T16:50:54Z
updated_at: 2021-08-13T18:36:33Z
url: https://github.com/clap-rs/clap/issues/1694
synced_at: 2026-01-12T16:14:11Z
```

# Specify defaults in terms of the underlying type rather than strings

---

_@emilazy_

I really like how the new clap v3 is shaping up! Now that structopt is integrated, it'd be great if defaults could be specified in terms of the default resulting value they produce rather than as a string. Other argument parsing libraries like Python's argparse work this way.

---

_Label `T: new feature` added by @emilazy on 2020-02-14 16:50_

---

_Comment by @pksunkara on 2020-02-14 16:51_

Could you provide a small example so we are on the same page? Thanks

---

_Comment by @emilazy on 2020-02-14 16:53_

e.g.

```rust
enum Switch {
    Magic,
    MoreMagic,
}

impl FromStr for Switch {
    // ...
}

#[derive(Clap)]
struct Opts {
    #[clap(default_value(Switch::MoreMagic))]
    switch: Switch,
}
```

Currently you have to do e.g. `#[clap(default_value("more-magic"))]`, which is less flexible and elegant (as well as incurring an unnecessary parse).

---

_Added to milestone `3.0` by @pksunkara on 2020-02-14 16:54_

---

_Label `C: derive macros` added by @pksunkara on 2020-02-14 16:54_

---

_Referenced in [clap-rs/clap#1695](../../clap-rs/clap/issues/1695.md) on 2020-02-14 17:05_

---

_Label `C: arg enums` added by @pksunkara on 2020-04-12 21:01_

---

_Comment by @kbknapp on 2020-04-29 03:02_

If your `enum` implements `Default` you can simply use `#[clap(default_value)]` on that field.

Full example:

```rust
use std::fmt;

use clap::Clap;

#[derive(Clap, Debug, PartialEq, Copy, Clone)]
enum Magic {
    Little,
    Lots,
    None,
}

impl Default for Magic {
    fn default() -> Self {
        Magic::Little
    }
}

impl fmt::Display for Magic {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "{:?}", self)
    }
}

#[derive(Clap)]
struct Ctx {
    /// How much magic do you want?
    #[clap(long, arg_enum, default_value)]
    magic: Magic,
}

fn main() {
    let ctx = Ctx::parse();
}
```

---

_Comment by @kbknapp on 2020-04-29 03:06_

@emilazy it's not 100% what you asked for, but in your example it's at least part of the way there. I need to re-familiarize myself with the derive macros we made, but I think the underlying issue is that it just calls `Arg::default_value` underneath which only accepts a string. Now since we're already doing it where values have a valid default, we may be able to extend that case more generally...but I'm not 100% certain how much of a re-work that would entail.

---

_Comment by @pksunkara on 2020-04-29 07:51_

Implementation wise, this would just adding one more method to the ArgEnum trait.

---

_Comment by @kbknapp on 2020-04-29 13:21_

We would also potentially need to impl `Display` for the enum as well...the downside being that prevents the consumer from doing this if they had some specialized version of `Display` they wanted to use.

---

_Comment by @mainrs on 2020-07-17 22:25_

This would still be useful for cases where the underlying default of the application differs from the default of the type:

```rust
use clap::Clap;
use std::env;
use std::path::PathBuf;

#[derive(Clap, Debug)]
#[clap(author, version)]
pub struct App {
    #[clap(name = "REPO", default = "default_repo_path", parse(from_os_str))]
    pub repo_path: PathBuf,
}

fn default_repo_path() -> PathBuf {
    env::current_dir().expect("Failed to find current working directory!")
}
```

This is currently IIRC not possible. The default of `PathBuf` isn't useful here.

---

_Comment by @Lunderberg on 2021-04-19 02:04_

A similar change would also be useful for `required_if`'s interaction with case insensitive arguments.  Currently, I want to make a case-insensitive enum argument, where some of the enum values would also have additional required arguments.  I have this set up as below, which works in most cases.

```rust
use clap::{arg_enum, App, Arg};                                                                                                                                      
                                                                                                                                                                     
arg_enum! {                                                                                                                                                          
    #[derive(Debug, PartialEq)]                                                                                                                                      
    enum EnumOpt {                                                                                                                                                   
        OptionA,                                                                                                                                                     
        OptionB,                                                                                                                                                     
    }                                                                                                                                                                
}                                                                                                                                                                    
                                                                                                                                                                     
fn main() {                                                                                                                                                          
    let matches = App::new("test")                                                                                                                                   
        .arg(                                                                                                                                                        
            Arg::with_name("enum")                                                                                                                                   
                .long("enum")                                                                                                                                        
                .takes_value(true)                                                                                                                                   
                .required(true)                                                                                                                                                      .possible_values(&EnumOpt::variants())                                                                                                               
                .case_insensitive(true),                                                                                                                             
        )                                                                                                                                                            
        .arg(                                                                                                                                                        
            Arg::with_name("dependent")                                                                                                                              
                .long("dependent")                                                                                                                                   
                .required_if("enum", "OptionB"),                                                                                                                     
        )                                                                                                                                                            
        .get_matches();                                                                                                                                              
    println!("Matches = {:?}", matches);                                                                                                                             
} 
```

If this is run as `test --enum OptionA`, I get the expected behavior, an error message that the `--dependent` flag is also required.  However, if this is run as `test --enum optiona`, the enum value is correctly set, but there is no error message for the missing `--dependent` flag.  Specifying `required_if` in terms of the underlying type would resolve this issue.

---

_Comment by @epage on 2021-07-21 20:03_

#2612 adds `ArgEnum::as_arg(&self) -> &'static str` to make it easier for end-users to implement `Display` so they can use `clap(arg_enum, default_value)`.  I've not yet gone through the effort of providing a `Display` or `Default` macro since there is probably more to be decided there.

---

_Comment by @epage on 2021-07-21 21:39_

Also wanted to add

> Currently you have to do e.g. #[clap(default_value("more-magic"))], which is less flexible and elegant (as well as incurring an unnecessary parse).

We'll do that extra parsing anyways because we store the default in `App` and then pull it back out from `ArgMatches`, relying completely on the build APIs machinery to do things for us, like generate help text with the default.

---

_Comment by @epage on 2021-07-22 14:22_

Another way of solving this problem is we change the attributes to:
- `default_value`: raw method that directly maps to `Arg::default_value`
- `default_value_t`: takes in a type, defaults to `Default::default`

StructOpt porting work
- If using `default_value`, change to `default_value_t`
- If using `default_value="foo"`, no change needed

Benefits
- Keeps to the pattern of exposing the raw methods as-is (`default_value` just forwards to `Arg::default_value`)
- Keeps to the pattern of typed stuff having a `_t` suffix (e.g. `ArgMatches::value_of_t`)
- Makes magic method (non-raw) less magical by not having it do either typed or string, but instead each is strictly typed or string

---

_Referenced in [clap-rs/clap#2635](../../clap-rs/clap/pulls/2635.md) on 2021-07-28 15:29_

---

_Removed from milestone `3.0` by @pksunkara on 2021-08-09 01:34_

---

_Added to milestone `3.1` by @pksunkara on 2021-08-09 01:34_

---

_Closed by @pksunkara on 2021-08-13 18:36_

---
