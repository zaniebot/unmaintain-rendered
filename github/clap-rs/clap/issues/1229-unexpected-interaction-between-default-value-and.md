---
number: 1229
title: Unexpected interaction between .default_value() and .required_unless_one()
type: issue
state: closed
author: longborough
labels: []
assignees: []
created_at: 2018-03-23T16:25:54Z
updated_at: 2018-08-02T03:30:21Z
url: https://github.com/clap-rs/clap/issues/1229
synced_at: 2026-01-07T13:12:19-06:00
---

# Unexpected interaction between .default_value() and .required_unless_one()

---

_Issue opened by @longborough on 2018-03-23 16:25_

Hi. Thank you for clap. I'm a complete Rust newbie, and struggling; clap made things a lot easier and I appreciate it!


### Rust Version 

* rustc 1.26.0-nightly (a04b88d19 2018-03-19)

### Affected Version of clap

* name = "clap"
* version = "2.31.2"

### Expected Behavior Summary

When a default_value is provided for an Arg and the Arg is required unless another Arg is present, then I think the absence of both should be signalled as  the message:

error: The following required arguments were not provided


### Actual Behavior Summary

Currently, if an Arg has a default_value, the required_unless_one() option is ignored, and the default value always provided.

The behaviour also occurs when the multiple_values flag is set. 

### Steps to Reproduce the issue

Run the code below with no command line parameters. I would expect it to fail, not provide the default.


### Sample Code or Link to Sample Code

````
extern crate clap ;
use self::clap::{Arg, App} ;
fn main() {
    let matches = App::new("mwe")
        .version("0.0.1")
        .arg(Arg::with_name("reverse")
             .short("r")
             .takes_value(false))
        .arg(Arg::with_name("list")
             .takes_value(true)
             .default_value("Breakfast")
             .required_unless_one(&["reverse",]))
        .get_matches() ;
    println!("{:?}",matches)
}
````

### Debug output (lines split to prettify)

````
D:\Development\Rust\mwe>target\debug\mwe
ArgMatches { args: {"list": MatchedArg { occurs: 0, indices: [1], vals: ["Breakfast"] }}, 
subcommand: None, 
usage: Some("USAGE:\n    mwe [FLAGS] <list>...") }

D:\Development\Rust\mwe>target\debug\mwe -r
ArgMatches { args: {"list": MatchedArg { occurs: 0, indices: [2], vals: ["Breakfast"] }, 
"reverse": MatchedArg { occurs: 1, indices: [1], vals: [] }}, 
subcommand: None, 
usage: Some("USAGE:\n    mwe [FLAGS] <list>...") }
````



---

_Comment by @longborough on 2018-03-23 16:34_

I'm so sorry. I've just seen the .default_value_if construct. I've closed this issue for now; if need be, I may open it again.

---

_Closed by @longborough on 2018-03-23 16:34_

---

_Comment by @kbknapp on 2018-03-23 17:13_

No problems! Feel free to re-open if you need to.

---
