---
number: 1783
title: subsubcommand  option q cannot use delimiter
type: issue
state: closed
author: inevity
labels:
  - C-bug
assignees: []
created_at: 2020-04-04T13:02:28Z
updated_at: 2020-04-05T02:33:15Z
url: https://github.com/clap-rs/clap/issues/1783
synced_at: 2026-01-10T01:27:05Z
---

# subsubcommand  option q cannot use delimiter

---

_Issue opened by @inevity on 2020-04-04 13:02_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rustc 1.42.0 (b8cedc004 2020-03-09)

### Code

```rust 
extern crate clap;

use clap::{App, AppSettings, Arg, ArgSettings, ArgMatches, SubCommand};
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let matches = App::new("Share")
        .version("1.0")
        .subcommand(
            App::new("ghost") // The name we call argument with
                .version("0.1") // Subcommands can have independent version
                .setting(AppSettings::SubcommandRequiredElseHelp)
                .subcommand(
                    App::new("list")
                         .about("list posts")
                         .version("0.1")
                         .arg(
                             Arg::with_name("posts") // And their own arguments
                                 .help("list post")
                                 .index(1)
                                 .required(true),
                         )
                         .arg(        
                             Arg::with_name("query")
                                 .short('q')
                                 .long("query")
                                 .multiple(true)
                                 .use_delimiter(true)
                                 .setting(ArgSettings::TakesValue)
                                 .help("set list query args"),         
                         ),
                )
        )
        .get_matches();

    match matches.subcommand() {
        ("ghost", Some(ghost_matches)) => {
            match ghost_matches.subcommand() {
                ("list", Some(list_matches)) => {

                    println!("to list posts/pages { }", list_matches.value_of("posts").unwrap());
                    if let Some(q) = list_matches.value_of("query") {
                    //if let Some(q) = matches.value_of("query") {
                        // why string
                   // if let Some(q) = list_matches.value_of("query").unwrap().collect::<Vec<_>>() {
                        //q = ();

                        println!("list post query {:#?}", q); //mean

                    }
                }
                ("", None) => println!("No ghost subcommand was used"), // If no subcommand was usd it'll match the tuple ("", None)
                _ => unreachable!(),
            }
        }
        ("", None) => println!("No main subcommand was used"), // If no subcommand was usd it'll match the tuple ("", None)
        _ => unreachable!(), // If all subcommands are defined above, anything else is unreachabe!()
    }


    Ok(())
}
Cargo.toml
[package]
name = "sharecli"
version = "0.1.0"
edition = "2018"


[dependencies]
clap = { git = "https://github.com/clap-rs/clap/" }
tokio = { version = "0.2", features = ["full"] }

```

### Steps to reproduce the issue

1. Run `sharec ghost list post -q limit:9,status:draft.`

2. 
```
if let Some(q) = list_matches.value_of("query") { // list_matches got from the matches.subcommand() which is from the second subcommand 
```
### Affected Version of `clap*`

master

### Actual Behavior Summary

q is String, "limit:9", 

### Expected Behavior Summary

Should as array. so we can use list_matches.value_of("query").unwrap().collect::<Vec<_>>()

### Additional context


---

_Label `T: bug` added by @inevity on 2020-04-04 13:02_

---

_Closed by @inevity on 2020-04-05 02:33_

---

_Comment by @inevity on 2020-04-05 02:33_

not bug.

---
