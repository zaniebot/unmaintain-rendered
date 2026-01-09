---
number: 2675
title: "Add support for the trait From<regex::RegexSet> to Arg"
type: issue
state: closed
author: PandH4cker
labels:
  - A-validators
assignees: []
created_at: 2021-08-10T11:07:34Z
updated_at: 2022-05-23T20:22:57Z
url: https://github.com/clap-rs/clap/issues/2675
synced_at: 2026-01-07T13:12:19-06:00
---

# Add support for the trait From<regex::RegexSet> to Arg

---

_Issue opened by @PandH4cker on 2021-08-10 11:07_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-beta.2

### Describe your use case

It would be nice that clap handle not only a single RegEx in the _validator_regex() function_ but a RegexSet.
RegexSet are useful to combine multiple regex together and check if any of the regex provided match the string to test.

### What I standed for BEFORE _validator_regex()_:
This is what I did in conjunction to _validator() function_.
```rust
use lazy_static::lazy_static;
use regex::Regex;

pub fn is_hosts(v: &str) -> Result<(), String> {
    lazy_static! {
        static ref HOSTNAME_REGEX: Regex = Regex::new(r"(?x)
            ^([a-zA-Z0-9]|[a-zA-Z0-9][a-zA-Z0-9\-]{0,61}[a-zA-Z0-9])
            (\.([a-zA-Z0-9]|[a-zA-Z0-9][a-zA-Z0-9\-]{0,61}[a-zA-Z0-9]))*$
        ").unwrap();
        static ref IP_REGEX: Regex = Regex::new(r"(?x)
            ^((\d|[1-9]\d|1\d{2}|2[0-4]\d|25[0-5])\.){3}
            (\d|[1-9]\d|1\d{2}|2[0-4]\d|25[0-5])$
        ").unwrap();
        static ref NETINT_REGEX: Regex = Regex::new(r"(?x)
            ^((\d|[1-9]\d|1\d{2}|2[0-4]\d|25[0-5])\.){3}
            (\d|[1-9]\d|1\d{2}|2[0-4]\d|25[0-5])/([1-9]|[12]\d|3[012])$
        ").unwrap();
    }
    match HOSTNAME_REGEX.is_match(v) || IP_REGEX.is_match(v) || NETINT_REGEX.is_match(v) {
        true => {
            Ok(())
        }
        false => {
            Err(format!("{} isn't a host nor an ip nor a network interface", v))
        }
    }
}
```

### What I standed for AFTER _validator_regex()_:

So I would like to do this kind of thing:
```rust
//In the module that generate the App
App::new("...")
// [SNIP...]
      .arg(
            Arg::new(target::NAME)
                .last(true)
                .takes_value(true)
                .value_delimiter(" ")
                .multiple(true)
                //.validator(validators::net::is_hosts)
                .validator_regex(HOST_REGEXSET, "only hostnames, ip, network intefaces are allowed")
                .required_unless_present_any(&[input_list::NAME, input_random::NAME])
                .display_order(1)
     )
```

```rust
//In a module of constants variables
use regex::RegexSet;

const HOSTNAME_REGEX_STR : &str = r"(?x)
            ^([a-zA-Z0-9]|[a-zA-Z0-9][a-zA-Z0-9\-]{0,61}[a-zA-Z0-9])
            (\.([a-zA-Z0-9]|[a-zA-Z0-9][a-zA-Z0-9\-]{0,61}[a-zA-Z0-9]))*$
";
const IP_REGEX_STR : &str = r"(?x)
            ^((\d|[1-9]\d|1\d{2}|2[0-4]\d|25[0-5])\.){3}
            (\d|[1-9]\d|1\d{2}|2[0-4]\d|25[0-5])$
";
const NETINT_REGEX_STR : &str = r"(?x)
            ^((\d|[1-9]\d|1\d{2}|2[0-4]\d|25[0-5])\.){3}
            (\d|[1-9]\d|1\d{2}|2[0-4]\d|25[0-5])/([1-9]|[12]\d|3[012])$
";
pub const HOST_REGEXSET : RegexSet = RegexSet::new(&[
    HOSTNAME_REGEX_STR,
    IP_REGEX_STR,
    NETINT_REGEX_STR
]).unwrap();
```

### Describe the solution you'd like

I would like a function or an implementation to **validate the RegexSet** like the _validator_regex()_:

```rust
impl<'a> From<RegexSet> for RegexRef<'a> {
    fn from(r: RegexSet) -> Self {
        RegexRef(Cow::Owned(r))
    }
}
```

But RegexRef take a Cow<'a, Regex>, maybe you should refactor this to have RegexSet instead ?

### Alternatives, if applicable

The solution I standed before the _validator_regex()_ works fine, I would like to know:

_Which one of these both solutions are the most effective ?_

Since I am not figuring out yet : _How could I use the lazy_static to improve the effectiveness of the regex compilation combining to store it in a constant variable and be reusable further in the code ?_

So until there is not an implementation for handling RegexSet I would use the validator and my match pattern 😄 .

### Additional Context

_No response_

---

_Label `T: new feature` added by @PandH4cker on 2021-08-10 11:07_

---

_Label `C: validators` added by @pksunkara on 2021-08-10 23:00_

---

_Added to milestone `3.0` by @pksunkara on 2021-08-10 23:00_

---

_Removed from milestone `3.0` by @epage on 2021-10-16 18:10_

---

_Added to milestone `3.1` by @epage on 2021-10-16 18:10_

---

_Referenced in [clap-rs/clap#2899](../../clap-rs/clap/pulls/2899.md) on 2021-10-16 21:06_

---

_Closed by @bors[bot] on 2021-10-16 22:47_

---

_Comment by @epage on 2022-05-23 20:22_

FYI I am considering removing `validator_regex`, see #3743

---
